# strategy-hunt

Multi-model bug-hunting automation for the **Strategy (MicroStrategy) Bug Bounty Program**.

- **Scope**: Strategy software + Strategy-owned web domains + the provided **authenticated** test envs:
  - `https://bugbounty.cloud.microstrategy.com/MicroStrategyLibrary/`
  - `https://bugbounty.cloud.microstrategy.com/MicroStrategy/`
  - Login via **Gmail OIDC** (own account + space; shared env, wiped periodically)
- **Disclosure**: filedrop portal — https://share.microstrategy.com/filedrop/vulnerabilityreporting
- **Rewards** (CVSS v4.0, PayPal): Critical $500–2000, High $500–1000, Medium $200–500, Low $20–200
- **SLAs**: first response 5bd / triage 15bd / bounty 30bd

## Strategy exclusions (no reward — long list, see scope.yml)
`demo.microstrategy.com`, automated scanner output, brute force, DoS, MITM, enumeration, open
redirects, HTML injection/self-XSS, missing headers/cookie flags/SSL/HSTS, clickjacking/UI-redressing,
CSV injection, content spoofing, known-vuln libs w/o exploit, version disclosure w/o POE, **unverified
AI-generated reports**, internal pivoting, mass account creation.

## What pays
Real exploitable issues in MicroStrategy software demonstrated on the provided env: SQLi, XSS,
path traversal, deserialization, auth bypass, IDOR on reports/projects/folders, privilege escalation,
session issues. Only interact with accounts you own; minimal proof; do not access Strategy data beyond
proving the vuln.

## Reporting
One vuln per report (chain if needed for impact). Filedrop portal: summary, repro steps, severity,
attachments. Confidentiality is strict — no external disclosure without written consent.
