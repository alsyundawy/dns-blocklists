# FAQ

A practical guide to how these DNS blocklists get built, which version fits your setup, and why some domains are left unblocked on purpose. If a term looks unfamiliar, check the [Glossary](#glossary) at the bottom.

## Table of Contents

1. [Where does the data come from, and how are the lists built?](#sources)
2. [Which list version should I use?](#whatshouldiuse)
3. [Which format should I use for my ad blocker or DNS server?](#formats)
4. [Quick setup guide](#quicksetup)
5. [Why aren't referral domains blocked?](#referral)
6. [Why aren't CMPs (cookie consent tools) blocked?](#cmps)
7. [Which lists are available on which DNS services?](#availablelists)
8. [How current is the data, and where can I get it?](#mirrors)
9. [Licensing and liability](#licensing)
10. [Getting help and reporting issues](#support)
11. [How do mini variants, NRD/DGA, and the bypass lists relate to each other?](#listrelationships)
12. [Glossary](#glossary)

---

## <a name="sources"></a> 1. Where does the data come from, and how are the lists built?

These lists aren't just copy-pasted from somewhere else. Each version is built from a mix of core sources, custom extensions, domain categories, Newly Registered Domains (NRDs), and several Top 1M lists like Umbrella, Cloudflare, Tranco, Chrome, and DomCop. False positives and dead domains get cleaned out constantly, and domains the community reports get added too.

On top of that, network logs get reviewed regularly to catch domains worth blocking that haven't made it onto a list yet. The full base list currently sits at around 45 million domains, including entries from the Top 1M lists going back more than 24 months. This combined list also helps figure out which domains are genuinely popular, keeping the blocklists accurate over time instead of frozen in place.

Every list gets tested before release against a big sample of real websites, just to make sure pages, navigation, images, and videos still work like they should.

Want the full list of base sources? Check [sources](sources.md).

**[Back to top](#table-of-contents)**

---

## <a name="whatshouldiuse"></a> 2. Which list version should I use?

Pick the version that fits how much technical help you have on hand and how much risk of breakage you're okay with. Rule of thumb: the stricter the list, the more protection you get, but also the higher the odds something you actually wanted to use gets blocked by accident.

The "risk of breakage" ratings below are a general guide, not exact error rates. They just show how aggressively each version filters domains, not a precise false-positive percentage.

| Version | Best for | Risk of breakage |
|:---|:---|:---|
| [Light](README.md#light) | No admin around to unblock stuff, or an ad blocker that can't handle big lists | Minimal. Built to avoid restrictions almost entirely |
| [Normal](README.md#normal) | Everyday use, same crowd as Light | Low. Restrictions are rare and usually minor |
| [Pro](README.md#pro) | Setups with an admin nearby who can unblock things if needed | Low to moderate. The go-to default for solid privacy without much hassle |
| [Pro++](README.md#proplus) | Experienced users with an admin available | Moderate. Might include some false positives that limit functionality |
| [Ultimate](README.md#ultimate) | Very experienced users with an admin available | High. Deliberately blocks some popular trackers, which can limit app or website functionality |

> [!WARNING]
> Ultimate comes with a few documented side effects worth knowing before you switch:
> - **Meta/Facebook:** some Meta trackers get blocked, which limits Facebook and Facebook Messenger. WhatsApp's graph trackers are blocked too, affecting avatar creation, the in-app help center, and video effects. Everything else in WhatsApp still works fine.
> - **Windows/Xbox:** some Microsoft trackers get blocked, which affects things like Windows Spotlight and Xbox Live Achievements Activity History.
> - **Location and IP trackers:** blocking these is great for privacy, but it can trigger extra CAPTCHAs, wrong regional settings, or reduced functionality on some sites.
>
> Running into one of these issues? Check the known-unblock lists for [Meta](share/facebook.txt) and [Microsoft](share/microsoft.txt), or the general [known issues list](share/ultimate-known-issues.txt).

> [!IMPORTANT]
> Whenever you can, pair your main list with the [Threat Intelligence Feeds (TIF)](README.md#tif) list for extra protection against malicious domains. If your ad blocker chokes on the full TIF list's size, grab the smaller [medium](README.md#tifmedium) or [mini](README.md#tifmini) version instead. On AdGuard Home or AdGuard DNS, it's also worth adding [Dandelion Sprout's Anti-Malware List](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareAdGuardHome.txt). There's also an [IPv4 list](README.md#tifips) you can run alongside the TIF full, medium, or mini list.

**Extra lists worth adding, depending on your goals:**

- **Security focus:** combine TIF with the [Dynamic DNS](README.md#dyndns) list (blocks dynamic DNS services often abused for phishing), the [Badware Hoster](README.md#hoster) list (blocks hosting providers whose infrastructure gets abused for malware a lot), the [Most Abused TLDs](README.md#tlds) list (blocks entire top-level domains with bad reputations, like `.top` or `.gdn`), and the [NRD](README.md#nrd) lists.
- **Protecting kids:** combine the [Gambling](README.md#gambling), [Anti Piracy](README.md#piracy), [Safesearch](README.md#safesearch), [DoH/VPN/TOR/Proxy Bypass](README.md#bypass), [Social Networks](README.md#social), and [NSFW](README.md#nsfw) lists. Heads up: the Social Networks list only blocks traditional platforms (Facebook, Instagram, TikTok, X, Snapchat), not messaging apps like WhatsApp or streaming platforms like Twitch.

> [!NOTE]
> **You usually don't need to add the Fake or Pop-Up Ads lists separately, they're already baked in, though coverage varies by version:**
> - The [Fake](README.md#fake) list (scam shops, fake streaming sites, cost traps) isn't included in Light at all. It's fully included in Normal, Pro, Pro++, Ultimate, and in TIF, TIF medium, and TIF mini.
> - The [Pop-Up Ads](README.md#popupads) list is only partially covered in Light, Normal, and TIF. It's fully included in Pro, Pro++, and Ultimate.
>
> Two specialized lists sit outside the main tiers and aren't included anywhere by default: [URL Shortener](README.md#urlshortener) (mainly for high-security setups, since it can break legit short links) and [DNS Rebind Protection](README.md#dnsrebind) (works with AdGuard, AdGuard Home, and AdGuard DNS, stops attackers from resolving external domains to your local network's private IP addresses). Only add these if your setup really needs them.

**[Back to top](#table-of-contents)**

---

## <a name="formats"></a> 3. Which format should I use for my ad blocker or DNS server?

Every list comes in five formats. Just pick the row that matches your ad blocker or DNS server, the rest all contain the same data, just structured differently for that specific tool.

| Format | Use it with |
|:---|:---|
| Adblock | Pi-hole, AdGuard, AdGuard Home, eBlocker, uBlock Origin, Brave (aggressive mode only), AdBlock-Fast, AdNauseam, Little Snitch Mini |
| DNSMasq | DNSMasq (v2.86 or newer), Diversion (v5 or newer) |
| Wildcard (Asterisk) | Blocky (v0.23 or newer), Nebulo, NetDuma, OPNsense, YogaDNS |
| Wildcard (Domains only) | DNSCloak, DNSCrypt, FRITZ!Box (FRITZ!OS v8.40 or newer), TechnitiumDNS, adblock-lean, PersonalDNSfilter, InviZible Pro |
| RPZ | Bind, Knot, PowerDNS, Unbound, and other software supporting Response Policy Zones |

A few lists don't follow this pattern:

- The full **TIF** list is too big for AdGuard Mobile for iOS and needs at least 2 GB of RAM in AdGuard Home. Its RPZ version is also split into two files, and you need both.
- **Most Abused TLDs** comes in AdGuard-specific, uBlock Origin-specific, and RPZ-specific variants instead of the usual five, since it relies on exclusion rules that work differently across tools. It also has an aggressive/allowlist pair for both the AdBlock and Wildcard formats.
- **DNS Rebind Protection** only works with AdGuard, AdGuard Home, and AdGuard DNS.
- **NRD/DGA** lists only come as Adblock and plain domain lists.
- The three [DoH/VPN/TOR/Proxy Bypass](README.md#bypass) lists build on each other: [Bypass Full](README.md#bypass_all) covers encrypted DNS servers plus VPN/TOR/proxy services, [DoH only](README.md#bypass_dns) is the narrower encrypted-DNS-only subset, and [DoH IPs](README.md#bypass_ips) is the IPv4 companion specifically for the DoH-only list, not for VPN/TOR/proxy services (which don't resolve to a fixed IP set).
- **Badware Hoster** and **Most Abused TLDs** also ship as a ControlD folder you can import straight into a ControlD profile. Of the two referral lists (see [section 5](#referral)), only the Referral Allowlist has a ControlD folder, the Referral Blocklist doesn't.

**[Back to top](#table-of-contents)**

---

## <a name="quicksetup"></a> 4. Quick setup guide

Here's the fast track to getting protection running, no need to read the rest of the FAQ first.

1. **Pick a version.** Not sure? Start with [Pro](README.md#pro), it's the go-to recommendation for solid protection without much breakage. Check [section 2](#whatshouldiuse) if you want a different balance of strictness and risk.
2. **Figure out your setup.** Are you blocking DNS network-wide (Pi-hole, AdGuard Home, TechnitiumDNS, OPNsense) or using a browser content blocker (uBlock Origin, AdGuard browser extension)? Network-wide blocking protects every device on your network, while a browser blocker only covers that one browser.
3. **Grab the right format.** Check [section 3](#formats) for what your tool expects, then copy the matching list URL from the [README](README.md#overview) into your tool's blocklist or filter subscription settings.
4. **Add Threat Intelligence Feeds (TIF).** Add the [TIF](README.md#tif) list (or its medium/mini version if your tool struggles with size) alongside your main list for extra protection against malware and phishing.
5. **No self-hosted DNS server?** Use one of the [online DNS services](#availablelists) instead, they let you turn these lists on without running your own setup.
6. **Layer on a browser content blocker too.** DNS-level blocking catches most ads, trackers, and malware, but not everything, some ads and scripts load from otherwise legit domains. A browser content blocker like uBlock Origin or AdGuard closes that gap. Think of the DNS list as your network-wide baseline and the browser blocker as the fine-tuned layer on top.
7. **Test it out.** Browse normally for a day. If something breaks, check [section 2](#whatshouldiuse) for known side effects (especially with Pro++ and Ultimate), unblock the specific domain in your tool, and check [section 10](#support) if you need to report a false positive or a missed domain.
8. **Keep it current.** These lists update regularly. If your tool doesn't auto-refresh subscribed lists, set a reminder to re-download, and check [section 8](#mirrors) if you want the freshest data possible.

**[Back to top](#table-of-contents)**

---

## <a name="referral"></a> 5. Why aren't referral domains blocked?

Referral domains are the affiliate and tracking links you often see on deal sites like Slickdeals, in emails, or in search results. They're allowed here on purpose, since they usually only fire when someone clicks a link, not automatically like ads do.

Blocking them would break things like the first result link in a search, and some of these domains double as newsletter unsubscribe links, so blocking them could trap you in unwanted emails instead of freeing you from them.

Here's the breakdown by list version:

- **Light and Normal:** all referral domains are allowed.
- **Pro:** most referral domains are still allowed, but a few get blocked if they're mainly used for other tracking or commonly tied to scam or spam links, even if they could technically also be used for link tracking.
- **Pro++ and Ultimate:** every referral domain that isn't used exclusively for link tracking gets blocked, including ones like `ad.doubleclick.net`, `adservice.google.*`, `app.adjust.*`, and `analytics.adjust.*`.

**Allowlist** (keeps all known link trackers unblocked):

| Format | Link |
|:---|:---|
| Adblock (AdGuard, AdGuard Home, uBlock Origin, etc.) | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/whitelist-referral.txt) |
| Adblock (Pi-hole v6+, TechnitiumDNS, etc.) | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/whitelist-referral-native.txt) |
| Wildcard<br>domains | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/whitelist-referral-onlydomains.txt) |
| ControlD<br>folder | [Download](https://github.com/hagezi/dns-blocklists/blob/main/controld/referral-allow-folder.json) |

Want to actually block referral domains anyway? (**Not recommended.**) Use the lists below. It's better to apply these in a browser content blocker like uBlock Origin instead of network-wide at the DNS level, since DNS-level blocking is a lot harder to fine-tune once it's live. Note that this list doesn't have a ControlD folder, only the Allowlist above does.

| Format | Link |
|:---|:---|
| Adblock | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/blocklist-referral-native.txt) |
| Wildcard<br>domains | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/blocklist-referral-onlydomains.txt) |

**[Back to top](#table-of-contents)**

---

## <a name="cmps"></a> 6. Why aren't CMPs (cookie consent tools) blocked?

Blocking CMPs network-wide breaks a ton of websites and actually takes away your ability to choose what you're consenting to. In practice, blocking a CMP usually just makes the site assume everything's accepted anyway, since it can no longer show you the consent choice in the first place ([see this discussion](https://github.com/hagezi/dns-blocklists/issues/1979#issuecomment-1870498567)).

Deciding whether to block or auto-allow a specific CMP is really a job for content blockers with dedicated filter lists, since those tools can tell which sites should be excluded from blocking a given CMP domain and which shouldn't. Take a look at the exclusion lists used by established cookie filter lists and it becomes pretty obvious why blanket DNS-level blocking just doesn't cut it here, it can't make the nuanced, per-site calls that a proper filter list can.

**[Back to top](#table-of-contents)**

---

## <a name="availablelists"></a> 7. Which lists are available on which DNS services?

Not every DNS provider offers every list. Here's what's currently available:

| Service | Light | Nor<br>mal | Pro | Pro<br>++ | Ulti<br>mate | TIF | By<br>pass | Dyn<br>DNS | Hoster | TLDs | Anti<br>Piracy | Gam<br>bling |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| AdGuard<br>DNS | :x: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: |
| ControlD | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :yellow_square: | :yellow_square: | :notebook: | :notebook: | :yellow_square: | :yellow_square: |
| Rethink<br>DNS | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :x: | :x: | :x: |
| DNS<br>warden | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :x: | :x: | :x: | :x: | :x: | :x: |

**Legend:**

- :green_circle: Fully available as a native list on that service.
- :x: Not available.
- :yellow_square: Included in ControlD's own native lists for that category, no separate list needed.
- :notebook: Available as a separate [ControlD folder](https://github.com/hagezi/dns-blocklists/tree/main/controld).

> [!NOTE]
> A few other free services bundle these lists with fixed presets instead of letting you pick individual versions: [HaGeZi DNS](https://github.com/hagezi/dns-servers) (EU resolvers running Pro + TIF), [DNSBUNKER.org](https://dnsbunker.org/) (Pro + TIF), [Public RDNS](https://public-rdns.com/) (aggressive, family-safe preset), [RobinGroppe.de](https://www.robingroppe.de/serverzeug/dns-server) (TIF only), and [OpenBLD.net](https://openbld.net/docs/get-started/third-party-filters/hagezi/) (Pro + TIF). Heads up: RethinkDNS only updates its copies once a week, so expect a bit of a lag compared to the source repository.

**[Back to top](#table-of-contents)**

---

## <a name="mirrors"></a> 8. How current is the data, and where can I get it?

The primary source for all lists is the [GitHub repository](https://github.com/hagezi/dns-blocklists). GitHub and its two full mirrors, GitLab and Codeberg, update in sync, once a day:

| Source | Update frequency |
|:---|:---|
| [GitHub/jsDelivr](https://github.com/hagezi/dns-blocklists) (primary) | Once a day |
| [gitlab.com/hagezi/mirror](https://gitlab.com/hagezi/mirror) | Once a day, in sync with GitHub |
| [codeberg.org/hagezi/mirror2](https://codeberg.org/hagezi/mirror2) | Once a day, in sync with GitHub |
| [hagezi-mirror.dnsbunker.org](https://hagezi-mirror.dnsbunker.org) | Every 4 to 8 hours |

> [!TIP]
> Need the freshest data possible? Use [hagezi-mirror.dnsbunker.org](https://hagezi-mirror.dnsbunker.org). It's connected directly to the build system and gets each new list version the moment it's built, ahead of the daily GitHub, GitLab, and Codeberg update.

> [!NOTE]
> The GitHub repository occasionally gets compressed and reinitialized to keep its size down. That resets the commit history and invalidates existing forks, worth knowing if you maintain a fork or rely on commit history for tracking changes.

**[Back to top](#table-of-contents)**

---

## <a name="licensing"></a> 9. Licensing and liability

The lists are published under the [GPL-3.0 license](https://www.gnu.org/licenses/gpl-3.0.html), so you can redistribute, modify, or adapt them, but only within the terms of that license. Check the license in the repository before redistributing the lists as part of your own product or service.

The maintainer ("the Provider") publishes the lists as-is, with no warranty of accuracy, completeness, or fitness for any particular purpose, and no guarantee that every malicious domain is caught or that no legitimate domain ever gets blocked by mistake. You use them entirely at your own risk, and the Provider isn't liable for damages from use or misuse, except in cases of willful misconduct, gross negligence, or death/personal injury caused by negligence.

Basically, treat these lists as one layer in a bigger security setup, not a standalone fix. They don't replace firewalls, antivirus or EDR tools, intrusion detection systems, or your own judgment about risk.

This FAQ entry is a plain-language summary and doesn't cover every detail. The [Disclaimer section](README.md#disclaimer) in the repository is the full, legally binding version. If anything here ever conflicts with it, the Disclaimer section governs.

**[Back to top](#table-of-contents)**

---

## <a name="support"></a> 10. Getting help and reporting issues

Found a legitimate domain that got blocked, or spotted one that should be blocked but isn't? Report it through the [issue tracker](https://github.com/hagezi/dns-blocklists/issues) on GitHub. That's the fastest way to get a false positive fixed or a coverage gap closed. You can also reach out by email at [support@hagezi.org](mailto:support@hagezi.org).

Got general questions or just want to chat? Head to the [GitHub Discussions](https://github.com/hagezi/dns-blocklists/discussions) page. There's also a public [Matrix support chat](https://matrix.to/#/#hagezi-support:tchncs.de?via=tchncs.de) if you'd rather talk things through directly. Prefer to reach out personally? [support@hagezi.org](mailto:support@hagezi.org) works too.

**[Back to top](#table-of-contents)**

---

## <a name="listrelationships"></a> 11. How do mini variants, NRD/DGA, and the bypass lists relate to each other?

A few lists in this collection sound similar or get recommended together, but they aren't interchangeable and aren't always meant to be combined. Here's how they actually relate.

**Mini variants aren't a universal category, each one is a size-optimized cut of exactly one specific list:**

- [Light](README.md#light) is the README's own size-optimized version of **Normal** ("basically a size-optimized version of Multi NORMAL"), it just isn't named "Normal Mini".
- [Pro Mini](README.md#promini), [Pro++ Mini](README.md#proplusmini), and [Ultimate Mini](README.md#ultimatemini) are each a cut of that exact tier only, limited to domains that also appear on the Umbrella, Cloudflare, Tranco, Chrome, BuiltWith, Majestic, or DomCop Top 1M/10M lists.
- [TIF Mini](README.md#tifmini) is a cut of **TIF Medium**, not the full TIF list directly.
- [Gambling Mini](README.md#gamblingmini) is a cut of **Gambling Medium**, not the full Gambling list directly.
- Light has no further mini version of its own, it's already the leanest tier in the Multi family.

> [!TIP]
> Pick the mini version of the tier you actually want. Grabbing a random "mini" list without matching it to your target tier defeats the purpose, since each one only contains that specific tier's domains, shrunk down.

**[NRD](README.md#nrd) and [DGA](README.md#nrd) are two alternatives, not two ingredients to combine.** DGA domains are already part of the full NRD list, just filtered down to the high-entropy subset likely generated by malware. Pick full NRD for broader coverage with more noise, or DGA alone for a narrower, lower-noise subset, not both at once.

**The three [DoH/VPN/TOR/Proxy Bypass](README.md#bypass) lists build on each other:**

- [Bypass Full](README.md#bypass_all) covers encrypted DNS servers plus VPN, TOR, and proxy services, the broadest of the three.
- [DoH only](README.md#bypass_dns) is the narrower encrypted-DNS-only subset.
- [DoH IPs](README.md#bypass_ips) is the IPv4 companion specifically for the DoH-only list, not for VPN/TOR/proxy services, since those don't resolve to a fixed, enumerable IP set the same way encrypted DNS servers do.

**[Back to top](#table-of-contents)**

---

## <a name="glossary"></a> 12. Glossary

| Term | What it means |
|:---|:---|
| Adblock format | One of the formats these lists come in. Looks like classic ad blocker filter rules and works with tools like Pi-hole, AdGuard, AdGuard Home, and uBlock Origin. |
| AdGuard / AdGuard Home | Two different things with confusingly similar names. AdGuard is a browser extension or app that only protects that one browser or device. AdGuard Home is a separate DNS server you run yourself, often on something like a Raspberry Pi, protecting every device on your network at once. |
| Admin (network admin) | Whoever manages the DNS server or router setup and can manually unblock a domain if a blocklist accidentally breaks something. Some list versions assume you have one on hand, others are built so you don't need one. |
| Allowlist (whitelist) | The opposite of a blocklist. Domains here always get through, even if a blocklist would otherwise catch them. |
| Blocklist (denylist) | A list of domains that get blocked so they can't load, usually to stop ads, trackers, or malware. |
| C2 server (command-and-control server) | A server attackers use to remotely control malware already running on infected devices. Threat Intelligence Feeds specifically target the domains these servers rely on. |
| Cisco Umbrella Top 1M | A ranking of the top 1 million most-visited domains, published by Cisco. Used here mainly to test the lists against a big batch of real, popular websites so nothing important breaks by accident. |
| CMP (Consent Management Platform/Provider) | The tech behind cookie consent pop-ups on websites, letting visitors choose what data a site can collect about them. Common examples include OneTrust, Cookiebot, and Usercentrics. |
| ControlD folder | A ControlD-specific feature for grouping custom rules into a reusable set you can apply across profiles. |
| Cryptojacking | When a website or app secretly uses your device's processing power to mine cryptocurrency in the background, usually without you noticing anything besides a slower device and a bigger power bill. |
| Denyallow / domain modifier | A rule type in filter lists used to carve out exceptions from a blocking rule. These modifiers have a technical length limit, so you can't cram unlimited exceptions into one rule, that's why exclusion lists sometimes stay short on purpose. |
| DGA (Domain Generation Algorithm) | A technique malware uses to automatically generate tons of random-looking domains on the fly, making it harder for defenders to block all of them in advance. |
| DNS (Domain Name System) | The system that translates website names, like example.com, into the numeric IP addresses computers use to find each other. Every blocklist works by intercepting these translations for unwanted domains. |
| DNS rebind protection | A safeguard against DNS rebinding attacks, where an attacker tricks a public domain into suddenly pointing at a private, local IP address to sneak into your home network. Available for AdGuard, AdGuard Home, and AdGuard DNS. |
| DNS resolver | The server that actually performs the DNS lookup for your device. AdGuard DNS, ControlD, RethinkDNS, and DNSwarden are all examples of resolvers that support these blocklists. |
| DNSMasq | A lightweight, widely used piece of software for DNS and DHCP, often running on routers or small home servers. One of the five formats these lists come in is built specifically for it. |
| Do53 | The classic, unencrypted way of doing DNS, over port 53. The name literally means "DNS over port 53", as opposed to encrypted options like DoH or DoT. |
| DoH / DoT (DNS-over-HTTPS / DNS-over-TLS) | Methods of encrypting DNS traffic so it can't be read or tampered with in transit. These can also bypass DNS-level blocklists by routing around your configured resolver, which is why a dedicated bypass list exists. |
| DoH3 / DoQ | Newer variants of encrypted DNS that run over QUIC instead of the older TCP-based connection, making lookups faster. Some DNS providers offer this as an extra connection option alongside regular DoH. |
| Dynamic DNS (DynDNS) | A service that gives a constantly changing IP address (common with home internet connections) a fixed, memorable domain name. Frequently abused for phishing campaigns, which is why there's a dedicated blocklist for it. |
| EDR (Endpoint Detection and Response) | A category of security software that watches individual devices for suspicious behavior and can respond automatically, more advanced than classic antivirus. Another extra protection layer these blocklists complement rather than replace. |
| False positive | A domain that gets mistakenly blocked even though it isn't actually harmful or unwanted, usually breaking a website or app feature. |
| Filter subscription | The setting in an ad blocker or DNS tool where you paste a blocklist's URL so the tool automatically downloads and keeps that list current, instead of you updating it by hand. |
| Fingerprinting | A tracking method that combines lots of small technical details about your device or browser to recognize you again, without needing a classic cookie. |
| GPL-3.0 | The GNU General Public License, version 3, an open-source license that allows redistribution and modification of the licensed material, as long as any redistributed or modified version is also published under the same license terms. |
| IDS/IPS (Intrusion Detection/Prevention System) | Security tools that watch network traffic for attack patterns. An IDS just flags suspicious activity, an IPS can actively block it. Another example of the extra protection layers these blocklists don't replace on their own. |
| IPv4 / IPv6 | Two versions of the internet protocol that hand out IP addresses. IPv4 uses the older, shorter-style addresses, IPv6 the newer, much longer ones. Some blocklists also ship as plain IP lists, since a domain could otherwise slip past a domain-only block by resolving over IPv6. |
| jsDelivr | A free content delivery network (CDN) that mirrors files straight from GitHub and npm onto a fast global server network. Links with @latest always point to the newest version of a file. Since jsDelivr caches everything, it keeps serving files even if GitHub is temporarily down, which is why the project uses jsDelivr links for some of its lists. |
| List tiers (Light/Normal/Pro/Pro++/Ultimate) | The five main strictness levels these blocklists come in, from Light (barely any restrictions) up to Ultimate (blocks aggressively, including some popular trackers). Each step up means more blocking power but also a higher chance something you actually wanted breaks. |
| Malware | An umbrella term for malicious software of all kinds, viruses, trojans, spyware, you name it, that infects a device, steals data, or lets someone else control it remotely. |
| Mirror | An exact copy of a project hosted elsewhere, for example on GitLab or Codeberg instead of GitHub. Acts as a backup source in case the main one is ever unreachable. |
| Native tracker | Trackers baked directly into devices, apps, or operating systems, think Amazon, Apple, Samsung, or Windows. They run quietly in the background collecting usage data, no matter which website you're actually visiting. |
| Network-wide blocking | Blocking domains for every device on a network at once, phones, laptops, smart TVs, everything, typically by changing the DNS server for the whole router. The opposite of a browser-only blocker, which only protects the browser it's installed in. |
| NRD (Newly Registered Domain) | A domain registered very recently, typically within the last 14 to 30 days. Threat actors often use fresh domains for scams or malware since they haven't been flagged by security tools yet. The underlying data for this project's NRD lists comes from Stamus Labs, a threat-research team that doesn't guarantee same-day updates. |
| Phishing | Scam attempts where fake websites or messages try to trick you into handing over passwords, banking details, or other sensitive info. |
| Pi-hole | A popular, free, open-source tool for running your own DNS server at home that blocks ads and trackers network-wide, commonly installed on a Raspberry Pi. |
| QUIC | A newer, UDP-based network protocol that sets up connections faster and encrypts them more efficiently than classic TCP. It's the foundation behind DoH3 and DoQ. |
| Referral domain | A domain used in affiliate or tracking links, commonly found on deal websites, in emails, and in search results. These typically only activate when a link is clicked, unlike ad domains, which load automatically. |
| RPZ (Response Policy Zone) | A DNS server feature (used by Bind, Knot, PowerDNS, and Unbound) that lets a resolver apply blocklists directly at the server level, instead of through a separate ad-blocking app. |
| Scam / fake shop | Fraudulent websites posing as fake online stores, bogus streaming sites, or hidden subscription traps, all designed to grab your money or your data. |
| TIF (Threat Intelligence Feeds) | A list built from security research sources that tracks domains actively known to be involved in malware, phishing, command-and-control servers, or other live threats. |
| TLD (Top-Level Domain) | The last segment of a domain name, like .com, .net, or a country code like .de. Some TLDs, like .top or .gdn, get abused for spam or scams way more often than others. |
| Top 1M list | A ranking of the one million most-visited domains on the internet, used to identify which domains are genuinely popular and worth extra trust. Umbrella, Cloudflare, Tranco, Chrome, DomCop, BuiltWith, and Majestic each publish their own version. |
| Tranco | A research-oriented ranking of the top million websites, built by averaging several other popularity rankings over a 30-day period, making it more stable and harder to manipulate than a single-source ranking. |
| uBlock Origin | A free, open-source ad and content blocker that runs as a browser extension. Works at the browser level, adding finer-grained filtering on top of a network-wide DNS blocklist. |
| VPN/TOR/Proxy bypass | Techniques that reroute traffic outside the local network's normal DNS path, which can accidentally or deliberately skip past blocklists. |
| Wildcard domain | A list format where each entry is just the domain itself, written either with a placeholder like an asterisk (*) or as a plain domain name, without listing every single subdomain separately. Since ad blockers automatically treat a blocked domain as covering all its subdomains too, the lists don't need to spell out every subdomain one by one, keeping them lean. Used by tools like Blocky, FRITZ!Box, or TechnitiumDNS. |

**[Back to top](#table-of-contents)**
