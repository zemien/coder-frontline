---
title: 'Humidity Monitor - Winter of Xamarin 2017'
date: 2017-06-28T23:42:47+12:00
featuredImg: /wp-content/uploads/sites/2/2017/06/mountain-ridge-pathways-snow-hike.jpg
categories:
  - Programming
---
I'm delighted to announce that I won Microsoft's Winter of Xamarin competition this year with my Humidity Monitor App. Here's my submission video:



The sanitised source code (with the keys and connection strings removed) is on [my GitHub repo](https://github.com/zemien/HumidityMonitor). Here's a rough guide on how you can replicate the project and some ideas to extend it further:

# Pre-requisites

  * A [Raspberry Pi 3](https://pishop.nz/RPI3-COMBO-1/) running [Windows 10 IoT](https://developer.microsoft.com/en-us/windows/iot/downloads)
  * [Sense HAT](https://www.pbtech.co.nz/product/SEVRBP0073/Raspberry-Pi-Official-Sense-HAT-with-Orientation-P) module
  * Recommended: [40-pin GPIO ribbon cable](https://pishop.nz/RPI-DUPONT/) to keep some distance between the Pi and the Sense HAT because the Pi's hot running temperature will affect Sense HAT readings
  * (Optional) A Belkin WeMo Smart Switch or alternative smart switch that can connect to IFTTT
  * (Optional) A dehumidifier that can be turned on from the switch (i.e. does not require additional button presses on the device)
  * (Optional) A free If This Then That ([IFTTT](https://ifttt.com)) account
  * Windows 10 PC or laptop is required to build Universal Windows Platform (UWP) apps, which is the platform Windows 10 IoT uses. A Mac OS computer can be used for all other development tasks in this project.
  *  [Visual Studio 2017](https://www.visualstudio.com/downloads/) (Windows or Mac) Community Edition and above. Make sure to choose UWP Development and Mobile Development options when installing.
  * [Syncfusion Essential Studio for Xamarin](https://www.syncfusion.com/products/xamarin): Syncfusion generously offers free [community licenses](https://www.syncfusion.com/products/communitylicense) for individual developers and small organisations.
  * An [Azure](https://azure.microsoft.com/en-us/) subscription. I stick to free tiers where possible. The only thing I am paying for is Azure Table Storage and it costs about $0.01 a month even after my sensor has been uploading data for a month. 
      * Join the [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) program if you don't have an existing Azure subscription because it provides free Azure credits over 12 months, along with other great benefits.
  * Git clone the Humidity Monitor App [source code from GitHub](https://github.com/zemien/HumidityMonitor). Make sure it builds successfully on Visual Studio 2017 before proceeding further. This helps validate you have all the required components.

# Humidity Logger (UWP)

The Humidity Logger is a simple Windows 10 Universal Windows Platform app that reads the relative humidity sensor on the Sense HAT and uploads aggregated data to Azure Table Storage. This app can only run successfully on a Pi with Sense HAT even though you could launch it in any Windows 10 device.

## Azure Table Storage

We first need to create an Azure Table Storage table to load readings in. We will specify three pieces of information - the location of the sensor (e.g. &#8220;Lounge&#8221;) as the Partition Key, the UTC date time it was inserted as the Row Key, and the relative humidity reading as an integer.

The Row Key is inserted as the number of ticks before DateTime.Max. This results in a decrementing number that I call Reverse Ticks in the code. This was done because Azure Table Storage is a simple NoSQL storage solution that doesn't have advanced querying or indexing capabilities. It automatically orders records by Partition Key and Row Key in ascending order, which means the record with the smallest Row Key (i.e. the most recently inserted record) is at the top. This makes it trivial to select the latest reading as I just need to read the first record.

Here's the basic steps to get the Azure Table Storage up and running for the logger:

Log in to the [Azure Portal](https://portal.azure.com) and click on the Add button to go to the Marketplace. Choose Storage account and specify options. I have gone with the cheapest options where possible, including Locally-redundant storage (LRS).

<img class="alignnone wp-image-354 size-large" src="/wp-content/uploads/sites/2/2017/06/01-create-azure-table-storage.png" alt="Create storage account in Azure Portal" width="640" height="403" srcset="/wp-content/uploads/sites/2/2017/06/01-create-azure-table-storage.png 1024w, /wp-content/uploads/sites/2/2017/06/01-create-azure-table-storage.png 300w, /wp-content/uploads/sites/2/2017/06/01-create-azure-table-storage.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

Next, go to Access keys and copy one of the connection strings. We will use this in two places in our code.

<img class="alignnone wp-image-355 size-large" src="/wp-content/uploads/sites/2/2017/06/02-access-key.png" alt="Obtain connection string" width="640" height="373" srcset="/wp-content/uploads/sites/2/2017/06/02-access-key.png 1024w, /wp-content/uploads/sites/2/2017/06/02-access-key.png 300w, /wp-content/uploads/sites/2/2017/06/02-access-key.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

## Add Connection Strings

The first place to add that Azure Table Storage connection string is in HumidityLogger\MainPage.xaml.cs in the SetUpAndLaunch() method:

```csharp
actions.Add(new AverageHumidityAction(this, new TableStorageHumidityLogger("TODO - Location code",
    "TODO - Azure table storage connection string", "HumidityLog")));
```

Also note the need for a Location code. This is used as the Partition Key in the table storage so keep it succinct, e.g. &#8220;Lounge&#8221;.

## Deploy to Raspberry Pi

I find debugging the application is the easiest way to deploy UWP apps onto the Pi. Once that's done, go into the Pi's [Windows Device Portal](https://developer.microsoft.com/en-us/windows/iot/docs/deviceportal) and make it the default app to run on startup.

# Humidity Monitor API

I've decided to create a .Net Core Web API from scratch, as opposed to using the Xamarin project template's option to generate a Web API backend. I've kept the API very simple with only a few supported actions. The main logic revolves around converting DateTime requests to and from the ReverseTicks structure used as the Row Key. Unit tests validate the conversion logic.

## Add Connection Strings

Paste the Azure Table Storage connection string from earlier into the HumidityController's constructor:

```csharp
public HumidityController()
{
    var storageAccount = CloudStorageAccount.Parse("TODO - Cloud table connection string");
    _client = storageAccount.CreateCloudTableClient();
    _tableName = "HumidityLog";
    _table = _client.GetTableReference(_tableName);
}
```

## Publish to Azure App Service

Go back to the Azure Portal and add a new App Service. Make sure to pick the Web App option, and not the Web App + SQL option.

<img class="alignnone wp-image-358 size-large" src="/wp-content/uploads/sites/2/2017/06/05-add-app-service.png" alt="Add a new Azure Portal App Service" width="640" height="380" srcset="/wp-content/uploads/sites/2/2017/06/05-add-app-service.png 1024w, /wp-content/uploads/sites/2/2017/06/05-add-app-service.png 300w, /wp-content/uploads/sites/2/2017/06/05-add-app-service.png 768w, /wp-content/uploads/sites/2/2017/06/05-add-app-service.png 800w, /wp-content/uploads/sites/2/2017/06/05-add-app-service.png 1338w" sizes="(max-width: 640px) 100vw, 640px" />

Specify the App Service options. You may need to create new resource groups and app service plans here. Make sure to create a free App Service Plan to keep costs low. Turn on Application Insights.

Note the App name forms the URL of the API, e.g. humidity-monitor.azurewebsite.net for the example below:

<img class="alignnone size-large wp-image-359" src="/wp-content/uploads/sites/2/2017/06/06-app-service-options.png" alt="Specify app service options" width="640" height="421" srcset="/wp-content/uploads/sites/2/2017/06/06-app-service-options.png 1024w, /wp-content/uploads/sites/2/2017/06/06-app-service-options.png 300w, /wp-content/uploads/sites/2/2017/06/06-app-service-options.png 768w" sizes="(max-width: 640px) 100vw, 640px" />

Head back to Visual Studio and use the Publish to Azure option. The UI looks a bit different in Windows but the general flow remains the same.

 <img class="alignnone size-medium wp-image-356" src="/wp-content/uploads/sites/2/2017/06/03-publish-to-azure-failure.png" alt="Right-click project and go to Publish > Publish to Azure" width="300" height="129" srcset="/wp-content/uploads/sites/2/2017/06/03-publish-to-azure-failure.png 300w, /wp-content/uploads/sites/2/2017/06/03-publish-to-azure-failure.png 768w, /wp-content/uploads/sites/2/2017/06/03-publish-to-azure-failure.png 898w" sizes="(max-width: 300px) 100vw, 300px" /><img class="alignnone size-large wp-image-360" src="/wp-content/uploads/sites/2/2017/06/07-publish-to-azure.png" alt="Publish to Azure App Service" width="640" height="431" srcset="/wp-content/uploads/sites/2/2017/06/07-publish-to-azure.png 1024w, /wp-content/uploads/sites/2/2017/06/07-publish-to-azure.png 300w, /wp-content/uploads/sites/2/2017/06/07-publish-to-azure.png 768w, /wp-content/uploads/sites/2/2017/06/07-publish-to-azure.png 1398w" sizes="(max-width: 640px) 100vw, 640px" />

Visual Studio 2017 has a [known bug](https://developercommunity.visualstudio.com/content/problem/29118/visual-studio-2017-does-not-show-azure-subscriptio.html) where it doesn't detect your subscription. Please try the workarounds in that link if you encounter it.

# Humidity Monitor App

The mobile app is built on Xamarin.Forms cross-platform framework using MVVM pattern. I've tried to keep everything within the Forms project and avoid platform-specific customisations where possible. I created the project on my Mac, which doesn't create a UWP variant. However there is no reason why it can't run on Windows 10 devices as well.

Anyone can create self-signed Android apps for self-distribution, but iOS apps require a developer account to generate valid certificates.

## Configure the App

The HumidityMonitorAppViewModel is the main view model driving the mobile app. It has a few important configurations to add in the class fields:

  * _location: The location code (i.e. Partition Key)
  * TIMEZONE: The [IANA time zone code according to NodaTime](https://gist.github.com/jrolstad/5ca7d78dbfe182d7c1be)
  * _dal: Requires the base URL of the Humidity Monitor API deployed onto the App Service, e.g. http://humidity-monitor.azurewebsites.net
  * (Optional) _switchDal: The IFTTT Maker Webhook key that you obtain from your IFTTT account

```csharp
public class HumidityMonitorAppViewModel : ViewModelBase
{
    string _location = "TODO - Location";
    private const string TIMEZONE = "TODO - IANA time zone code";
    HumidityViewModel _latestHumidity;
    ObservableCollection<HumidityViewModel> _detailedHumidityCollection;
    ObservableCollection<HumiditySummaryViewModel> _humiditySummaryCollection;
    DateTime _detailedHumidityDate;
    DateTime _humiditySummaryStartDate;
    DateTime _humiditySummaryEndDate;
    bool _canGetLatestHumidity;
    bool _isLoading;
    bool _viewSwitch;
    ViewEnum _currentView;
    readonly HumidityMonitorDAL _dal = new HumidityMonitorDAL("TODO - Backend API base URI");
    readonly IftttDehumidifierSwitchDAL _switchDal = new IftttDehumidifierSwitchDAL("TODO - IFTTT Maker Webhook Key");
    //... more code here ...
}
```

## Create Android Signing Certificate

Follow [Xamarin's guide to generate a self-signed Android certificate](https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/publishing_an_application/part_2_-_signing_the_android_application_package/). This is mandatory for the next step.

## CI/CD with Azure Mobile Center

This is an optional step but one that made my testing life easier because it automatically builds and distributes my Android app on every Git push. The [Azure Mobile Center](https://mobile.azure.com) is currently in preview and is free to use. It hooks into your Azure account so make sure to sign in with your existing Azure credentials. There are many guides out there on how to configure CI/CD pipeline with the Mobile Center.

# Next Steps for Improvement

Here are some suggested improvements that you can try yourself:

  * The Sense HAT has more sensors. Incorporate readings from different sensors, e.g. temperature and pressure, to derive further insights.
  * Improve security of the API because you wouldn't want everyone to be able to see your humidity readings.
  * Incorporate the IANA time zone code into the Table Storage so it doesn't need to be specified on every API call.
  * Make the location code configurable in the Humidity Logger UI and Xamarin app so that it can easily support different locations without changing hardcoded strings. The Xamarin app can then be extended to view records from different locations from a drop-down.
  * Create configurable notifications that will alert you if the humidity breaches a threshold. This will require building a background service that polls the API at specific intervals. A fancier solution is to do the monitoring on the backend (Azure Functions, maybe?) and send a push notification to the user.

# Conclusion

The Humidity Monitor App has many components but I've done most of the hard work. All you need is to configure some Azure resources and you're done. I've also left enough gaps in the mobile app for you want to take this further. Enjoy!