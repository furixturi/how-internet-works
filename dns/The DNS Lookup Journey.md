# The DNS Lookup Journey
[Medium](https://medium.com/@xiaolishen/the-dns-lookup-journey-240e9a5d345c)

…Or one answer to the famous “what happens when you type an URL in your browser address bar and hit the Enter key” question.

![The DNS Lookup Journey Diagram](https://raw.githubusercontent.com/furixturi/how-internet-works/master/dns/DNS%20lookup.jpg)

Finally I thought I might as well figure this out and take some note, then I made the charming graph above with [https://app.diagrams.net/](https://app.diagrams.net/) (sorry I still typed [draw.io](https://draw.io) in my browser address bar and watched it redirected).

So, what exactly happened after I hit the Enter key?

1. My **browser** looks at its DNS cache to see if it’d been there before and knew the IP address mapped to it. Let’s say it didn’t.
2. My **computer** also looks into its local DNS cache to see if it knows an IP address mapped to that domain. Nope.
3. My home **router** comes in to try its local DNS cache. Still no luck.
4. We go out to ask my **ISP’s DNS server** if that domain is in its cache. Sorry it is not there, but that recursive DNS server can help us resolve it.
5. The resolver goes to ask the **root name servers** about the domain name, let’s say it is _example.com_
6. Root name servers know all the **TLD (top level domain) name servers**. Since we came with a _.com_ domain, it forwards our query to one TLD name server that handles _.com_ domains.
7. The _.com_ TLD name server knows the **authoritative name server** who stores DNS record for the domain _example.com_ so it forwards the query ahead.
8. The authoritative name server respond with an _A record_ (address record which is an IP address) mapped to the domain name.
9. The IP address was then passed all the way back to our browser, each one in between who has a DNS cache will cache it on the way so next time when we or someone else asks about example.com, the answer comes faster.
10. Browser opens a **TCP/IP connection** to the IP address, which is the address of the server hosting _example.com_, then sends an **HTTP request**. If the server is up and running, it sends back **HTTP responses** to our browser.

That’s the DNS lookup journey in the unfortunate case where the record is not available in any middle stop’s DNS cache.

Of course, it can get more complicated if there was _DNS redirect_ in play. At step 8, the authoritative name server might see an _Alias record_ or a _CNAME record_ (canonical name) mapped to the queried domain.

In the case of an Alias record, the authoritative name server goes on to chase down the mapped domain name until it can get back an IP address for you.

In the case of a CNAME record, the authoritative name server simply respond with the mapped “canonical name”. Say we asked for _foo.example.com_ and it’s a CNAME record which points to the canonical name _bar.example.com_, we will get back the latter and then start another DNS lookup journey with it.


