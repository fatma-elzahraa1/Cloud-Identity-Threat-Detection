# Detection Tuning — Impossible Travel Rule

## Problem
The initial version of `detection_1_impossible_travel.spl` triggered on **4 events**.
Investigation (see `../investigation/false_positive_analysis.md`) showed that
1 of the 4 (25%) was a false positive caused by legitimate corporate VPN usage.

## Investigation Summary

| Alert | User | Verdict | Reason |
|---|---|---|---|
| Egypt → United States (15 min) | fatma.tarek | True Positive | New device, hosting-provider IP (Equinix) |
| Egypt → Germany (20 min) | omar.youssef | True Positive | New device, hosting-provider IP (velia.net) |
| Germany → Brazil (15 min) | omar.youssef | True Positive | Continuation of same session hijack |
| Egypt → Netherlands (40 min) | reem.saeed | **False Positive** | Same device/browser/OS both events — corporate VPN exit node |

## Tuning Action
Added the identified VPN exit IP (`34.90.10.5`) to a trusted allowlist directly
in the detection logic:

```spl
| where NOT SourceIP IN ("34.90.10.5")
```

## Result

| Metric | Before Tuning | After Tuning |
|---|---|---|
| Total alerts | 4 | 3 |
| True positives | 3 | 3 |
| False positives | 1 (25%) | 0 |

**Noise reduced by 25% with zero loss of true-positive detection coverage.**

## Production Note
In a real environment, this would be implemented more robustly via a maintained
CIDR-range lookup table of known corporate VPN/proxy egress ranges, rather than
a single hardcoded IP — this simplified version demonstrates the tuning
methodology on a small dataset.
