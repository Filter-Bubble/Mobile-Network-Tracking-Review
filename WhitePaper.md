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
| &nbsp;&nbsp;&nbsp;&nbsp;[Create own browser via Chrome custom tabs](#create-own-browser-via-chrome-custom-tabs) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Request data from internet company via webpage](#request-data-from-internet-company-via-webpage) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Request data from internet company via email](#request-data-from-internet-company-via-email) |
| [Method comparison](#method-comparison) |
| [Discussion](#discussion) |

## Introduction

The news we read online may influence our opinions and beliefs, while little is known about how much we life in news bubbles that filter certain types of news for certain types of people and the effect this may have on our openions and beliefs.
To study this reliable research data is needed. Most research so far focuses on survey data and desktop applications (insert references).  However, mobile news consumption is expected to be important too (insert references).
However, to collect data on mobile (and desktop) news consumption a couple of important criteria need to be taken into account:
- Legal (GDPR) constraints
- Participant acceptability: we want participants to be representative of the general population, by which the threshold to participate in study needs to be low.
- Technical feasibility.

In this paper we aim to explore what technologies are available for monitoring (mobile) news consumption and to what extend they meet these requirements.

### Related Work

Previous study that explored technologies for news consumption looked at... (to be completed)

### Criteria

In this section we will map out the criteria that can be put on (mobile) technologies for monitoring news consumption.

**Technologically viable:**
- Collects mobile browsing data
- Can track full HTTPS and HTTP urls
- Deals with in-app browsing
- Tracks only whitelisted websites
- Can be build in less than two month project
- Easy to maintain, despite evolutions in mobile phone and web technologies
- Data can be collected over long periods of time

**Legal and GDPR compliant:**
- No data storage without explicit permission from participant
- ... (note: expand on this with specific criteria)

**Non-intrusive and scalable:**
- Small time investment for researcher.
- Small time investment and simple to setup for participant.
- Good overview to participant of what information is being shared.
- Minimal action required by participant in daily life situation, e.g. we do not want the participant to have keep a diary of what news they consume.
- No impact on normal use of phone/PC: e.g. we do not want apps that rapidly drain the phone battery.

## Tracking Options

Note (Vincent): I think a general introduction about how internet traffic works would help, because this will provide a conceptual framework for the reader without expert knowledge in this area. For example, does our audience know what a proxy server is, what we mean by network interactions, what do we mean by illegal (national law? facebook rules? both?)

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


### Browser Based

Desktop browser plugins can be used to track browsing history (possible for both Chrome and Firefox). Using Google Sync, or other types of syncronisation, the mobile history can be exported to Desktop.
Facebook: turn off in-app browsing => request browser history > possible in full FB app, not in FBlite, but does not seem to sync consistently (perhaps only after 5 seconds of viewing, which might even be a good thing) > what happens after update? > disable automatic updates? > FB also has a Download your information option (which includes advertisers_you've_interacted_with).

Comment by Vincent: I have move the 'create our own browser' option below, because it seems an alternative method, by which it is easier to also evaluate it separately. Further, I have renamed it as 'Create own browser, e.g. via Chrome custom tabs' to make it a bit more specific (please correct me if I misunderstood something).

 - [Download Chrome settings](https://android.stackexchange.com/questions/117288/how-can-i-find-my-chrome-bookmarks-from-windows/117334).
 - [Google MyActivity](https://myactivity.google.com/myactivity).
 - [SE on Disable in-app browsing](https://android.stackexchange.com/questions/201150/disable-all-in-app-browsing-while-keeping-chrome-as-defaut-browser).
 - [History of in-app browsing](https://www.addthis.com/blog/2017/04/04/in-app-browsers-explained/).
 - [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d).
 - [Either disable in-app browsing per app or disable WebView on the entire phone](https://www.quora.com/How-do-I-turn-off-Android-WebView-in-apps).

| Pro | Con |
|-----|-----|
| Uses existing browser technology | Requires participant to turn off in-app browsing |
| Works with all browsers | No distinction between mobile and desktop browsing |
| ... | Extra research needed to understand how this works when apps are updated over time |
| ... | Extra research need to understand how this interacts with Chrome custom tabs in other apps |


### Create own browser via Chrome custom tabs
An alternative method is to create a forwarding app that opens Chrome for us [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d). An app which presents itself as a browser, logs the url and then opens the real default browser so the user notices nothing. Chrome custom tabs is an example of this.

*Comment Vincent: How do we check that the default is used by the user. Is it possible that some other phone apps overrule the default browser and choose their own favorite?*

*Comment Vincent: It is unclear to me what this option adds relative to Browser based option (above)*

| Pro | Con |
|-----|-----|
| ... | ... |


### Request data from internet company via webpage

We could do a GDPR request by directly downloading data via special webpage (at Google this is [TakeOut](http://takeout.google.com) which gives URL + timestamp + deviceid). I verified that signing into Chrome on mobile and on Desktop (and enabling sync) gives the browser history on Google Takeout. It includes a client_id and a time_usec. In-app browser history is available in full chrome app, but does not sync.

Note that this does not work for all internet companies: facebook / twitter do not allow analytics to happen on their website (except via their specific tools *Comment Vincent: Can we elaborate here?* ). How does this interact with our writing a plugin? *Comment Vincent: Can we elaborate here?* Note also that Chrome Custom Tabs does not forward its browser history to Google Takeout. So, any app using Chrome Custom Tabs will bias our data collection.

 - [Disable all in-app browsing while keeping Chrome as default browser](https://android.stackexchange.com/questions/201150/disable-all-in-app-browsing-while-keeping-chrome-as-default-browser)
 - [Disable in-app browsing on the Facebook Lite app](https://android.stackexchange.com/questions/201210/how-can-in-app-browsing-be-disabled-on-the-facebook-lite-app)
 - [Include in-app browsing history in Chrome sync](https://android.stackexchange.com/questions/201204/include-in-app-browsing-history-in-chrome-sync)


*Comment Vincent: Do I understand it correctly that Google takeout will not be representative if the participant uses many apps that work with Chrome Custom Tabs, because those traffic will not end up in Google Take out?*

| Pro | Con |
|-----|-----|
| Easy procedure for the participant | ... |
| Data can be collected over long periods of time | Google takeout requires that we have similar solutions for Twitter and Facebook based news consumption |

### Request data from internet company via email

We could do a GDPR request via email (e.g. sanoma or NLprofiel and using the cookies of the users to identify them) ...

## Method comparison

Evaluation of all options against criteria:

| Criteria | VPN | Browser-base | Create own browser, e.g. via Chrome custom tabs | Request data from internet company via webpage (filled in for Google Takeout) | Request data from internet company via email |
|-----|-----|-----|-----|-----| -----|
| Collects mobile browsing data | Yes | Yes | Yes | Yes | Yes |
| Can track HTTPS urls | Yes? | Yes | Yes | Yes | Yes |
| Deals with in-app browsing | Yes | Yes | No |  Yes, if configured | Not applicable |
| Tracks only whitelisted sites | No | No | No | No | Yes |
| Can be build in less than two month project | Yes |  Yes | ? | Yes | Yes |
| Easy to maintain, despite evolutions in mobile phone and web technologies | ? | Yes | ? | Yes | Yes |
| Data can be collected over long periods of time | Yes | Yes, if participant does not delete their browser history | ? | Yes | Unknown |
| No data storage without explicit permission from participant | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | Yes |
| ... insert extra criteria related to GDPR | ... | ... | ... | ... | ... |
| Small time investment for (social science) researcher | Yes | Yes | Yes | Yes | Yes |
| Small time investment and simple to setup for participant | ? |  Yes | ? | Yes | Yes |
| Good overview of what information is being shared | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | Yes |
| No action required by participant in daily life situation | Yes | Yes | Must use our browser | Yes | Yes |
| No impact on normal use of phone/PC: e.g. no apps that drain the battery. | Yes? |  Yes | Unclear | Yes | Yes |


## Discussion

To do:
- More research needed on the effect of Chrome custom tabs used by other phone apps. How does this affect the output from Browser-based monitoring and Google Takeout?
- More research needed on the specific requirements we can derive from GDPR?
- More research needed on pros and cons of GDPR-based data requests via email.
- The criteria on maintainability and time needed to build it need critical attention from someone with more experience in this than me (Vincent).
