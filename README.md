# frida-rootbypass-and-sslunpinning-lg-thinq
Frida Script to bypass root detection and SSL Unpinning for mitm proxy listening for the LG ThinQ Android App

I needed to inspect some of the commands sent over the LG ThinQ API, but any of the common SSL unpinning scripts triggered the LG app's root detection. 

Used with Windows Subsystem for Android (WSA) On Windows 11 - Rooted w/ Magisk & Google Play Store/Services 

 - [(See here for more info to setup)](https://github.com/LSPosed/MagiskOnWSALocal)

This script successfully bypasses the root detection and allows for the SSL unpinning to occur, so the mitmproxy (or similar) can be used to view information sent to the API. 

## Example Usage (With WSA)

- Connect with ADB
  - `adb connect 127.0.0.1:58526`
>```shell
>C:\Users\USERNAME>adb connect 127.0.0.1:58526
>connected to 127.0.0.1:58526
>```
- Modify Proxy Settings
  - `adb shell am start -a android.intent.action.MAIN -n com.android.settings/.Settings`
>```shell
>C:\Users\USERNAME>adb shell am start -a android.intent.action.MAIN -n com.android.settings/.Settings
>Starting: Intent { act=android.intent.action.MAIN cmp=com.android.settings/.Settings }
>```
  - Go to network settings, and modify VirtWifi network and set Proxy to your `Ethernet adapter vEthernet (WSLCore)` interface IP (you can find this in powershell with `ipconfig` command) and port 8080 (or other port set)
- Launch app with Frida script
  - `frida -l frida-rootbypass-and-sslunpinning-lg-thinq.js -U -f com.lgeha.nuts`
```shell
```C:\Users\USERNAME\Downloads>frida -l frida-rootbypass-and-sslunpinning-lg-thinq.js -U -f com.lgeha.nuts
     ____
    / _  |   Frida 16.1.3 - A world-class dynamic instrumentation toolkit
   | (_| |
    > _  |   Commands:
   /_/ |_|       help      -> Displays the help system
   . . . .       object?   -> Display information about 'object'
   . . . .       exit/quit -> Exit
   . . . .
   . . . .   More info at https://frida.re/docs/home/
   . . . .
   . . . .   Connected to Pixel 5 (id=127.0.0.1:58526)
Spawned `com.lgeha.nuts`. Resuming main thread!
[Pixel 5::com.lgeha.nuts ]-> [+] SSLPeerUnverifiedException auto-patcher
[+] HttpsURLConnection (setDefaultHostnameVerifier)
[+] HttpsURLConnection (setSSLSocketFactory)
[+] HttpsURLConnection (setHostnameVerifier)
[+] SSLContext
[+] TrustManagerImpl
[+] OkHTTPv3 (list)
[ ] OkHTTPv3 (cert)
[+] OkHTTPv3 (cert array)
[ ] OkHTTPv3 ($okhttp)
[ ] Trustkit OkHostnameVerifier(SSLSession)
[ ] Trustkit OkHostnameVerifier(cert)
[ ] Trustkit PinningTrustManager
[ ] Appcelerator PinningTrustManager
[ ] OpenSSLSocketImpl Conscrypt
[ ] OpenSSLEngineSocketImpl Conscrypt
[ ] OpenSSLSocketImpl Apache Harmony
[ ] PhoneGap sslCertificateChecker
[ ] IBM MobileFirst pinTrustedCertificatePublicKey (string)
[ ] IBM MobileFirst pinTrustedCertificatePublicKey (string array)
[ ] IBM WorkLight HostNameVerifierWithCertificatePinning (SSLSocket)
[ ] IBM WorkLight HostNameVerifierWithCertificatePinning (cert)
[ ] IBM WorkLight HostNameVerifierWithCertificatePinning (string string)
[ ] IBM WorkLight HostNameVerifierWithCertificatePinning (SSLSession)
[ ] Conscrypt CertPinManager
[ ] CWAC-Netsecurity CertPinManager
[ ] Worklight Androidgap WLCertificatePinningPlugin
[ ] Netty FingerprintTrustManagerFactory
[ ] Squareup CertificatePinner (cert)
[ ] Squareup CertificatePinner (list)
[ ] Squareup OkHostnameVerifier (cert)
[ ] Squareup OkHostnameVerifier (SSLSession)
[+] Android WebViewClient (SslErrorHandler)
[ ] Android WebViewClient (WebResourceError)
[ ] Apache Cordova WebViewClient
[ ] Boye AbstractVerifier
[ ] Appmattus (CertificateTransparencyInterceptor)
[ ] Appmattus (CertificateTransparencyTrustManager)
Unpinning setup completed
---
Security check bypassed, reporting NOT_ROOTED.
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing OkHTTPv3 (list): route.lgthinq.com
  --> Bypassing Trustmanager (Android < 7) request
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing OkHTTPv3 (list): aic-service.lgthinq.com
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing HttpsURLConnection (setDefaultHostnameVerifier)
  --> Bypassing OkHTTPv3 (list): noti.lgthinq.com
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing OkHTTPv3 (list): common.lgthinq.com
  --> Bypassing TrustManagerImpl checkTrusted
  --> Bypassing OkHTTPv3 (list): aic-common.lgthinq.com
```

SSL Unpinning formed from https://github.com/httptoolkit/frida-android-unpinning/
