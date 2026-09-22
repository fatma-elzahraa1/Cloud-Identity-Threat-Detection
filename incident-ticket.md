# Incident Ticket

**Title:** Suspicious Cloud Authentication — Possible Account Compromise

**Severity:** High

**Status:** Closed — Remediation Recommended

**Affected User:** fatma.tarek@company.com

**Source IP:** 172.98.55.20 (AS 15830 — Equinix, data-center/hosting range)

**Detection Rule:** Impossible Travel (`detections/detection_1_impossible_travel.spl`)

---

## Summary
The affected account authenticated successfully from Cairo, Egypt at 09:00
UTC, followed by a second successful authentication from New York, USA at
09:15 UTC — a 15-minute gap that is not physically achievable through
legitimate travel. The second sign-in originated from a device never
previously associated with the account and from an IP address belonging to a
hosting/data-center provider rather than a residential ISP.

## Evidence
- Sign-in log entries (see `investigation/timeline_fatma_tarek.md`)
- VirusTotal IP enrichment: 0/89 malicious flags, ASN = Equinix (EMEA)
  Acquisition Enterprises B.V.
- Device fingerprint mismatch: `Laptop-FT02` (known) vs. `Unknown-Device`
  (never seen before)

## Investigation
1. Confirmed the account's normal baseline device/location via prior
   sign-in history.
2. Verified the second sign-in's IP was not associated with any known VPN
   exit range used by this organization's employees (unlike the confirmed
   false positive on `reem.saeed@company.com`).
3. Confirmed timing gap makes legitimate travel impossible.
4. Cross-referenced IP reputation via VirusTotal — no blocklist hits, but
   ASN ownership (data center) is inconsistent with normal end-user traffic.

## MITRE ATT&CK
T1078 — Valid Accounts (see `investigation/mitre_attack_mapping.md`)

## Recommended Actions
- [ ] Revoke all active sessions for fatma.tarek@company.com
- [ ] Force immediate password reset
- [ ] Require MFA re-registration
- [ ] Review all resource access performed during the New York session
      (files accessed, emails sent, forwarding rules created, etc.)
- [ ] Block source IP 172.98.55.20 (and monitor the /23 range) at the
      identity provider / conditional access level if supported
- [ ] Check whether the same source IP or ASN appears against any other
      account in the environment
- [ ] Notify the account owner directly to confirm whether the New York
      login was recognized

## Analyst Notes
This ticket demonstrates the full triage-to-response workflow for a
cloud-identity "impossible travel" alert, including how it was distinguished
from a confirmed false positive on a different account using the same
underlying detection rule (see `investigation/false_positive_analysis.md`).
