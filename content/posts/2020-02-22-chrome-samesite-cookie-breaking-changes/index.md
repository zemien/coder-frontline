---
title: Chrome's SameSite Cookie Changes are Breaking Apps
date: 2020-02-22T21:36:00+13:00
featuredImg: /chromes-samesite-cookie-changes-are-breaking-apps/food-man-person-eating.jpg
categories:
  - Programming
---

----

**Updated 8 April 2020** with alternative regex if your OS or framework or app automatically adds the SameSite=Lax header to your session cookies. It is important to double check what your app is emitting so you can tailor the regex to suit!

----

I just spent a good 6 hours of my life trying to debug a weird web app issue that I finally pinned down to the SameSite cookie attribute changes spearheaded by Google. The change has been signalled for months but I had ignored it, thinking it's only going to affect niche scenarios that will be updated by other packages. In fact I think this will slowly grow to have the same impact as the TLS 1.0 shut off in 2018 in terms of how many e-commerce website owners are caught out by it.

This was the buggy behaviour I noticed:

1. Users initiated the checkout process
2. Users were taken to a third-party payment gateway to enter their card details
3. The payment gateway sends the user back to the site
4. The web server backend could not retrieve the session cookie, thus treating it as a new user session. This meant that it was unable to retrieve the shopping cart to finalize the order.

I will cover the key points but send you off to more informative sites where necessary. I will then list some mitigating approaches for ASP .Net, my web framework of choice.

## Investigation

You need to pay attention to the SameSite changes if your website or application:

- Uses cookies for website functionality. There may be important cookies added implicitly by the framework or imported components. Common ones include cookies that identify session and user preferences.
- Connects to a third-party service, which includes showing their content. Common examples are authentication providers and payment gateways.
- Can be served over non-secure HTTP protocol

In my case, the web application was using ASP.NET_SessionId session cookies to keep track of the user's details and shopping cart contents. It was also using the Paymark Click hosted payment gateway which [Posts to Return URL after payment details are submitted](http://docs.dev.paymark.nz/click/#header-two-return-options). No SameSite option was set on the ASP.NET_SessionId cookie, so it was treated as "Lax" by default. This meant the cookie was not included when the Paymark Click page sends the POST to the return URL.

## Resolution and Mitigation

I highly recommend spending a fruitful 15 minutes reading Rowan's [simple explanation of the SameSite attribute](https://web.dev/samesite-cookies-explained/). Start there before implementing any solutions you find so that you understand why you're doing it. You can also review [.Net-specific information in Barry's blog post](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) announcing the changes.

In my case, my session state cookie needed to have both `secure` and `SameSite=None` headers. Luckily my site is HTTPS-only so that solves the secure part, but what about the None header?

### ASP .Net 4.7.2 and above, and .Net Core 2.1 and above

Cookies have a [SameSite property](https://docs.microsoft.com/en-us/dotnet/api/system.web.httpcookie.samesite) which can be set to one of three enum values (None, Lax, Strict) according to your needs. However, the [default behaviour for "None"](https://docs.microsoft.com/en-us/dotnet/api/system.web.samesitemode) varies if you did not specify a value. In the past it would not emit any SameSite attribute, but recent Windows patches will change it to emit the SameSite=None cookie header. More importantly, FormsAuth and SessionState cookies will be issued with `SameSite=Lax`, which still wouldn't work properly in my encountered scenario. The [Azure App Service was updated with these changes](https://azure.microsoft.com/en-us/updates/app-service-samesite-cookie-update/) in January 2020.

One approach is to add the `cookieSameSite="None"` attribute to your web.config:

```xml
<sessionState mode="StateServer" cookieSameSite="None" cookieless="false" timeout="20" />
```

### ASP .Net before 4.7.2

The online shopping site I was supporting is built on .Net 4.5.1 which meant that it did not have the same attribute I could specify. Instead, I am using this URL Rewrite (modified off a couple of StackOverlow answers [here](https://stackoverflow.com/a/59281131) and [here](https://stackoverflow.com/a/59300799/824036)) that applies `SameSite=None` to ASP .Net session cookies regardless if there's one:

**IMPORTANT!** The following solutions require the [URL Rewrite IIS extension](https://www.iis.net/downloads/microsoft/url-rewrite).

```xml
<rewrite>
  <outboundRules>
    <rule name="SessionCookieAddNoneHeader">
      <match serverVariable="RESPONSE_Set-Cookie" pattern="(.*ASP.NET_SessionId.*)" />
      <!-- Use this regex if your OS/framework/app adds SameSite=Lax automatically to the end of the cookie -->
      <!-- <match serverVariable="RESPONSE_Set-Cookie" pattern="((.*)(ASP.NET_SessionId)(=.*))(?=SameSite)" /> -->
      <action type="Rewrite" value="{R:1}; SameSite=None" />
    </rule>
  </outboundRules>
</rewrite>
```

## User Agent Sniffing

There is a huge caveat to the above approaches. Some older browsers do not recognize the None option, and might reject it outright, ignore the header, or treat it as "Strict". All outcomes will likely cause more problems. Check out Chromium's post on [known incompatible clients](https://www.chromium.org/updates/same-site/incompatible-clients) for the latest list and UserAgent detection pseudo-code. The web.config solutions I shared above do not have this User Agent sniffing capability, which means it will break the site on affected browsers.

My best solution right now for existing production sites follows. I expanded my IIS URL Rewrite rule to remove `SameSite=None` header from most incompatible browsers. Kudos to CatchJS for doing real-world testing as documented in their blog: [User-Agent Sniffing Only Way to Deal With Upcoming SameSite Cookie Changes
](https://catchjs.com/Blog/SameSiteCookies). I modified their detection approach into a URL Rewrite pre-condition:

```xml
<rewrite>
  <outboundRules>
    <preConditions>
      <!-- Checks User Agent to identify browsers incompatible with SameSite=None -->
      <preCondition name="IncompatibleWithSameSiteNone" logicalGrouping="MatchAny">
        <add input="{HTTP_USER_AGENT}" pattern="(CPU iPhone OS 12)|(iPad; CPU OS 12)" />
        <add input="{HTTP_USER_AGENT}" pattern="(Chrome/5)|(Chrome/6)" />
        <add input="{HTTP_USER_AGENT}" pattern="( OS X 10_14).*(Version/).*((Safari)|(KHTML, like Gecko)$)" />
      </preCondition>
    </preConditions>

    <!-- Adds or changes SameSite to None for the session cookie -->
    <!-- Note that secure header is also required by Chrome and should not be added here -->
    <rule name="SessionCookieAddNoneHeader">
      <match serverVariable="RESPONSE_Set-Cookie" pattern="(.*ASP.NET_SessionId.*)" />
      <!-- Use this regex if your OS/framework/app adds SameSite=Lax automatically to the end of the cookie -->
      <!-- <match serverVariable="RESPONSE_Set-Cookie" pattern="((.*)(ASP.NET_SessionId)(=.*))(?=SameSite)" /> -->
      <action type="Rewrite" value="{R:1}; SameSite=None" />
    </rule>

    <!-- Removes SameSite=None header from all cookies, for most incompatible browsers -->
    <rule name="CookieRemoveSameSiteNone" preCondition="IncompatibleWithSameSiteNone">
      <match serverVariable="RESPONSE_Set-Cookie" pattern="(.*)(SameSite=None)" />
      <action type="Rewrite" value="{R:1}" />
    </rule>
  </outboundRules>
</rewrite>
```

## HTTP Caveat

Chrome will only accept `SameSite=None` if it is paired with the `secure` header. Adding that header is not covered in this article, and can usually be added in via different code and config approaches. The `secure` header also means that the cookie must be served over HTTPS, so make sure your website handles any HTTP redirection as required.