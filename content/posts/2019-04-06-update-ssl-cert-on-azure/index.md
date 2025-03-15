---
title: Update SSL Cert on Azure App Service
date: 2019-04-06T09:00:00+13:00
featuredImg: /update-ssl-cert-on-azure-app-service/old_legal_document.jpg
categories:
  - Programming
---

There are so many different tutorials on how to provision and install an SSL/TLS certificate on Azure App Services that I thought I'd write one that worked for me. I added some indicative timelines so you know how long each step may take. As a rule I recommend starting this process at least a week before the current certificate will expire.

# Pre-requisites
Find a different tutorial if you don't have these exact components because your results may vary:

1. Windows computer (Windows 10 or newer, or a server edition)
2. [DigiCert Certificate Utility](https://www.digicert.com/util/)
3. Azure App Service that requires a SSL certificate to be updated
4. SSL certificate available to purchase
5. Access to domain name DNS settings

# Create certificate
## Generate CSR (5 minutes)

1. Open the DigiCert Certificate Utility and [follow the official steps to generate a CSR](https://www.digicert.com/util/csr-creation-microsoft-servers-using-digicert-utility.htm)
2. Remember to put a wildcard URL if you are purchasing one.
3. The important thing is you don't need to generate the CSR on the server that you're going to install it on.

## Purchase certificate using CSR (10 minutes)

1. Purchase the certificate with the CSR generated from DigiCert Certificate Utility. This step is largely similar with all certificate sellers.

## Verify domain ownership via DNS (up to 1 hour)

1. As part of the certificate generation process you will need to verify domain name ownership. I recommend using the DNS method, where you add a TXT record with a provided string. This part may take an hour or more to propagate, but is usually faster.

## Download certificate (5 minutes)

1. The SSL provider will e-mail the certificate in text format.
2. Copy the certificate text into a text editor and save it with a .cer extension.

## Convert certificate into .pfx (10 minutes)

1. Open the DigiCert Certificate Utility on the same server you generated the CSR.
2. Import the .cer file into the server following the [installation instructions](https://www.digicert.com/util/ssl-certificate-installation-using-digicert-utility-for-microsoft-servers.htm).
3. Next, export it as a .pfx file following the [export instructions](https://www.digicert.com/util/pfx-certificate-management-utility-import-export-instructions.htm).
4. Store the password for the .pfx in a secure manner.

# Install certificate on Azure App Service

## Upload PFX certificate to Azure (5 minutes)

1. Go to Azure Portal and access one of the App Services you wish to update. Go to the SSL settings blade.
2. Click on Private Certificate (.pfx) tab.
3. Click on Upload Certificate button.
4. Select the .pfx file exported from DigiCert Certificate Utility.
5. Enter the password used to secure the .pfx.
6. Click on Upload. This should take a few seconds. If the notifications say that it is still uploading after a minute, refresh the Portal and upload again.

{{< 
  figure src="azure-upload-pfx.png" 
  title="Upload PFX certificate to Azure" 
  alt="Screenshot showing labeled steps to upload PFX certificate to Azure" 
>}}

## Assign certificate to SSL Binding (5+ minutes)

1. Go back in the Bindings tab.
2. Scroll down to SSL Bindings section and update each binding with the new certificate.
3. Browse to the site and confirm the new certificate is being used.

Refer to [Microsoft documentation](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-tutorial-custom-SSL#bind-your-ssl-certificate) because the Azure UI changes often.

I hope this helps someone! Probably myself, in a year's time.

Background photo credit: <a href="https://visualhunt.co/a1/5fdb850b">University of Glasgow Library</a> on <a href="https://visualhunt.com/re3/1b6991c0">Visual Hunt</a> / <a href="http://creativecommons.org/licenses/by-nc-sa/2.0/"> CC BY-NC-SA</a>