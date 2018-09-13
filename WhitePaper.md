# Title

| Abstract |
|----------|
| Text. |

| Keywords |
|----------|
| Traffic interception, traffic analysis, online tracking, network monitoring, network forensics, sniffers. |

| Table of Contents |
|-------------------|
| [Introduction](#introduction) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Related Work](#related-work) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Criteria](#criteria) |
| [Tracking Options](#tracking-options) |
| &nbsp;&nbsp;&nbsp;&nbsp;[VPN](#vpn) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Browser based](#browser-based) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Activity Logger](#activity-logger) |
| [Discussion](#discussion) |

## Introduction

To study the role of news consumption in people's opinion reliable research data is needed. Most research so far focuses on survey data and desktop applications. To collect data on news consumption we need to take into account: legal (GDPR) constraints, user experience, and technical feasibility. In this paper we aim to explore what technologies are available and to what extend they meet these requirements.

### Related Work

Text.

### Criteria

**Technologically viable:**
- Works on mobile
- Works with HTTPS
- Deals with in-app browsing
- Tracks only whitelisted sites
- Tracks full url
- Can be build in a < two month project
- Easy to maintain, despite evolutions in mobile phone and web technologies

**Legal and GDPR compliant:**
- No data storage without explicit permission from participant
- ... (note: expand on this with specific critria)

**Non-intrusive and scalable:**
- Small time investment for participant and researcher at the start and end
- Simple to setup for participant
- Good overview of what information is being share
- No action required by participant in daily life situation
- No impact on normal use of phone/PC: e.g. no apps that drain the battery.

## Tracking Options

Note: I think a general introduction about how internet traffic works would help, because this will provide the reader with no expert knowledge in this area with a framework.

### VPN

Sniffers are software meant to expose network interactions, including HTTP or HTTPS requests and responses [(Blog about sniffers)](https://stanfy.com/blog/monitor-mobile-app-traffic-with-sniffers/). They are originally meant for developers to debug their software. Well known applications for sniffing HTTP(S) traffic include [Fiddler](https://www.telerik.com/fiddler), [Charles Proxy](https://www.charlesproxy.com/documentation/proxying/ssl-proxying/), TCP Catcher and Burp Suite. Such apps set up a proxy server, to which the mobile device connects. The sniffer intercepts the HTTP requests and responses. In order to be able to see and manipulate HTTPS requests you need to perform a Man-in-the-Middle (MitM) attack. At the proxy, traffic is decrypted, stored and subsequently re-encrypted to be send to the mobile device. The downside of this option is that the mobile device will see that the signature on the encryption is different than that of the original encryption. To prevent the mobile device from raising an error, the proxy needs to sign the data with its own certificate, which needs to be registered on the mobile device. This requires installing a certificate. Even then, global certificate store on mobile untrusted by apps. To work around this, update the certificate in the Apps' APK. Easy, but illegal. Also, need to redistribute the modified APK, which is also not allowed under Facebooks license.

 - [Blog about sniffers](https://stanfy.com/blog/monitor-mobile-app-traffic-with-sniffers/).
 - [Fiddler sniffer app](https://www.telerik.com/fiddler).
 - [MitM app](https://mitmproxy.org/).
 - [Charles Proxy sniffer app](https://www.charlesproxy.com/documentation/proxying/ssl-proxying/).
 - [Security SE about HTTPS](https://security.stackexchange.com/questions/153440/intercepting-android-app-traffic-with-burp).
 - [3G/4G does not allow proxy](https://medium.com/@rotxed/how-to-debug-http-s-traffic-on-android-7fbe5d2a34)
 - [Key pinning to protext MitM](https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning).
 - [idem](https://www.nowsecure.com/blog/2017/06/15/certificate-pinning-for-android-and-ios-mobile-man-in-the-middle-attack-prevention).
 - [Changes to Trusted Certificate Authorities in Android Nougat](https://android-developers.googleblog.com/2016/07/changes-to-trusted-certificate.html).
 - [Whom You Gonna Trust? A Longitudinal Study on TLS Notary Service](https://www.sba-research.org/wp-content/uploads/publications/TLSnotaries_preprint.pdf).
 - [HTTPS traffic analysis and client identification using passive SSL/TLS fingerprinting](https://jis-eurasipjournals.springeropen.com/track/pdf/10.1186/s13635-016-0030-7).

| Pro | Con |
|-----|-----|
| Easy to set up with existing VPN Apps in the playstore + openVPN or so at server | https hides the url, only server name (ie. DNS records) available. |
| will get all traffic (Apps + browser) | |
| catches all HTTP | All major websites and browsers consider this 'unsafe' and are starting to refuse visiting sites in http.   |



Evaluation against criteria:

| Criteria | Assessment |
|-----|-----|
| Works on mobile | Yes |
| Works with HTTPS | Yes |
| Deals with in-app browsing | Yes |
| Tracks only whitelisted sites | No, but possible |
| Tracks full url | No |
| Can be build in a < 2 month project | ? |
| Easy to maintain, despite evolutions in mobile phone and web technologies | ? |
| ... insert criteria related to GDPR | ... |
| Small time investment for participant and researcher at the start and end | Yes |
| Simple to setup for participant | ? |
| Good overview of what information is being share | possibly |
| No action required by participant in daily life situation | Yes |
| No impact on normal use of phone/PC: e.g. no apps that drain the battery. | Yes? |


### Browser Based

Desktop browser plugins can be used to track browsing history (possible for both Chrome and Firefox). Using Google Sync, or other types of syncronisation, the mobile history can be exported to Desktop.
Facebook: in-app browsing uitzetten => dan browser history opvragen > possible in full FB app, not in FBlite, but does not seem to sync consistently (perhaps only after 5 seconds of viewing, which might even be a good thing) > what happens after update? > disable automatic updates? > FB also has a Download your information option (which includes advertisers_you've_interacted_with).

An alternative method is to create our own browser, which is basically a forwarding app which opens Chrome for us [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d). An app which presents itself as a browser, logs the url and then opens the real default browser so the user notices nothing.

 - [Download Chrome settings](https://android.stackexchange.com/questions/117288/how-can-i-find-my-chrome-bookmarks-from-windows/117334).
 - [Google MyActivity](https://myactivity.google.com/myactivity).
 - [SE on Disable in-app browsing](https://android.stackexchange.com/questions/201150/disable-all-in-app-browsing-while-keeping-chrome-as-defaut-browser).
 - [History of in-app browsing](https://www.addthis.com/blog/2017/04/04/in-app-browsers-explained/).
 - [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d).
 - [Either disable in-app browsing per app or disable WebView on the entire phone](https://www.quora.com/How-do-I-turn-off-Android-WebView-in-apps).

Text.

| Pro | Con |
|-----|-----|
| Reason 1 | Reason 2 |
| Reason 3 | Reason 4 |

| Criteria | Assessment |
|-----|-----|
| Works on mobile | Yes |
| Works with HTTPS | Yes |
| Deals with in-app browsing | Yes |
| Tracks only whitelisted sites | No, but possible |
| Tracks full url | Yes |
| Can be build in a < 2 month project | Yes |
| Easy to maintain, despite evolutions in mobile phone and web technologies | Yes |
| ... insert criteria related to GDPR | ... |
| Small time investment for participant and researcher at the start and end | Yes |
| Simple to setup for participant | Yes |
| Good overview of what information is being share | No, but possible |
| No action required by participant in daily life situation | Yes |
| No impact on normal use of phone/PC: e.g. no apps that drain the battery. | Yes |

### Activity Logger

GDPR request via email (e.g. sanoma or NLprofiel and using the cookies of the users to identify them) or directly download data via special webpage (at Google this is [TakeOut](http://takeout.google.com) which gives URL + timestamp + deviceid). I verified that signing into Chrome on mobile and on Desktop (and enabling sync) gives the browser history on Google Takeout. It includes a client_id and a time_usec. In-app browser history is available in full chrome app, but does not sync. Note that facebook / twitter do not allow analytics to happen on their website (except via their specific tools). How does this interact with our writing a plugin? Note also that Chrome Custom Tabs does not forward its browser history to Google Takeout.

 - [Disable all in-app browsing while keeping Chrome as default browser](https://android.stackexchange.com/questions/201150/disable-all-in-app-browsing-while-keeping-chrome-as-default-browser)
 - [Disable in-app browsing on the Facebook Lite app](https://android.stackexchange.com/questions/201210/how-can-in-app-browsing-be-disabled-on-the-facebook-lite-app)
 - [Include in-app browsing history in Chrome sync](https://android.stackexchange.com/questions/201204/include-in-app-browsing-history-in-chrome-sync)

| Pro | Con |
|-----|-----|
| Reason 1 | Reason 2 |
| Reason 3 | Reason 4 |

(to do: Do criteria evaluation separately for email and google take out).

## Discussion

Text.
