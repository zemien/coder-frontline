---
title: TLS 1.2 support on .Net 4.5
date: 2019-07-19T14:00:00+12:00
categories:
  - Programming
---

Part of my job involves supporting legacy software running on older .Net Frameworks. There was a period of upheaval last year when support for TLS 1.0 was switched off by many payment processing gateways. A lot of online guides use this simple trick to force .Net 4.5 application to use TLS 1.2:

```csharp
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
```

This turns out to be a bad move because the code will only support TLS 1.2, while older services may still need TLS 1.1 or 1.0. I found another code snippet that adds TLS 1.2 to the list of supported protocols:

```csharp
ServicePointManager.SecurityProtocol |= SecurityProtocolType.Tls11 | SecurityProtocolType.Tls12;
```

This approach didn't work for me because the older OS I was on used TLS 1.0 as the default protocol.

I finally found a comprehensive (if a little confusing) guide in the official Microsoft docs on [Transport Layer Security (TLS) best practices with the .NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls). The solution for my particular case is to make a Registry tweak to enforce and prefer secure protocols.

Taken from that page, here's a snippet which can be saved as a .reg file and run on the server. Please be careful when making changes to the Registry, and it is recommended to revisit the [official documentation](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls) for latest updates.

```registry
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001
```