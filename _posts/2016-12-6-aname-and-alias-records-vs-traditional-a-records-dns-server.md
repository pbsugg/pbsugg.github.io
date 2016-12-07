---
layout: post
title: Mitigating Denial-of-Service Attack Risks with DNS Providers - ANAME, ALIAS and CNAME Records
---

I had difficulty logging onto the [GitHub](https://github.com) website this afternoon, and while I haven't been able to confirm that [the outage](https://twitter.com/githubstatus/status/806228936208289792) was because of [yet another](http://www.datacenterdynamics.com/content-tracks/security-risk/ddos-attacks-hit-cloudflare-originate-from-new-botnet/97438.fullarticle) broad-based denial-of-service attack, there is no doubt that [this type](https://www.hackread.com/new-mirai-like-botnet-ddos-attack/) of [event](http://www.computerworld.com/article/3147081/security/new-botnet-launching-daily-massive-ddos-attacks.html) is becoming more widespread. Most recent big incidents have targeted DNS (domain name server) providers. I've had reason to look into this issue because I set up a few new site domains recently, and have some high-level thoughts about reducing a site's exposure to a generalized DNS attack.

First, some terms: your apex domain name (synonyms: root, base, canonical) is typically the name of your site, stripped of all prefixes (i.e., `www`). `example.com` is an apex domain. Now, until relatively recently, the main way to set up a site was to point this apex domain to your DNS provider's `A` record. DNS is full of these obscure-sounding record types, so bear with me. The `A` record is the one that points directly to your site's IP address, a numerical value (e.g., `127.0.0.1`). All other record types (e.g., those including `www`, subdomains like `blog.example.com`) map to this basic domain type: the `A` record.

 The crucial thing to understand for the purpose of denial-of-service attacks is that the domain listed in an `A` record *cannot* be re-routed to another server in the event of an attack. `A` records are "dumb." By convention, they point to the IP you give them, and ONLY that IP. This means that when an attack on your DNS occurs, and it affects that IP, there is usually nothing the DNS can do to re-route your traffic to another server. You're stuck with that affected IP until the attack subsides.

Other record types (e.g., `CNAME`, `ANAME`, `ALIAS`) don't have this limitation. Their essential advantage is that during an attack, they can usually switch you to another IP address, in an unaffected part of the DNS network.

There's your first mitigation step: if your apex-level domain is pointing to an A record, you simply need to change that. Here are a few strategies, although the specific procedure will depend on the particular options offered by your DNS provider.

**CNAME** aka "canonical" name records, are called such because they don't point directly to an IP, but rather to the canonical domain name which then points to the IP. CNAMES have been around for a while. If `example.com` were your apex domain, then `www.example.com` (or, more precisely, the entry would be `www`) could be a `CNAME` record that pointed to your A record, which points to your IP. One risk-mitigation strategy is to put your apex domain into a `CNAME` record, which can then be pointed elsewhere during an attack. Again, how one might do this depends on the DNS provider, but you can see examples of that approach in [heroku](https://devcenter.heroku.com/articles/custom-domains#add-a-custom-root-domain) and [netlify](https://www.netlify.com/blog/2016/01/12/this-weekends-ddos-attack-and-whats-in-a-cname/)

**ANAME / ALIAS** records. This record-type has been around for a few years, although it is not supported by all providers (GoDaddy doesn't support it) and [regarded with caution by others](https://help.iwantmyname.com/customer/portal/articles/1599947-do-you-support-alias-or-aname-dns-records-) because it is not part of the official industry standard. But if your DNS provider offers it, it is at least worth a look. These records also allow you to map your apex-level domain name to another, non-IP hostname. Its biggest advantage over `CNAME` records seems to be that it allows for speed enhancements in certain situations because, unlike the `CNAME` record--which must first hop to your apex domain and THEN to the IP--the `ANAME` or `ALIAS` record can jump directly to the IP address. It also seamlessly tracks IP address changes in the event of changes by the `A` Record. Note: I haven't been able to come up with a definitive answer about the difference (if any) between the `ANAME` and `ALIAS` record types.

If you manage a site whose DNS configuration is not fully familiar to you, it's worth investigating this issue and easily protecting yourself against avoidable downtime. Listing your apex domain in a record type besides a vanilla A record also has other advantages--like allowing for speed improvements through caching on a [CDN](http://www.webopedia.com/TERM/C/CDN.html).


_Additional Resources_:

[Heroku: The Limitations of DNS A-Records](https://devcenter.heroku.com/articles/apex-domains)

[DNS Made Easy: ANAME Records](http://help.dnsmadeeasy.com/managed-dns/records/aname-records/)

[DNS Made Easy: Working With ANAME Records (Video)](https://www.youtube.com/watch?v=oJ4AbqTtt_8)
