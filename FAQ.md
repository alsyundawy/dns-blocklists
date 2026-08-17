# FAQ

A practical guide to how these DNS blocklists are built, which version fits your setup, and why certain domains are deliberately left unblocked. If a term looks unfamiliar, check the [Glossary](#glossary) at the bottom.

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
11. [Glossary](#glossary)

---

## <a name="sources"></a> 1. Where does the data come from, and how are the lists built?

These lists aren't simple copy-paste jobs from other sources. Each version is put together from a mix of foundational sources, custom extensions, domain categories, Newly Registered Domains (NRDs), and several Top 1M lists, including Umbrella, Cloudflare, Tranco, Chrome, and DomCop. False positives and dead domains get removed on an ongoing basis, and domains reported by the community get added in too.

On top of that, network logs are regularly reviewed to spot domains worth blocking that haven't made it onto a list yet. The full base list currently holds around 45 million domains, including entries from the Top 1M lists going back more than 24 months. This combined list is also used to figure out which domains are genuinely popular and worth tracking, which helps keep the blocklists accurate over time instead of static.

Every list is also tested before release against a large sample of real websites, to check that pages, navigation, images, and videos keep working as expected.

The complete list of base sources is documented here: [sources](sources.md).

**[Back to top](#table-of-contents)**

---

## <a name="whatshouldiuse"></a> 2. Which list version should I use?

Pick the version that matches how much technical support you have on hand and how much risk of breakage you're willing to accept. As a rule of thumb: the stricter the list, the more protection you get, but also the higher the chance something you actually want to use gets blocked by mistake.

The risk of breakage ratings below are a qualitative guide, not measured error rates. They reflect how aggressively each version filters domains and how likely it is to catch something you didn't want blocked, not a specific false-positive percentage.

| Version | Best for | Risk of breakage |
|:---|:---|:---|
| [Light](README.md#light) | No admin available to unblock things, or an ad blocker that can't handle large lists | Minimal. Built to avoid restrictions almost entirely |
| [Normal](README.md#normal) | Everyday use, same audience as Light | Low. Restrictions are rare and usually minor |
| [Pro](README.md#pro) | Setups with an admin nearby who can unblock domains if needed | Low to moderate. Recommended default for solid privacy with minimal hassle |
| [Pro++](README.md#proplus) | Experienced users with an admin available | Moderate. May include some false positives that limit functionality |
| [Ultimate](README.md#ultimate) | Very experienced users with an admin available | High. Deliberately blocks some popular trackers, which can limit app or website functionality |

> [!WARNING]
> Ultimate has specific, documented side effects worth knowing before you switch to it:
> - **Meta/Facebook:** some Meta trackers are blocked, which restricts Facebook and Facebook Messenger. WhatsApp's graph trackers are also blocked, affecting avatar creation, the in-app help center, and video effects. Other WhatsApp features are unaffected.
> - **Windows/Xbox:** some Microsoft trackers are blocked, affecting features like Windows Spotlight and Xbox Live Achievements Activity History.
> - **Location and IP trackers:** blocking these improves privacy but can trigger extra CAPTCHAs, wrong regional settings, or reduced site functionality on some services.
>
> If you hit one of these issues, check the maintainer's known-unblock lists for [Meta](share/facebook.txt) and [Microsoft](share/microsoft.txt), or the general [known issues list](share/ultimate-known-issues.txt).

> [!IMPORTANT]
> Whenever possible, pair your main list with the [Threat Intelligence Feeds (TIF)](README.md#tif) list for extra protection against malicious domains. If your ad blocker struggles with the full TIF list's size, use the smaller [medium](README.md#tifmedium) or [mini](README.md#tifmini) version instead. On AdGuard Home or AdGuard DNS, also consider adding [Dandelion Sprout's Anti-Malware List](https://raw.githubusercontent.com/DandelionSprout/adfilt/master/Alternate%20versions%20Anti-Malware%20List/AntiMalwareAdGuardHome.txt). There's also an [IPv4 list](README.md#tifips) you can run alongside the TIF full, medium, or mini list.

**Extra lists worth adding, depending on your goals:**

- **Security focus:** combine TIF with the [Dynamic DNS](README.md#dyndns) list (blocks dynamic DNS services abused in phishing), the [Badware Hoster](README.md#hoster) list (blocks hosting providers whose infrastructure is frequently abused for malware), the [Most Abused TLDs](README.md#tlds) list (blocks entire top-level domains with poor reputations, like `.top` or `.gdn`), and the [NRD](README.md#nrd) lists.
- **Protecting kids:** use the [Gambling](README.md#gambling), [Anti Piracy](README.md#piracy), [Safesearch](README.md#safesearch), [DoH/VPN/TOR/Proxy Bypass](README.md#bypass), [Social Networks](README.md#social), and [NSFW](README.md#nsfw) lists together. Note that the Social Networks list only blocks traditional social platforms (Facebook, Instagram, TikTok, X, Snapchat), not messaging apps like WhatsApp or streaming platforms like Twitch.

> [!NOTE]
> **You usually don't need to add the Fake or Pop-Up Ads lists separately, they're already built in, though coverage varies by version:**
> - The [Fake](README.md#fake) list (scam shops, fake streaming sites, cost traps) is not included in Light at all. It's fully included in Normal, Pro, Pro++, Ultimate, and in TIF, TIF medium, and TIF mini.
> - The [Pop-Up Ads](README.md#popupads) list is fully included in Pro, Pro++, and Ultimate. It's only partially included in Normal and partially in TIF, so if you run Normal together with the full TIF list, Pop-Up Ads is fully covered in that combination.
>
> Two other specialized lists sit outside the main tiers and are not included in any of them by default: [URL Shortener](README.md#urlshortener) (recommended mainly for high-security environments, since it can break legitimate short links) and [DNS Rebind Protection](README.md#dnsrebind) (AdGuard/AdGuard Home only, blocks attackers from resolving external domains to your local network's private IP addresses). Add these only if your environment specifically calls for them.

**[Back to top](#table-of-contents)**

---

## <a name="formats"></a> 3. Which format should I use for my ad blocker or DNS server?

Every list is published in five formats. Pick the row that matches your ad blocker or DNS server, all other rows contain the same data, just structured differently for that specific tool.

| Format | Use it with |
|:---|:---|
| Adblock | Pi-hole, AdGuard, AdGuard Home, eBlocker, uBlock Origin, Brave (aggressive mode only), AdBlock-Fast, AdNauseam, Little Snitch Mini |
| DNSMasq | DNSMasq (v2.86 or newer), Diversion (v5 or newer) |
| Wildcard (Asterisk) | Blocky (v0.23 or newer), Nebulo, NetDuma, OPNsense, YogaDNS |
| Wildcard (Domains only) | DNSCloak, DNSCrypt, FRITZ!Box (FRITZ!OS v8.40 or newer), TechnitiumDNS, adblock-lean, PersonalDNSfilter, InviZible Pro |
| RPZ | Bind, Knot, PowerDNS, Unbound, and other software supporting Response Policy Zones |

A few lists deviate from this pattern:

- The full **TIF** list is too large for AdGuard (browser extension) and needs at least 2 GB of RAM in AdGuard Home. Its RPZ version is also split into two files, both are required.
- **Most Abused TLDs** ships in AdGuard-specific, uBlock Origin-specific, and RPZ-specific variants instead of the standard five, since it relies on exclusion rules that work differently across tools.
- **DNS Rebind Protection** is only available for AdGuard and AdGuard Home.
- **NRD/DGA** lists are only available as Adblock and plain domain lists.
- Several lists (**Badware Hoster**, **Most Abused TLDs**, referral allow/block lists) also ship as a ControlD folder for direct import into a ControlD profile.

**[Back to top](#table-of-contents)**

---

## <a name="quicksetup"></a> 4. Quick setup guide

A minimal path to get protection running, without needing to read the rest of this FAQ first.

1. **Pick a version.** If you're unsure, start with [Pro](README.md#pro), it's the maintainer's own recommendation for solid protection with minimal breakage. See [section 2](#whatshouldiuse) if you want a different balance of strictness versus risk.
2. **Identify your setup.** Are you blocking DNS network-wide (Pi-hole, AdGuard Home, TechnitiumDNS, OPNsense) or using a browser content blocker (uBlock Origin, AdGuard browser extension)? Network-wide blocking protects every device on your network; a browser blocker only protects that browser.
3. **Get the right format.** Check [section 3](#formats) for the format your tool expects, then copy the corresponding list URL from the [main list overview](README.md#overview) into your tool's blocklist or filter subscription settings.
4. **Add Threat Intelligence Feeds (TIF).** Add the [TIF](README.md#tif) list (or its medium/mini version if your tool struggles with size) alongside your main list for stronger protection against malware and phishing.
5. **No self-hosted DNS server?** Use one of the [online DNS services](#availablelists) instead, they let you enable these lists without running your own infrastructure.
6. **Layer in a browser content blocker too.** DNS-level blocking stops most ads, trackers, and malware, but it can't catch everything, some ads and scripts load from domains that are otherwise legitimate. Adding a browser content blocker like uBlock Origin or AdGuard alongside your DNS blocklist closes that gap. Treat the DNS list as your network-wide baseline and the browser blocker as the finer-grained layer on top.
7. **Test it.** Browse normally for a day. If something breaks, check [section 2](#whatshouldiuse) for known side effects (especially relevant for Pro++ and Ultimate), unblock the specific domain in your tool, and see [section 10](#support) if you need to report a false positive or a missed domain.
8. **Keep it current.** These lists update regularly. If your tool doesn't auto-refresh subscribed lists, set a periodic re-download, and see [section 8](#mirrors) if you need the freshest possible data.

**[Back to top](#table-of-contents)**

---

## <a name="referral"></a> 5. Why aren't referral domains blocked?

Referral domains are the affiliate and tracking links you often see on deal sites like Slickdeals, in emails, or in search results. They're allowed in these lists on purpose, because they're normally only triggered when someone manually clicks a link, not to serve ads automatically.

Blocking them would break things like the first result link in a search, and some of these domains are also used for unsubscribing from newsletters, so blocking them can trap you into unwanted emails instead of freeing you from them.

Here's how it breaks down by list version:

- **Light and Normal:** all referral domains are allowed.
- **Pro:** most referral domains are still allowed, but a few get blocked if they're mainly used for other tracking purposes or are commonly tied to scam or spam links, even though they could technically also be used for link tracking.
- **Pro++ and Ultimate:** every referral domain that isn't used exclusively for link tracking gets blocked, including domains like `ad.doubleclick.net`, `adservice.google.*`, `app.adjust.*`, and `analytics.adjust.*`.

**Allowlist** (contains all known link trackers, so they stay unblocked):

| Format | Link |
|:---|:---|
| Adblock (AdGuard, AdGuard Home, uBlock Origin, etc.) | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/whitelist-referral.txt) |
| Adblock (Pi-hole v6+, TechnitiumDNS, etc.) | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/whitelist-referral-native.txt) |
| Wildcard<br>domains | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/whitelist-referral-onlydomains.txt) |
| ControlD<br>folder | [Download](https://github.com/hagezi/dns-blocklists/blob/main/controld/referral-allow-folder.json) |

If you actually want to block referral domains (**not recommended**), use the lists below. It's better to apply these in a browser content blocker like uBlock Origin rather than network-wide at the DNS level, since DNS-level blocking is much harder to fine-tune once it's live.

| Format | Link |
|:---|:---|
| Adblock | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adblock/blocklist-referral-native.txt) |
| Wildcard<br>domains | [Download](https://raw.githubusercontent.com/hagezi/dns-blocklists/main/wildcard/blocklist-referral-onlydomains.txt) |

**[Back to top](#table-of-contents)**

---

## <a name="cmps"></a> 6. Why aren't CMPs (cookie consent tools) blocked?

Blocking CMPs network-wide breaks a lot of websites and takes away your ability to actually choose what you consent to. In practice, blocking a CMP usually just makes the site treat everything as accepted anyway, since the site can no longer show you the consent choice in the first place ([see this discussion](https://github.com/hagezi/dns-blocklists/issues/1979#issuecomment-1870498567)).

Deciding whether to block or auto-allow a specific CMP is a job for content blockers with dedicated filter lists, since those tools can tell which sites should be excluded from blocking a given CMP domain and which shouldn't. If you look at the exclusion lists used by established cookie filter lists, it becomes pretty clear why blanket DNS-level blocking isn't a good idea: it can't make the nuanced, per-site decisions a proper filter list can.

**[Back to top](#table-of-contents)**

---

## <a name="availablelists"></a> 7. Which lists are available on which DNS services?

Not every DNS provider offers every list. Here's the current availability:

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
> A few other free services bundle these lists with fixed presets rather than letting you pick individual list versions: [HaGeZi DNS](https://github.com/hagezi/dns-servers) (EU resolvers running Pro + TIF), [DNSBUNKER.org](https://dnsbunker.org/) (Pro + TIF), [Public RDNS](https://public-rdns.com/) (aggressive, family-safe preset), [RobinGroppe.de](https://www.robingroppe.de/serverzeug/dns-server) (TIF only), and [OpenBLD.net](https://openbld.net/docs/get-started/third-party-filters/hagezi/) (Pro + TIF). RethinkDNS updates its copies of the lists only once a week, so expect a short lag there compared to the source repository.

**[Back to top](#table-of-contents)**

---

## <a name="mirrors"></a> 8. How current is the data, and where can I get it?

The primary source for all lists is the [GitHub repository](https://github.com/hagezi/dns-blocklists). The GitHub repository and its two full mirrors, GitLab and Codeberg, are updated in sync, once a day:

| Source | Update frequency |
|:---|:---|
| [GitHub/jsDelivr](https://github.com/hagezi/dns-blocklists) (primary) | Once a day |
| [gitlab.com/hagezi/mirror](https://gitlab.com/hagezi/mirror) | Once a day, in sync with GitHub |
| [codeberg.org/hagezi/mirror2](https://codeberg.org/hagezi/mirror2) | Once a day, in sync with GitHub |
| [hagezi-mirror.dnsbunker.org](https://hagezi-mirror.dnsbunker.org) | Every 4 to 8 hours |

> [!TIP]
> If you need the freshest possible data, use [hagezi-mirror.dnsbunker.org](https://hagezi-mirror.dnsbunker.org). It's connected directly to the build system and receives each new list version as soon as it's built, ahead of the daily GitHub, GitLab, and Codeberg update.

> [!NOTE]
> The GitHub repository is occasionally compressed and reinitialized to reduce its overall size. This invalidates existing forks and resets the commit history, which is worth knowing if you maintain a fork or rely on commit history for change tracking.

**[Back to top](#table-of-contents)**

---

## <a name="licensing"></a> 9. Licensing and liability

The lists are published under the GPL-3.0 license and can be redistributed, modified, or adapted, but only within the terms of that license. Review the license in the repository before redistributing the lists as part of your own product or service.

The maintainer publishes the lists as-is, with no warranty of accuracy, completeness, or fitness for a particular purpose, and no guarantee that every malicious domain is caught or that no legitimate domain is ever blocked by mistake. Use is entirely at your own risk; the maintainer disclaims liability for damages arising from use or misuse of the lists, except in cases of willful misconduct or gross negligence.

Practically, this means the lists should be treated as one layer in a broader security setup, not a standalone solution. They don't replace firewalls, antivirus or EDR tools, intrusion detection systems, or your own risk assessment. For the full legal text, see the [Disclaimer section](README.md#disclaimer) in the repository.

**[Back to top](#table-of-contents)**

---

## <a name="support"></a> 10. Getting help and reporting issues

If a legitimate domain gets blocked, or you spot a domain that should be blocked but isn't, report it through the [issue tracker](https://github.com/hagezi/dns-blocklists/issues) on GitHub. This is the fastest way to get a false positive corrected or a gap in coverage closed. Alternatively, you can reach us by email at [support@hagezi.org](mailto:support@hagezi.org).

For general questions or discussion, use the [GitHub Discussions](https://github.com/hagezi/dns-blocklists/discussions) page. A public [Matrix support chat](https://matrix.to/#/#hagezi-support:tchncs.de?via=tchncs.de) is also available if you prefer a more direct, conversational channel. If you'd like to contact us personally, [support@hagezi.org](mailto:support@hagezi.org).

**[Back to top](#table-of-contents)**

---

## <a name="glossary"></a> 11. Glossary

| Term | What it means |
|:---|:---|
| Adblock format | One of the formats these lists come in. It looks like classic ad blocker filter rules and works with tools like Pi-hole, AdGuard, AdGuard Home, and uBlock Origin. |
| AdGuard / AdGuard Home | Two different things with confusingly similar names. AdGuard is a browser extension or app that only protects that one browser or device. AdGuard Home is a separate DNS server you run yourself, often on something like a Raspberry Pi, that protects every device on your network at once. |
| Admin (network admin) | Whoever manages the DNS server or router setup and can manually unblock a domain if a blocklist accidentally breaks something. Some list versions assume you have one on hand, others are built so you don't need one. |
| Allowlist (whitelist) | The opposite of a blocklist. Domains on this list always get through, even if a blocklist would otherwise catch them. |
| Blocklist (denylist) | A list of domains that get blocked so they can't load, most often used to stop ads, trackers, or malware. |
| C2 server (command-and-control server) | A server attackers use to remotely control malware that's already running on infected devices. Threat Intelligence Feeds specifically target the domains these servers rely on. |
| Cisco Umbrella Top 1M | A ranking of the top 1 million most-visited domains, published by Cisco. Used here mainly to test the lists against a big batch of real, popular websites so nothing important accidentally breaks. |
| CMP (Consent Management Platform/Provider) | The technology behind the cookie consent pop-ups on websites, letting visitors choose what data a site is allowed to collect about them. Common examples include OneTrust, Cookiebot, and Usercentrics. |
| ControlD folder | A ControlD-specific feature for grouping custom rules into a manageable, reusable set that can be applied across profiles. |
| Cryptojacking | When a website or app secretly uses your device's processing power to mine cryptocurrency in the background, usually without you noticing anything besides a slower device and a higher power bill. |
| Denyallow / domain modifier | A rule type used in filter lists to carve out exceptions from a blocking rule. These modifiers have a technical length limit, so you can't cram unlimited exceptions into a single rule, that's why exclusion lists sometimes stay short on purpose. |
| DGA (Domain Generation Algorithm) | A technique malware uses to automatically generate large numbers of random-looking domains on the fly, making it harder for defenders to block every one of them in advance. |
| DNS (Domain Name System) | The system that translates website names, like example.com, into the numeric IP addresses computers use to find each other. Every blocklist works by intercepting these translations for unwanted domains. |
| DNS rebind protection | A safeguard against DNS rebinding attacks, where an attacker tricks a public domain into suddenly pointing at a private, local IP address to sneak into your home network. Currently only available for AdGuard and AdGuard Home. |
| DNS resolver | The server that actually performs the DNS lookup for your device. AdGuard DNS, ControlD, RethinkDNS, and DNSwarden are all examples of resolvers that support these blocklists. |
| DNSMasq | A lightweight, widely used piece of software for DNS and DHCP, often running on routers or small home servers. One of the five formats these lists are published in is built specifically for it. |
| Do53 | The classic, unencrypted way of doing DNS, over port 53. The name is literally short for "DNS over port 53", as opposed to encrypted options like DoH or DoT. |
| DoH / DoT (DNS-over-HTTPS / DNS-over-TLS) | Methods of encrypting DNS traffic so it can't be read or tampered with in transit. These can also be used to bypass DNS-level blocklists by routing around your configured resolver, which is why a dedicated bypass list exists. |
| DoH3 / DoQ | Newer variants of encrypted DNS that run over QUIC instead of the older TCP-based connection, making the lookup faster. Some DNS providers offer this as an extra connection option alongside regular DoH. |
| Dynamic DNS (DynDNS) | A service that gives a constantly changing IP address (common with home internet connections) a fixed, memorable domain name. Frequently abused for phishing campaigns, which is why there's a dedicated blocklist for it. |
| EDR (Endpoint Detection and Response) | A category of security software that watches individual devices for suspicious behavior and can respond automatically, more advanced than classic antivirus. It's one of the extra protection layers these blocklists are meant to complement, not replace. |
| False positive | A domain that gets mistakenly blocked even though it isn't actually harmful or unwanted, usually causing a website or app feature to break. |
| Filter subscription | The setting in an ad blocker or DNS tool where you paste a blocklist's URL so the tool automatically downloads and keeps that list up to date, instead of you updating it by hand. |
| Fingerprinting | A tracking method that combines lots of small technical details about your device or browser to recognize you again, without needing to set a classic cookie. |
| GPL-3.0 | The GNU General Public License, version 3, an open-source license that allows redistribution and modification of the licensed material, provided that any redistributed or modified version is also published under the same license terms. |
| IDS/IPS (Intrusion Detection/Prevention System) | Security tools that watch network traffic for attack patterns. An IDS just flags suspicious activity, an IPS can actively block it. Another example of the extra protection layers these blocklists don't replace on their own. |
| IPv4 / IPv6 | Two versions of the internet protocol that hand out IP addresses. IPv4 uses the older, shorter-style addresses, IPv6 the newer, much longer ones. Some blocklists also ship as plain IP lists, since a domain could otherwise slip past a domain-only block by resolving over IPv6. |
| jsDelivr | A free content delivery network (CDN) that mirrors files straight from GitHub and npm onto a fast global server network. Links with @latest always point to the newest version of a file. Since jsDelivr caches everything, it also keeps serving files even if GitHub itself is temporarily down, which is why the project uses jsDelivr links for some of its lists. |
| List tiers (Light/Normal/Pro/Pro++/Ultimate) | The five main strictness levels these blocklists come in, from Light (barely any restrictions) up to Ultimate (blocks aggressively, including some popular trackers). Each step up means more blocking power but also a higher chance something you actually wanted breaks. |
| Malware | An umbrella term for malicious software of all kinds, viruses, trojans, spyware, you name it, that infects a device, steals data, or lets someone else control it remotely. |
| Mirror | An exact copy of a project hosted somewhere else, for example on GitLab or Codeberg instead of GitHub. Acts as a backup source in case the main one is ever unreachable. |
| Native tracker | Trackers baked directly into devices, apps, or operating systems, think Amazon, Apple, Samsung, or Windows. They run quietly in the background collecting usage data, regardless of which website you're actually visiting. |
| Network-wide blocking | Blocking domains for every device on a network at once, phones, laptops, smart TVs, everything, typically by changing the DNS server for the whole router. The opposite of a browser-only blocker, which only protects the browser it's installed in. |
| NRD (Newly Registered Domain) | A domain registered very recently, typically within the last 14 to 30 days. Threat actors often use fresh domains for scams or malware because they haven't been flagged by security tools yet. The underlying data for this project's NRD lists comes from Stamus Labs, a threat-research team that does not guarantee same-day updates. |
| Phishing | Scam attempts where fake websites or messages try to trick you into handing over passwords, banking details, or other sensitive info. |
| Pi-hole | A popular, free, open-source tool for running your own DNS server at home that blocks ads and trackers network-wide, commonly installed on a Raspberry Pi. |
| QUIC | A newer, UDP-based network protocol that sets up connections faster and encrypts them more efficiently than classic TCP. It's the foundation behind DoH3 and DoQ. |
| Referral domain | A domain used in affiliate or tracking links, commonly found on deal websites, in emails, and in search results. These typically only activate when a link is clicked, unlike ad domains, which load automatically. |
| RPZ (Response Policy Zone) | A DNS server feature (used by Bind, Knot, PowerDNS, and Unbound) that lets a resolver apply blocklists directly at the server level, rather than through a separate ad-blocking application. |
| Scam / fake shop | Fraudulent websites posing as fake online stores, bogus streaming sites, or hidden subscription traps, all designed to grab your money or your data. |
| TIF (Threat Intelligence Feeds) | A list built from security research sources that tracks domains actively known to be involved in malware, phishing, command-and-control servers, or other live threats. |
| TLD (Top-Level Domain) | The last segment of a domain name, such as .com, .net, or a country code like .de. Some TLDs, like .top or .gdn, are far more commonly abused for spam or scams than others. |
| Top 1M list | A ranking of the one million most-visited domains on the internet, used to identify which domains are genuinely popular and worth extra trust. Umbrella, Cloudflare, Tranco, Chrome, DomCop, BuiltWith, and Majestic each publish their own version. |
| Tranco | A research-oriented ranking of the top million websites, built by averaging several other popularity rankings over a 30-day period, making it more stable and harder to manipulate than a single-source ranking. |
| uBlock Origin | A free, open-source ad and content blocker that runs as a browser extension. It works at the browser level, adding finer-grained filtering on top of a network-wide DNS blocklist. |
| VPN/TOR/Proxy bypass | Techniques that reroute traffic outside the local network's normal DNS path, which can accidentally or deliberately skip past blocklists. |
| Wildcard domain | A list format where each entry is just the domain itself, written either with a placeholder like an asterisk (*) or as a plain domain name, without listing every single subdomain separately. Since ad blockers automatically treat a blocked domain as covering all of its subdomains too, the lists don't need to spell out unnecessary subdomains one by one, which keeps them lean. Used by tools like Blocky, FRITZ!Box, or TechnitiumDNS. |

**[Back to top](#table-of-contents)**
