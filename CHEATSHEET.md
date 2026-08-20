# :bookmark_tabs: Blocklists Cheat Sheet <a name="top"></a>

A quick, scannable reference table for every list in this collection: what it blocks, how hard it hits, who it's best suited for, and the actual file name. Click a list name to jump to its full section in the [README](README.md). For the reasoning behind mini variants, NRD vs DGA, and bypass list relationships, see the [FAQ](FAQ.md#listrelationships).

### :bookmark_tabs: Table of Contents <a name="toc"></a>

1. [Quick Decision Guide](#quickguide)
2. [Multi (all-in-one protection)](#cheat_multi)
3. [Security and Threat Protection](#cheat_security)
4. [Bypass and Access Control](#cheat_bypass)
5. [Content and Lifestyle Filters](#cheat_content)
6. [Native Trackers and Referral Domains](#cheat_native)
7. [Inclusion Matrix](#inclusionmatrix)
8. [Recommended Combos](#combos)

---

### Quick Decision Guide <a name="quickguide"></a>

| If you want... | Use this |
|:----------------|:---------|
| The absolute basics, no admin available | [Light](README.md#light) |
| Solid everyday protection, low breakage risk | [Normal](README.md#normal) |
| A strong, balanced default | [Pro](README.md#pro) + [TIF](README.md#tif) |
| More aggressive than Pro, admin available | [Pro++](README.md#proplus) + [TIF](README.md#tif) |
| Maximum privacy and aggressive blocking | [Ultimate](README.md#ultimate) + [TIF](README.md#tif) |
| Normal-level protection, low-RAM device | [Light](README.md#light) ([mini-variant mapping](FAQ.md#listrelationships)) |
| Pro/Pro++/Ultimate-level protection, low-RAM device | [Pro Mini](README.md#promini) / [Pro++ Mini](README.md#proplusmini) / [Ultimate Mini](README.md#ultimatemini) |
| A safe network for kids | [Gambling](README.md#gambling) + [Anti Piracy](README.md#piracy) + [Safesearch](README.md#safesearch) + [DoH/VPN/TOR/Proxy Bypass](README.md#bypass) + [Social Networks](README.md#social) + [NSFW](README.md#nsfw) |
| Locked-down corporate or school network | [Bypass Full](README.md#bypass_all) + [Dynamic DNS](README.md#dyndns) + [Badware Hoster](README.md#hoster) |
| Threat hunting / reduced attack surface | [TIF](README.md#tif) + [Dynamic DNS](README.md#dyndns) + [Badware Hoster](README.md#hoster) + [Most Abused TLDs](README.md#tlds) + [NRD](README.md#nrd) ([NRD vs DGA](FAQ.md#listrelationships)) |
| Zero native device/app tracking, no exceptions | [Ultimate](README.md#ultimate) |

**[Back to top](#toc)**

---

### :books: Multi (all-in-one protection) <a name="cheat_multi"></a>

| List | What It Blocks | Blocking Level / Risk of Breakage | Best For | Caveats | File Name (Adblock) |
|:-----|:----------------|:---------------|:---------|:--------|:---------------------|
| [Multi Light](README.md#light) | Ads, trackers, metrics, some badware | Relaxed / Minimal | No admin, low-RAM setups | No crash/error trackers until Pro. This is Normal's own size-optimized version, just not named "Normal Mini" | `light.txt` |
| [Multi Normal](README.md#normal) | Ads, affiliate links, trackers, telemetry, phishing, malware, scams, fakes, cryptojacking | Relaxed to Balanced / Low | Everyday use | No crash/error trackers until Pro | `multi.txt` |
| [Multi Pro](README.md#pro) | Same categories as Normal, wider net (recommended default) | Balanced / Low to moderate | Admin available | Blocks a few referral domains, see [Referral Allowlist](FAQ.md#referral). First tier with crash/error trackers | `pro.txt` |
| [Multi Pro Mini](README.md#promini) | Pro, size-optimized | Balanced / Low to moderate | Pro-level, limited hardware | Top 1M/10M domains only | `pro.mini.txt` |
| [Multi Pro++](README.md#proplus) | Same as Pro, more aggressive | Balanced to Aggressive / Moderate | Experienced users, admin available | :warning: Every non-link-tracking referral domain blocked | `pro.plus.txt` |
| [Multi Pro++ Mini](README.md#proplusmini) | Pro++, size-optimized | Balanced to Aggressive / Moderate | Pro++-level, limited hardware | Top 1M/10M domains only | `pro.plus.mini.txt` |
| [Multi Ultimate](README.md#ultimate) | Strict cleanup, some popular trackers | Aggressive / High | Very experienced users, admin available | :warning: Can affect Facebook/WhatsApp, Microsoft/Xbox, IP-based site behavior | `ultimate.txt` |
| [Multi Ultimate Mini](README.md#ultimatemini) | Ultimate, size-optimized | Aggressive / High | Ultimate-level, limited hardware | Top 1M/10M domains only | `ultimate.mini.txt` |

**[Back to top](#toc)**

---

### :closed_lock_with_key: Security and Threat Protection <a name="cheat_security"></a>

| List | What It Blocks | Blocking Level | Best For | Caveats | File Name (Adblock) |
|:-----|:----------------|:---------------|:---------|:--------|:---------------------|
| [Fake](README.md#fake) | Scams, fake shops, fake streaming sites | Bundled in (see [Inclusion Matrix](#inclusionmatrix)) | Anti-phishing baseline | None known | `fake.txt` |
| [Pop-Up Ads](README.md#popupads) | Pop-up ads | Bundled in (see [Inclusion Matrix](#inclusionmatrix)) | Anti-pop-up baseline | Large list on its own | `popupads.txt` |
| [Threat Intelligence Feeds](README.md#tif) | Malware, cryptojacking, scams, spam, phishing, C2 domains | Standalone add-on | Security-focused, enough RAM | :warning: Too big for iOS AdGuard app, needs 2GB RAM in AdGuard Home, RPZ split into 2 files | `tif.txt` |
| [TIF Medium](README.md#tifmedium) | Trimmed TIF | Standalone | Struggles with full TIF | :warning: Needs 1GB RAM in AdGuard Home | `tif.medium.txt` |
| [TIF Mini](README.md#tifmini) | Trimmed TIF Medium (not the full list directly) | Standalone | Struggles with Medium | Reduced feed coverage | `tif.mini.txt` |
| [TIF IPs](README.md#tifips) | IPv4 companion to TIF | Standalone | Firewalls, IP-level coverage | Disable IPv6 resolution in AdGuard Home | `tif-ips.txt` |
| [NRD](README.md#nrd) | Every newly registered domain, no filtering | Aggressive | Allowlist-comfortable users | :warning: Higher false positives, data from Stamus Labs. Already includes all DGA domains | `nrd7.txt`, `nrd14-8.txt`, `nrd21-15.txt`, `nrd28-22.txt`, `nrd35-29.txt` |
| [DGA](README.md#nrd) | High-entropy NRDs only | Aggressive, narrower | Want just the malware subset | :warning: Subset of NRD, use instead of it, not alongside it, see [FAQ](FAQ.md#listrelationships) | `dga7.txt`, `dga14.txt`, `dga30.txt` |
| [Dynamic DNS](README.md#dyndns) | Dynamic DNS abused for phishing | Standalone | Security-focused admins | None known | `dyndns.txt` |
| [Badware Hoster](README.md#hoster) | Hosting providers abused for malware | Standalone | Accepts collateral damage | :warning: Blocks legit sites on same hosts. ControlD folder available | `hoster.txt` |
| [Most Abused TLDs](README.md#tlds) | Entire high-abuse TLDs (`.top`, `.shop`, `.gdn`) | Aggressive | Strong anti-spam/scam | :warning: Some legit sites caught. Multiple format variants, see [README](README.md#tlds) | `spam-tlds.txt`, `spam-tlds-ublock.txt`, `spam-tlds-adblock.txt`, `spam-tlds-adblock-aggressive.txt` + `spam-tlds-adblock-allow.txt` |
| [DNS Rebind Protection](README.md#dnsrebind) | Local-network rebind attacks | Standalone, not in any tier | AdGuard / AdGuard Home / AdGuard DNS users | Works with all three AdGuard products | `dns-rebind-protection.txt` |

**[Back to top](#toc)**

---

### :outbox_tray: Bypass and Access Control <a name="cheat_bypass"></a>

| List | What It Blocks | Blocking Level | Best For | Caveats | File Name (Adblock) |
|:-----|:----------------|:---------------|:---------|:--------|:---------------------|
| [Bypass Full](README.md#bypass_all) | Encrypted DNS, VPN, TOR, proxy services | Standalone | Corporate/parental lockdown | Also block ports 53 and 853 outbound. See [how the three bypass lists relate](FAQ.md#listrelationships) | `doh-vpn-proxy-bypass.txt` |
| [Bypass, DoH only](README.md#bypass_dns) | Encrypted DNS servers only | Standalone | Narrower scope than Full | Same port-blocking requirement | `doh.txt` |
| [Bypass, DoH IPs](README.md#bypass_ips) | IPv4 of encrypted DNS servers | Standalone | IP-level firewall coverage | Companion to DoH-only, not to Full. Disable IPv6 resolution in AdGuard Home | `doh-ips.txt` |
| [Safesearch Not Supported](README.md#safesearch) | Search engines skipping Safesearch | Standalone | Parental/admin Safesearch enforcement | None known | `nosafesearch.txt` |
| [URL Shortener](README.md#urlshortener) | All known link shorteners | Standalone, not in any tier | High-security environments | :warning: Can break legit short links | `urlshortener.txt` |

**[Back to top](#toc)**

---

### :underage: Content and Lifestyle Filters <a name="cheat_content"></a>

| List | What It Blocks | Blocking Level | Best For | Caveats | File Name (Adblock) |
|:-----|:----------------|:---------------|:---------|:--------|:---------------------|
| [Anti Piracy](README.md#piracy) | Piracy, illegal streaming/download sites | Standalone | Off-network piracy | None known | `anti.piracy.txt` |
| [Gambling](README.md#gambling) | Gambling sites and content | Standalone | Parental/workplace filtering | None known | `gambling.txt` |
| [Gambling Medium](README.md#gamblingmedium) | Trimmed Gambling | Standalone | Struggles with full list | Reduced coverage | `gambling.medium.txt` |
| [Gambling Mini](README.md#gamblingmini) | Trimmed Gambling Medium (not the full list directly) | Standalone | Struggles with Medium | Top 1M/10M domains only | `gambling.mini.txt` |
| [Social Networks](README.md#social) | Facebook, Instagram, TikTok, X, Snapchat | Standalone | Digital detox, focus filtering | Doesn't touch WhatsApp or Twitch | `social.txt` |
| [NSFW](README.md#nsfw) | Adult content | Standalone | Workplace/school networks | None known | `nsfw.txt` |

**[Back to top](#toc)**

---

### :calling: Native Trackers and Referral Domains <a name="cheat_native"></a>

| List | What It Blocks | Blocking Level | Best For | Caveats | File Name (Adblock) |
|:-----|:----------------|:---------------|:---------|:--------|:---------------------|
| [Native Tracker](README.md#native) | Device/app/OS trackers (Amazon, Apple, Huawei, Microsoft, Samsung, TikTok, LG webOS, Roku, Vivo, OPPO/Realme, Xiaomi) | Bundled in all tiers at varying strength, see [README](README.md#native) | Extra device-specific coverage | May need manual unblocking per device | `native.amazon.txt`, `native.apple.txt`, `native.huawei.txt`, `native.winoffice.txt`, `native.samsung.txt`, `native.tiktok.txt`, `native.tiktok.extended.txt`, `native.lgwebos.txt`, `native.roku.txt`, `native.vivo.txt`, `native.oppo-realme.txt`, `native.xiaomi.txt` |
| [Referral Allowlist](FAQ.md#referral) | Not a blocklist, keeps affiliate/tracking links working | Deliberate carve-out | Pro++/Ultimate users | Only referral list with a ControlD folder | `whitelist-referral.txt`, `whitelist-referral-native.txt` |
| [Referral Blocklist](FAQ.md#referral) | Referral/affiliate tracking domains | Aggressive, opt-in | Advanced users, browser blocker preferred | :warning: Breaks search results, newsletter unsubscribe links. No ControlD folder | `blocklist-referral-native.txt` |

See the FAQ's [referral breakdown](FAQ.md#referral) for exactly which domains get blocked at which tier.

**[Back to top](#toc)**

---

### Inclusion Matrix <a name="inclusionmatrix"></a>

Which lists are already included (fully or partially) in each Multi tier.

| List | Light | Normal | Pro | Pro++ | Ultimate | TIF |
|:-----|:-----:|:------:|:---:|:-----:|:--------:|:---:|
| [Fake](README.md#fake) | :x: | :green_circle: | :green_circle: | :green_circle: | :green_circle: | :green_circle: |
| [Pop-Up Ads](README.md#popupads) | :yellow_square: | :yellow_square: | :green_circle: | :green_circle: | :green_circle: | :yellow_square: |
| [TIF / Medium / Mini](README.md#tif) | :x: | :yellow_square: | :yellow_square: | :yellow_square: | :yellow_square: | -- |
| [Native Tracker](README.md#native) | :yellow_square: | :yellow_square: | :yellow_square: | :yellow_square: | :green_circle: | -- |
| [Bypass, Safesearch, DynDNS, Hoster, Shortener, TLDs, Rebind, Piracy, Gambling, Social, NSFW](README.md#dyndns) | :x: | :x: | :x: | :x: | :x: | :x: |

:green_circle: fully included · :yellow_square: partially included · :x: not included

**[Back to top](#toc)**

---

### Recommended Combos <a name="combos"></a>

| Use Case | Combo |
|:---------|:------|
| Balanced daily driver | [Pro](README.md#pro) + [TIF](README.md#tif) |
| Stronger native tracker coverage | [Pro](README.md#pro)/[Pro++](README.md#proplus) + [TIF](README.md#tif) + specific [Native Tracker](README.md#native) devices, or [Ultimate](README.md#ultimate) for full coverage |
| More aggressive than Pro | [Pro++](README.md#proplus) + [TIF](README.md#tif) |
| Maximum protection | [Ultimate](README.md#ultimate) + [TIF](README.md#tif) |
| Kid-safe home network | [Gambling](README.md#gambling) + [Anti Piracy](README.md#piracy) + [Safesearch](README.md#safesearch) + [DoH/VPN/TOR/Proxy Bypass](README.md#bypass) + [Social Networks](README.md#social) + [NSFW](README.md#nsfw) |
| Corporate/school lockdown | [Pro++](README.md#proplus) + [Bypass Full](README.md#bypass_all) + [Dynamic DNS](README.md#dyndns) + [Badware Hoster](README.md#hoster) |
| Threat hunting | [TIF](README.md#tif) + [Dynamic DNS](README.md#dyndns) + [Badware Hoster](README.md#hoster) + [Most Abused TLDs](README.md#tlds) + [NRD](README.md#nrd) |
| Threat hunting, lower noise | Same as above, swap [NRD](README.md#nrd) for [DGA](README.md#nrd) |
| Pro-level, low-RAM | [Pro Mini](README.md#promini) + [TIF Mini](README.md#tifmini) |
| Normal-level, low-RAM | [Light](README.md#light) + [TIF Mini](README.md#tifmini) |

> [!TIP]
> Pair any DNS-level combo with a browser content blocker like [AdGuard](https://adguard.com), [uBlock Origin](https://github.com/uBlockOrigin/), or [Ghostery](https://www.ghostery.com/). See [Recommendation](README.md#recommendation).

**[Back to top](#toc)**

---

> [!NOTE]
> Entry counts and inclusion status can shift as the lists get rebuilt. Always check the [live README](README.md) and [FAQ](FAQ.md) for current numbers and policy details.

---

Back to [Table of Contents](README.md#bookmark_tabs-table-of-contents) of the main README.
