# Technologies for Monitoring News Consumption

| Abstract |
|----------|
| This whitepaper explores what technologies are available for monitoring mobile news consumption and to what extend they meet certain requirements. |

| Keywords |
|----------|
| News consumption, traffic interception, traffic analysis, online tracking, network monitoring, network forensics, sniffers. |

| Table of Contents |
|-------------------|
| [Introduction](#introduction) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Related Work](#related-work) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Criteria](#criteria) |
| [Theoretical Background](#theoretical-background) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Internet Traffic and Security](#internet-traffic-and-security) |
| &nbsp;&nbsp;&nbsp;&nbsp;[Legality and GDPR](#legality-and-gdpr) |
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

To collect data about mobile news consumption, a couple of criteria need to be taken into account. These will be discussed in the [Criteria](#criteria) section. Subsequently, we will explore what technologies are available for monitoring mobile news consumption and to what extend these meet the requirements.

### Related Work

Previous studies that explored technologies for news consumption looked at ... (_to be completed_).

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

It is important to remember that any approach can, in principle, be make GDPR compliant by using secure storage, filtering of data and obtaining explicit approval from the participant. The real question here is whether the approach is GDPR compliant by default or easy to make compliant.

- No data storage without explicit permission from participant *Laurens: what is meant with this?*
- Does not store contact information: telephone number or email address. *Laurens: what is meant with this?*
- Does not store the social media account name. *Laurens: what is meant with this?*
- Does not store information on the specific location of the participant, e.g. ip address, GPS coordinates, or physical address. *Laurens: what is meant with this?*

**Non-intrusiveness and scalability:**
- Small time investment for researcher. (to do: define this more clearly)
- Small time investment and simple to setup for participant.
- Good overview to participant of what information is being shared.
- Minimal action required by participant in daily life situation, e.g. we do not want the participant to have keep a diary of what news they consume.
- No impact on normal use of phone/PC: e.g. we do not want apps that rapidly drain the phone battery.

## Theoretical Background

*Note (Vincent): I think a general introduction about how internet traffic works would help, because this will provide a conceptual framework for the reader without expert knowledge in this area. For example, does our audience know what a proxy server is, what we mean by network interactions, what do we mean by illegal (national law? facebook rules? both?)*

### Internet Traffic and Security

[Blog about the Internet](http://www.theshulers.com/whitepapers/internet_whitepaper/): 
The Internet is a global network of computers where each computer has a unique address. Internet addresses are in the form _nnn.nnn.nnn.nnn_ where _nnn_ must be a number from 0-255. Such an address is known as an IP address. Using the internet, packets of data can be sent to other computers. The IP (Internet Protocol) directs packets to a specific computer using an IP address. Data packets are first sent to your ISP (Internet Service Provider) and, then, routed onto the ISP's backbone. From here the packets will usually journey through several routers and over several backbones, dedicated lines, and other networks until they find their destination. When a packet arrives at a router, the router examines the IP address put there by the IP protocol layer on the originating computer. The router checks it's routing table. If the network containing the IP address is found, the packet is sent to that network. If the network containing the IP address is not found, then the router sends the packet on a default route, usually up the backbone hierarchy to the next router. Hopefully the next router will know where to send the packet. IP addresses are not easy to remember, so the DNS (Domain Name Service) can translate names like "www.website.com" to IP addresses using a distributed database. If a DNS server does not contain the domain name requested by another computer, the DNS server re-directs the requesting computer to another DNS server. Once the data packet arrives at the computer, the TCP (Transmission Control Protocol) directs packets to a specific application on a computer using a port number. Ports can be thought of as separate channels on each computer, which allow the use of multiple applications simultaneously via different port numbers. One of the most commonly used services on the Internet is the World Wide Web (WWW). The application protocol that makes the web work is HTTP (Hypertext Transfer Protocol). HTTP is the protocol that web browsers and web servers use to communicate with each other over the Internet. Most protocols are connection oriented. This means that the two computers communicating with each other keep the connection open over the Internet. HTTP does not however. Before an HTTP request can be made by a client, a new connection must be made to the server.

[HowStuffWorks about Encryption](https://computer.howstuffworks.com/encryption.htm): 
HTTP is not secured, so the data packets can be read by anyone. HTTPS is secured using a combination of symmetric and asymetric keys. A popular implementation of the asymetric public-key encryption is the Secure Sockets Layer (SSL). Originally developed by Netscape, SSL is an Internet security protocol used by Internet browsers and Web servers to transmit sensitive information. SSL has become part of an overall security protocol known as Transport Layer Security (TLS). During an HTTPS connection, the browser sends out the public key and the certificate, checking three things: 1) that the certificate comes from a trusted party; 2) that the certificate is currently valid; and 3) that the certificate has a relationship with the site from which it's coming.

[Blog about HTTPS](https://robertheaton.com/2014/03/27/how-does-https-actually-work/): 
Once the connection is established, both parties can use the agreed algorithm and keys to securely send messages to each other. The handshake begins with the client sending a ClientHello message. This contains all the information the server needs in order to connect to the client via SSL, including the various cipher suites and maximum SSL version that it supports. Now that contact has been established, the server has to prove its identity to the client. This is achieved using its SSL certificate which contains various pieces of data, including the name of the owner, the domain it is attached to, the certificate's public key, the digital signature and information about the certificate's validity dates. The client checks that it either implicitly trusts the certificate, or that it is verified and trusted by one of several Certificate Authorities (CAs) that it also implicitly trusts. The encryption of the actual message data exchanged by the client and server will be done using a symmetric algorithm, the exact nature of which was already agreed during the Hello phase. A symmetric algorithm uses a single key for both encryption and decryption, in contrast to asymmetric algorithms that require a public/private key pair. Both parties need to agree on this single, symmetric key, a process that is accomplished securely using asymmetric encryption and the server's public/private keys.

[Blog about ESNI](https://blog.cloudflare.com/encrypted-sni/): 
The Server Name Indication (SNI) extension lets servers host multiple TLS-enabled websites on the same set of IP addresses, by requiring clients to specify which site they want to connect to during the initial TLS handshake. Without SNI the server wouldn't know which certificate to serve to the client or which configuration to apply to the connection. The client adds the SNI extension containing the hostname of the site it's connecting to to the ClientHello message. It sends the ClientHello to the server during the TLS handshake. Unfortunately the ClientHello message is sent unencrypted, due to the fact that client and server don't share an encryption key at that point. This means that an on-path observer (say, an ISP, coffee shop owner, or a firewall) can intercept the plaintext ClientHello message, and determine which website the client is trying to connect to. That allows the observer to track which sites a user is visiting. But with SNI encryption the client encrypts the SNI even though the rest of the ClientHello is sent in plaintext. ESNI was released in 2018 and is currently not yet used by many browsers.

### Legality and GDPR

_insert background_

## Tracking Options

### VPN Sniffers

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
| Easy to set up with existing VPN Apps in the playstore + openVPN or so at server | Https hides the url, only server name (ie. DNS records) available. |
| will get all traffic (Apps + browser) | All major websites and browsers consider this 'unsafe' and are starting to refuse visiting sites in http. |
| catches all HTTP content | |
| Possibly violates Terms of Service of some apps |  |

### Browser Based with Mobile Sync

Desktop browser plugins can be used to track browsing history (possible for both Chrome and Firefox). Using Google Sync, or other types of synchronisation, the mobile history can be exported to Desktop.

In-app browsing history needs to be included. This can be a problem:
 - [Include in-app browsing history in Chrome sync](https://android.stackexchange.com/questions/201204/include-in-app-browsing-history-in-chrome-sync)

A solutions would be to switch off in-app browsing globally. This, too, can be a problem:
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

We could do a GDPR request by directly downloading data via special webpage (at Google this is [TakeOut](http://takeout.google.com) which gives URL + timestamp + deviceid). For Chrome mobile and on Desktop (by enabling sync), this gives the browser history on Google Takeout. It includes a client_id and a time_usec. In-app browser history is available in full chrome app, but does not sync.

Note that this does not work for all internet companies: Facebook / twitter do not allow analytics to occur on their website (except via their specific tools *Comment Vincent: Can we elaborate here?* ). How does this interact with our writing a plugin? *Comment Vincent: Can we elaborate here?*

Note, again, that Chrome Custom Tabs does not forward its browser history to Google Takeout. So, any app using Chrome Custom Tabs will bias our data collection, similar to the previous section.

| Pro | Con |
|-----|-----|
| Easy procedure for the participant | Does not (always) include in-app browsing history |
| Data can be collected over long periods of time | Requires participant to keep in-app browsing off (globally or for each app) |
| Distinction between mobile and desktop browsing (at least for Google Takeout) | App-updates might automatically bring in-app browsing back on |

### Create own mobile browser via "Custom Tabs"

Although many apps allow the user to specify whether or not to use in-app browsing, some apps do not. Furthermore, it is hard to verify that users will keep in-app browsing turned off all the time, or that it does not automatically revert back after an update.

One method to log in-app browsing is to create a forwarding app that presents itself as the default browser, logs the url and then opens a real browsing (such as Chrome, however: [Evaluation of using chrome custom tabs (blog post)](https://medium.com/@vardaansh1/use-chrome-custom-tabs-they-said-it-will-be-fun-they-said-b5fabe5daea3)) to render the webpage [Manager app suggestion](https://android.stackexchange.com/questions/145745/prevent-apps-opening-links-in-chrome-custom-tabs-i-e-open-in-default-browser-d).

However, how would the researcher verify that the user does not change the default browser. And can we be certain that some apps cannot overrule the default browser and choose their own favourite?

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
| 2. Can track HTTPS and HTTP urls | No | Yes | Yes | Yes | Yes |
| 3. Deals with in-app browsing | Yes | No | Yes |  Not all browsers | Not applicable |
| 4. Tracks only whitelisted sites | No | No | No | No | Yes |
| 5. Can be build in less than two month project | Yes |  Yes | Probably not | Yes | Yes |
| 6. Easy to maintain, despite evolutions in mobile phone and web technologies | Yes | Relatively easy | Relatively easy | Yes | Yes |
| 7. Data can be collected over long periods of time (multiple months) | Yes | Yes | Yes | Yes | Possibly not |
| 8. No data storage without explicit permission from participant | No | No | No | No | Yes |
| 9. Does not store contact information: telephone number or email address. | Yes, unless it is part of an url? | Yes, unless it is part of an url? | Yes, unless it is part of an url? | No | Yes |
| 10. Does not store the social media account name. | Yes, unless it is part of an url? | Yes, unless it is part of an url? | Yes, unless it is part of an url? | No | Yes |
| 11. Does not store information on the specific location of the participant | No? | Yes? | Yes? | Yes, if data request is made correctly | Yes |
| 12. Small time investment for (social science) researcher | Yes | Yes | Yes | Yes | Yes |
| 13. Small time investment and simple to setup for participant | ? |  Yes | ? | Yes | Yes |
| 14. Good overview of what information is being shared | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | No, we would have to facilitate that | Yes |
| 15. No action required by participant in daily life situation | Yes | Yes | Must use our browser | Yes | Yes |
| 16. No impact on normal use of phone/PC: e.g. no apps that drain the battery. | Yes | Yes | Yes, assuming it only collects and redirects | Yes | Yes |

## Discussion

To do:
- More research needed on the effect of Chrome custom tabs used by other phone apps. How does this affect the output from Browser-based monitoring and Google Takeout?
- More research needed on the specific requirements we can derive from GDPR?
- More research needed on pros and cons of GDPR-based data requests via email.
- The criteria on maintainability and time needed to build it need critical attention from someone with more experience in this than me (Vincent).
