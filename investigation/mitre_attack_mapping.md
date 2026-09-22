# MITRE ATT&CK Mapping

| Technique ID | Name | Tactic | How it applies here |
|---|---|---|---|
| T1078 | Valid Accounts | Defense Evasion, Persistence, Privilege Escalation, Initial Access | All four detections in this project center on legitimate credentials being used from unexpected devices, locations, or timing patterns — not on exploiting a technical vulnerability. |
| T1621 | Multi-Factor Authentication Request Generation | Credential Access | Relevant to Detection 4 (MFA Anomaly): the pattern of repeated MFA failures followed by a success is consistent with an attacker repeatedly generating MFA prompts to pressure the victim into approving one by mistake ("MFA fatigue/bombing"). |

## Important Caveat
Matching T1078 is **not proof of compromise on its own**. Valid credentials
can be used legitimately in ways that superficially resemble this technique —
for example, an employee traveling for work or connecting through a
corporate VPN (see `false_positive_analysis.md`). The technique ID describes
*what the attacker did*, not *whether an attack occurred*. Determining that
requires the surrounding context: device history, IP reputation/ASN type,
and timing — which is exactly the investigation work carried out in this
project.

Reference: https://attack.mitre.org/techniques/T1078/
