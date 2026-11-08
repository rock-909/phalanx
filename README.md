# Phalanx

> *Phalanx: the interlocking shield wall of ancient Greek infantry —
> each shield covers the man beside it.*

Curated Surge rulesets. Upstream sources are verified, filtered, and
re-assembled into boundary rules for a personal network.

## Why this repo exists

Surge can subscribe to rule sets by URL. But upstream lists often:

- mix domains that belong to different policy groups
- carry typos that make Surge discard the **entire file**
- stop being maintained without notice

This repo holds the **processed** versions: same coverage, fewer surprises.
Configuration files (which contain credentials) live in a separate private repo.

## Files

| File | Rules | Purpose |
|---|---|---|
| `AdDomain.list` | 395 | Ad / tracking domains |
| `OverseasAI.list` | 407 | AI services not covered by dedicated groups |
| `MediaRotate.list` | 50 | Media sites needing rotating exit IPs |

## Usage

```ini
RULE-SET,https://raw.githubusercontent.com/rock-909/phalanx/main/AdDomain.list,Advertising,no-resolve
RULE-SET,https://raw.githubusercontent.com/rock-909/phalanx/main/OverseasAI.list,"OverseasAI"
RULE-SET,https://raw.githubusercontent.com/rock-909/phalanx/main/MediaRotate.list,MediaRotate,no-resolve
```

## Attribution

See [SOURCES.md](SOURCES.md) for:

- upstream projects for each vendored file, their licenses, and exactly what was changed
- the rule sets consumed **directly** from upstream (not vendored) — 41 additional
  rule sets from Aegis, blackmatrix7 and Loyalsoldier that complete the pipeline

## License

MIT — see [LICENSE](LICENSE).
Rule content derived from third-party projects remains under their
original licenses; see SOURCES.md.
