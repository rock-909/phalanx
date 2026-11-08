# Sources, Licenses, and Local Changes

Every rule file here is derived from upstream work. This document records
where each one came from, what license it carries, and what was changed.

---

## AdDomain.list — 395 rules

| | |
|---|---|
| **Upstream** | [Thoseyearsbrian/Aegis](https://github.com/Thoseyearsbrian/Aegis) — `rules/AdDomain.list` |
| **Upstream license** | Apache License 2.0 |
| **Snapshot** | 2026-11-07 |
| **Upstream size** | 395 rules |

### Local changes

1. **Fixed a typo that broke the entire file.**
   Upstream line 27 reads `OMAIN-SUFFIX,company-target.com` (missing the `D`).
   Surge parses rule sets as a whole — one malformed line causes it to
   discard the file entirely, silently disabling all 395 rules.
   Fixed to `DOMAIN-SUFFIX`.

### Sync

```bash
tools/vendor-rules.sh --sync AdDomain
```

---

## OverseasAI.list — 407 rules

| | |
|---|---|
| **Upstream** | [viewer12/OverseasAI.list](https://github.com/viewer12/OverseasAI.list) |
| **Upstream license** | GNU General Public License v2.0 |
| **Snapshot** | 2026-11-07 |
| **Upstream size** | 498 rules |

### Local changes

**Removed 91 rules** that belong to other policy groups. The upstream file
is an aggregate that bundles OpenAI, Claude, Gemini, Cloudflare and Stripe
domains into a single list — which forces downstream users to write
override rules just to route them correctly.

| Removed | Count | Belongs to |
|---|---|---|
| Google / Gemini / Bard / AI Studio | 57 | `Google` |
| OpenAI / ChatGPT / Sora | 19 | `OpenAI` |
| Claude / Anthropic | 10 | `Claude` |
| Cloudflare | 4 | `Cloud` |
| Stripe | 2 | `Stripe` |

The remaining 407 rules cover AI services that have **no dedicated policy
group** — small and mid-size tools such as Weights & Biases, Weaviate,
Zed, v0, Vast.ai, You.com, Chroma, Warp, and similar.

### Sync

```bash
tools/vendor-rules.sh --sync OverseasAI
```

---

## MediaRotate.list — 50 rules

| | |
|---|---|
| **Upstream** | Original work — no upstream |
| **License** | MIT |
| **Last verified** | 2026-10-14 |

Domains that need a rotating exit IP (media sites that rate-limit or
block by IP). Each entry was verified live on 2026-10-14.

---

## Also consumed directly (not vendored)

The Surge configuration subscribes to these upstream rule sets **directly**,
without vendoring. They are not stored in this repository, but they are part
of the same rule pipeline — listed here so the full picture is visible.

### Thoseyearsbrian/Aegis — 17 rule sets

Apache License 2.0 · https://github.com/Thoseyearsbrian/Aegis

| Rule set | Purpose |
|---|---|
| `Malware_IOC_Block.list` | Malware indicators of compromise |
| `Pegasus_Block.list` | NSO Group spyware |
| `Backdoor_Block.list` | Backdoor domains |
| `Botnet_Block.list` | Botnet command nodes |
| `Phishing_Block.list` | Phishing sites |
| `Scam_Block.list` | Scam sites |
| `PCDNDomain.list` | PCDN domains |
| `BehaviorDomain.list` | Behaviour tracking |
| `Quarantine_Block.list` | Observation quarantine (pre-matching) |
| `OpenAI.list` | OpenAI + Claude + third-party AI services |
| `Crypto.list` | Crypto services |
| `GitHub.list` | GitHub |
| `Google.list` | Google services |
| `Apple.list` | Apple services |
| `RegionalRouting.list` | Regional routing |
| `China.list` | China direct-connect |
| `Proxy.list` | General proxy fallback |

### blackmatrix7/ios_rule_script — 19 rule sets

GPL-2.0 · https://github.com/blackmatrix7/ios_rule_script

Streaming: `Netflix` `Disney` `HBO` `Spotify` `YouTube`
Social: `Telegram` `TikTok` `Facebook` `Twitter` `Weibo` `WeChat`
Services: `PayPal` `Microsoft` `OneDrive` `Docker` `Cloudflare` `Tesla` `Speedtest` `GoogleSearch`

### Loyalsoldier/surge-rules — 5 rule sets

GPL-3.0 · https://github.com/Loyalsoldier/surge-rules

| Rule set | Purpose |
|---|---|
| `private.txt` | Private / LAN addresses |
| `reject.txt` | Ad blocking (~45,000 rules) |
| `icloud.txt` | iCloud services |
| `telegramcidr.txt` | Telegram IP ranges |
| `cncidr.txt` | China IP ranges |

> **Known issue**: `reject.txt` contains `siftscience.com`, which is an
> Anthropic anti-fraud service. Blocking it breaks Claude login. The Surge
> configuration works around this by placing an explicit
> `DOMAIN-SUFFIX,siftscience.com,"Claude"` rule **before** the `reject.txt`
> subscription. A permanent fix would require vendoring `reject.txt`.

---

## License implications

`OverseasAI.list` is derived from a **GPL-2.0** project. Under GPL-2.0,
distributing a derivative work requires the derivative to also be
distributed under GPL-2.0.

`AdDomain.list` is derived from an **Apache-2.0** project, which requires
preserving copyright notices (see [NOTICE](NOTICE)).

The MIT license in [LICENSE](LICENSE) applies to the original work in this
repository (README, SOURCES.md, tooling). **Rule files retain their
upstream licenses** as described above.

---

## Acknowledgements

These projects do the hard work of tracking domains that change daily.
This repository would not exist without them.

- **Thoseyearsbrian/Aegis** — security and routing rule sets
- **viewer12/OverseasAI.list** — AI service aggregation
- **blackmatrix7/ios_rule_script** — streaming, social, developer categories
- **Loyalsoldier/surge-rules** — IP layer and ad blocking
