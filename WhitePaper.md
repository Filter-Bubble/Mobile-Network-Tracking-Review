# Technologies for Monitoring News Consumption

| Abstract |
|----------|
| This whitepaper explores what technologies are available for monitoring (mobile) news consumption and to what extend they meet certain requirements. |

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
| &nbsp;&nbsp;&nbsp;&nbsp;[Browser Based with Mobile Sync](#browser-based-with-mobile-sync) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Request data from Browser/App company](#request-data-from-browser-app-company) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Create own mobile browser via "Custom Tabs"](#create-own-mobile-browser-via-custom-tabs) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Request data from News company](#request-data-from-news-company) |
| [Method comparison](#method-comparison) |
| [Discussion](#discussion) |

## Introduction

The news we read online may influence our opinions and beliefs. However, little is known about how much we live in news bubbles that filter the news based on our personal characteristics and interests, and the effect such bubbles may have on our opinions and beliefs.

To study this effect, reliable news consumption data is needed. Most research so far focuses on self-reported survey data and on tracking desktop activity (_insert references_). However, news consumption via mobile devices is expected to be ever more important (_insert references_).

To collect data about mobile news consumption, a couple of important criteria need to be taken into account. These will be discussed in the [Criteria](#criteria) section. Based on our criteria, we will explore what technologies are available for monitoring mobile news consumption and to what extend these meet the requirements.

### Related Work

Previous study that explored technologies for news consumption looked at... (_to be completed_)

### Criteria

This section lists several criteria by which to judge technologies for monitoring mobile news consumption.

**Technological viability:**
- Collects mobile browsing data
- Can track full HTTPS and HTTP urls
- Deals with in-app browsing
- Tracks only whitelisted websites
- Can be built in a short amount of time
- Easy to maintain, despite evolutions in mobile phone and web technologies
- Data can be collected over long periods of time (multiple months)

**Legality and GDPR compliance:**

It is important to remember that any approach can, in principle, be make GDPR compliant by using secure storage, filtering of data and obtaining explicit approval from the participant. The real question here is  whether the approach is GDPR compliant by default or easy to make compliant.

- No data storage without explicit permission from participant
- Does not store contact information: telephone number or email address.
- Does not store the social media account name.
- Does not store information on the specific location of the participant, e.g. ip address, GPS coordinates, or physical address.

**Non-intrusiveness and scalability:**
- Small time investment for researcher. (to do: define this more clearly)
- Small time investment and simple to setup for participant.
- Good overview to participant of what information is being shared.
- Minimal action required by participant in daily life situation, e.g. we do not want the participant to have keep a diary of what news they consume.
- No impact on normal use of phone/PC: e.g. we do not want apps that rapidly drain the phone battery.

## Tracking Options

*Note (Vincent): I think a general introduction about how internet traffic works would help, because this will provide a conceptual framework for the reader without expert knowledge in this area. For example, does our audience know what a proxy server is, what we mean by network interactions, what do we mean by illegal (national law? facebook rules? both?)*

### VPN

Sniffers are software meant to expose network interactions, including HTTP or HTTPS requests and responses [(Blog about sniffers)](https://stanfy.com/blog/monitor-mobile-app-traffic-with-sniffers/). They are originally meant for developers to debug their software. Well known applications for sniffing HTTP(S) traffic include [Fiddler](https://www.telerik.com/fiddler), [Charles Proxy](https://www.charlesproxy.com/documentation/proxying/ssl-proxying/), TCP Catcher and Burp Suite. Such apps set up a proxy server, to which the mobile device connects. The sniffer intercepts the HTTP requests and responses. In order to be able to see and manipulate HTTPS requests you need to perform a Man-in-the-Middle (MitM) attack. At the proxy, traffic is decrypted, stored and subsequently re-encrypted to be send to the mobile device. The downside of this option is that the mobile device will see that the signature on the encryption is different than that of the original encryption. To prevent the mobile device from raising an error, the proxy needs to sign the data with its own certificate, which needs to be registered on the mobile device. This requires installing a certificate. Even then, global certificate store on mobile untrusted by apps. To work around this, update the certificate in the Apps' APK. Easy, but illegal. Also, need to redistribute the modified APK, which is also not allowed under Facebooks license.

 - [Blog about sniffers](https://stanfy.com/blog/monitor-mobile-app-traffic-with-sniffers/).
 - [Fiddler sniffer app](https://www.telerik.com/fiddler).
 - [MitM app](https://mitmproxy.org/).
 - [Charles Proxy sniffer app](https://www.charlesproxy.com/documentation/proxying/ssl-proxying/).
 - [Security SE about HTTPS](https://security.stackexchange.com/questions/153440/intercepting-android-app-traffic-with-burp).
 - [3G/4G does not allow proxy](https://medium.com/@rotxed/how-to-debug-http-s-traffic-on-android-7fbe5d2a34)
 - [Key pinning to protect MitM](https://en.wikipedia.org/wiki/HTTP_Public_Key_Pinning).
 - [idem](https://www.nowsecure.com/blog/2017/06/15/certificate-pinning-for-android-and-ios-mobile-man-in-the-middle-attack-prevention).
 - [Changes to Trusted Certificate Authorities in Android Nougat](https://android-developers.googleblog.com/2016/07/changes-to-trusted-certificate.html).
 - [Whom You Gonna Trust? A Longitudinal Study on TLS Notary Service](https://www.sba-research.org/wp-content/uploads/publications/TLSnotaries_preprint.pdf).
 - [HTTPS traffic analysis and client identification using passive SSL/TLS fingerprinting](https://jis-eurasipjournals.springeropen.com/track/pdf/10.1186/s13635-016-0030-7).

| Pro | Con |
|-----|-----|
| Easy to set up with existing VPN Apps in the playstore + openVPN or so at server | https hides the url, only server name (ie. DNS records) available. |
| will get all traffic (Apps + browser) | All major websites and browsers consider this 'unsafe' and are starting to refuse visiting sites in http. |
| catches all HTTP | |

### Browser Based with Mobile Sync

Desktop browser plugins can be used to track browsing history (possible for both Chrome and Firefox). Using Google Sync, or other types of synchronisation, the mobile history can be exported to Desktop.

In-app browsing history needs to be included. This can be a problem:
 - [Include in-app browsing history in Chrome sync](https://android.stackexchange.com/questions/201204/include-in-app-browsing-history-in-chrome-sync)

Then, in-app browsing needs to be switched off globally. This, too, can be a problem:
 - [Disable all in-app browsing while keeping Chrome as default browser](https://android.stackexchange.com/questions/201150/disable-all-in-app-browsing-while-keeping-chrome-as-default-browser)

Then, in-app browsing needs to be switched off for each app (Facebook: turn off in-app browsing => request browser history > possible in full FB app, not in FBlite > what happens after update? > disable automatic updates? > FB also has a Download your information option which includes advertisers_you've_interacted_with). This, again, can be a problem:
 - [Disable in-app browsing on the Facebook Lite app](https://android.stackexchange.com/questions/201210/how-can-in-app-browsing-be-disabled-on-the-facebook-lite-app)

Extra research is needed to understand how this works when apps are updated over time and to understand how this interacts with Chrome custom tabs in other apps.

 - [Download Chrome settings](https://android.stackexchange.com/questions/117288/how-can-i-find-my-chrome-bookmarks-from-windows/117334).
 - [Google MyActivity](https://myactivity.google.com/myactivity).
 - [SE on Disable in-app browsing](https://android.stackexchange.com/questions/201150/disable-all-in-app-browsing-while-keeping-chrome-as-defaut-browser).
 - [History of in-app browsing](https://www.addthis.com/blog/2017/04/04/in-app-browsers-explained/).
 - [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d).
 - [Either disable in-app browsing per app or disable WebView on the entire phone](https://www.quora.com/How-do-I-turn-off-Android-WebView-in-apps).

| Pro | Con |
|-----|-----|
| Uses existing browser technology | Does not (always) include in-app browsing history |
| Works with all browsers | Requires participant to keep in-app browsing off (globally or for each app) |
|  | App-updates might automatically bring in-app browsing back on |
|  | Not always distinction between mobile and desktop browsing |

### Request data from Browser/App company

We could do a GDPR request by directly downloading data via special webpage (at Google this is [TakeOut](http://takeout.google.com) which gives URL + timestamp + deviceid). I verified that signing into Chrome on mobile and on Desktop (and enabling sync) gives the browser history on Google Takeout. It includes a client_id and a time_usec. In-app browser history is available in full chrome app, but does not sync.

Note that this does not work for all internet companies: facebook / twitter do not allow analytics to happen on their website (except via their specific tools *Comment Vincent: Can we elaborate here?* ). How does this interact with our writing a plugin? *Comment Vincent: Can we elaborate here?*

Note, again, that Chrome Custom Tabs does not forward its browser history to Google Takeout. So, any app using Chrome Custom Tabs will bias our data collection, similar to the previous section.

| Pro | Con |
|-----|-----|
| Easy procedure for the participant | Does not (always) include in-app browsing history |
| Data can be collected over long periods of time | Requires participant to keep in-app browsing off (globally or for each app) |
|  | App-updates might automatically bring in-app browsing back on |
|  | Not always distinction between mobile and desktop browsing |

### Create own mobile browser via "Custom Tabs"

Although many apps allow the user to specify whether or not to use in-app browsing, some apps do not. Furthermore, it is hard to verify that users will keep in-app browsing turned off all the time, or that it does not automatically revert back after an update.

One method to log in-app browsing is to create a forwarding app that presents itself as the default browser, logs the url and then opens a real browsing (such as Chrome, however: [Evaluation of using chrome custom tabs (blog post)](https://medium.com/@vardaansh1/use-chrome-custom-tabs-they-said-it-will-be-fun-they-said-b5fabe5daea3)) to render the webpage [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d).

However, how would the researcher verify that  the user does not change the default browser. And can we be certain that some apps cannot overrule the default browser and choose their own favourite?

| Pro | Con |
|-----|-----|
| Logs both regular and in-app browsing history | Difficult to engineer such a browser manager |
| Avoids context switch by enabling in-app browsing | Perhaps some apps can overrule the default browser |
| Avoids cookie information being shared between apps > vulnerabilities |  |

### Request data from News company

We could do a GDPR request via email to the news sources (e.g. sanoma or NLprofiel and using the cookies of the users to identify them) ...

| Pro | Con |
|-----|-----|
| Easy for the user | Much work for the researcher |
| Legal | Some companies might not want to comply |
|  | Some companies may simply not have access to that data themselves (might not track in-app browsing) |

## Method comparison

Evaluation of all options against criteria:

| Criteria | VPN | Browser-based | Create own browser | Request data from Browser/app company | Request data from News company |
|-----|-----|-----|-----|-----| -----|
| 1. Collects mobile browsing data | Yes | Yes | Yes | Yes | Yes |
| 2. Can track HTTPS and HTTP urls | Yes? | Yes | Yes | Yes | Yes |
| 3. Deals with in-app browsing | Yes | No, this needs to be turned off in the other apps | No |  Yes, if configured | Not applicable |
| 4. Tracks only whitelisted sites | No | No | No | No | Yes |
| 5. Can be build in less than two month project | Yes |  Yes | ? | Yes | Yes |
| 6. Easy to maintain, despite evolutions in mobile phone and web technologies | ? | Yes | Yes? | Yes | Yes |
| 7. Data can be collected over long periods of time (multiple months) | Yes | Yes, if participant does not delete their browser history | Yes | Yes | Unknown |
| 8. No data storage without explicit permission from participant | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | Yes |
| 9. Does not store contact information: telephone number or email address. | Yes, unless it is part of an url? | Yes, unless it is part of an url? | Yes, unless it is part of an url? | No | Yes |
| 10. Does not store the social media account name. | Yes, unless it is part of an url? | Yes, unless it is part of an url? | Yes, unless it is part of an url? | No | Yes |
| 11. Does not store information on the specific location of the participant, e.g. ip address, GPS coordinates, or physical address. | No? | Yes? | Yes? | Yes, if data request is made correctly | Yes |
| 12. Small time investment for (social science) researcher | Yes | Yes | Yes | Yes | Yes |
| 13. Small time investment and simple to setup for participant | ? |  Yes | ? | Yes | Yes |
| 14. Good overview of what information is being shared | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | Yes |
| 15. No action required by participant in daily life situation | Yes | Yes | Must use our browser | Yes | Yes |
| 16. No impact on normal use of phone/PC: e.g. no apps that drain the battery. | Yes? |  Yes | Yes, assuming it only collects browsing behaviour | Yes | Yes |


## Discussion

To do:
- More research needed on the effect of Chrome custom tabs used by other phone apps. How does this affect the output from Browser-based monitoring and Google Takeout?
- More research needed on the specific requirements we can derive from GDPR?
- More research needed on pros and cons of GDPR-based data requests via email.
- The criteria on maintainability and time needed to build it need critical attention from someone with more experience in this than me (Vincent).
