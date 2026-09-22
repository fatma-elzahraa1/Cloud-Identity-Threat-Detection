# False Positive Analysis

A detection is only as useful as the analyst's ability to correctly dismiss
benign matches. Two cases from this dataset were investigated and confirmed
as false positives.

---

## Case 1 — reem.saeed@company.com (Impossible Travel Rule)

**Alert:** Login from Egypt, then Netherlands, 40 minutes apart.

| Field | Event 1 | Event 2 |
|---|---|---|
| Country | Egypt | Netherlands |
| Device | Laptop-RS03 | Laptop-RS03 |
| Browser | Chrome 128 | Chrome 128 |
| OS | Windows 11 | Windows 11 |

**Investigation:** Device, browser, and OS are identical across both events —
only the source IP and resulting geolocation changed. It is highly unlikely
an attacker would replicate the exact same device fingerprint as the
legitimate user.

**Conclusion:** False Positive — most likely explained by the user routing
their traffic through a corporate or personal VPN with an Amsterdam exit
node.

**Tuning action:** IP `34.90.10.5` added to a trusted allowlist
(see `../detections/detection_tuning.md`).

---

## Case 2 — yara.mostafa@company.com (Impossible Travel Rule)

**Alert:** Login from Cairo, then Giza, 12 minutes apart.

| Field | Event 1 | Event 2 |
|---|---|---|
| City | Cairo | Giza |
| Country | Egypt | Egypt |
| Device | Laptop-YM08 | Laptop-YM08 |
| Browser | Chrome 128 | Chrome 128 |

**Investigation:** Same device/browser/OS across both events. Unlike the
`reem.saeed` case, this alert did not actually fire on the country-level
detection logic (Cairo and Giza are both in Egypt), but is documented here
because a more granular, city-level version of the rule would have flagged
it. Cairo and Giza are adjacent metropolitan areas roughly 20km apart, making
a 12-minute transition physically plausible by car.

**Conclusion:** False Positive — benign city-level GeoIP variance, not
cross-border travel. Documented as a limitation to watch for if the
detection is ever tightened to compare cities instead of countries.

---

## Takeaway
Both false positives shared a common signal that separated them from true
positives: **identical device/browser/OS fingerprint** across the two
events. In both confirmed true-positive cases (fatma.tarek, omar.youssef),
the second event came from an unrecognized device. Device consistency is
therefore one of the strongest single indicators for triaging this alert
type.
