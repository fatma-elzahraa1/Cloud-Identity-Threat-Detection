# Investigation Timeline — fatma.tarek@company.com

## Alert Trigger
Detection: Impossible Travel Rule (`detections/detection_1_impossible_travel.spl`)

## Events

| Time (UTC) | Event | Location | IP | Device | Result |
|---|---|---|---|---|---|
| 09:00:00 | Sign-in | Cairo, Egypt | 41.238.10.9 | Laptop-FT02 (known device) | Success |
| 09:15:00 | Sign-in | New York, USA | 172.98.55.20 | Unknown-Device | Success |

## Analyst Observation
The same account authenticated successfully from two geographically distant
locations (Cairo and New York) within a 15-minute window, which is not
achievable through legitimate physical travel. The second authentication
originated from a device never previously associated with this user. This
combination raises the likelihood that the account's credentials were
compromised and used from an unauthorized location, though this has not yet
been confirmed with the account owner.

## IP Enrichment (VirusTotal)
- IP: 172.98.55.20
- Reputation: 0/89 vendors flagged as malicious (no known blocklist match)
- ASN: AS 15830 — Equinix (EMEA) Acquisition Enterprises B.V.
- Note: Equinix is a data-center/hosting provider, not a residential ISP.
  A login from a data-center IP is inconsistent with typical end-user network
  behavior and is commonly associated with VPS/proxy usage by attackers.
- Minor GeoIP discrepancy: VirusTotal returned Canada (CA) as the registered
  country for the ASN, while the sign-in log geolocation reported New York, US.
  This is a known limitation of IP-based geolocation and does not change the
  core finding.

## MITRE ATT&CK Mapping
- **T1078 — Valid Accounts**: The attacker authenticated using legitimate,
  valid credentials rather than exploiting a technical vulnerability. Note
  that T1078 alone is not proof of compromise — surrounding context (device,
  IP reputation, timing) is what elevates this from benign to suspicious.

## Severity
**High** — new device + hosting-provider IP + physically impossible timing,
with no confirmed legitimate explanation (unlike the reem.saeed VPN case).

## Recommended Actions
- Revoke all active sessions for this account
- Force password reset
- Review MFA registration / require MFA re-enrollment
- Review all activity performed during the New York session
- Check whether IP 172.98.55.20 or its /23 range appears against any other
  account in the environment
