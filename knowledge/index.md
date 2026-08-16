# Strategy (MicroStrategy) hunt KB — verified learnings (RAG for all models)
> BEST-in-class opportunity: Strategy provides AUTHENTICATED test environments —
> https://bugbounty.cloud.microstrategy.com/MicroStrategyLibrary/ (Library) and
> /MicroStrategy/ (Web), login via Gmail OIDC (own account + space, shared env,
> wiped periodically). Up to $2,000/vuln (CVSS v4.0 tiered). Report via filedrop
> portal: https://share.microstrategy.com/filedrop/vulnerabilityreporting
> What pays: real exploitable issues in Strategy software (MicroStrategy BI platform:
> Library, Web, Intelligence Server, REST API, mobile) — historical MSTR classes:
> SQLi, XSS, path traversal, deserialization, auth bypass, IDOR on reports/dashboards.

## REJECTED CLASSES (policy — do not propose; long list, see scope.yml)
- REJECTED demo.microstrategy.com (explicitly excluded host).
- REJECTED automated scanner output, brute force, DoS, MITM, enumeration.
- REJECTED missing headers/cookie flags/SSL/TLS/HSTS/pinning (w/o direct vuln).
- REJECTED clickjacking/UI-redressing/tapjacking/tabnabbing w/o sensitive actions.
- REJECTED open redirects, HTML injection, self-XSS, CSV injection, content spoofing.
- REJECTED known-vuln-lib w/o exploitability, version disclosure w/o POE.
- REJECTED "unverified AI-generated vulnerability reports" — human verification REQUIRED.

## ALIVE SURFACE FACTS (verified)
- 2026-08-16 policy: private program, approved researchers, up to $2,000. Targets =
  software published by Strategy at community.microstrategy.com/s/products + Strategy-
  owned web domains + the two provided authenticated envs (bugbounty.cloud.microstrategy.com).
  SLAs: first response 5bd, triage 15bd, bounty 30bd. (setup seed, live status UNVERIFIED)
- 2026-08-16 (setup seed) MicroStrategy products: Library (web BI), Web/MicroStrategy
  (legacy web), Intelligence Server, REST API (demo.microstrategy.com/MicroStrategyLibrary/api-docs),
  mobile. Known public API surface documented. Authenticated env allows REAL access-control
  testing (IDOR on reports/projects/folders, user impersonation, privilege escalation).

## OPEN QUESTIONS
- Live status of bugbounty.cloud.microstrategy.com env (fingerprint: version, headers)
- Whether OIDC (Gmail) account creation is required before testing — user must create
  one Gmail-based account in the shared env (allowed: own account only)
- Which Strategy version is deployed in the test env (affects CVE targeting)

## FINDING INBOX (validated = move to reports/)
- (empty)
- 2026-08-16 FINGERPRINT: bugbounty.cloud.microstrategy.com LIVE. Server header "ESF" (Google Front End). All pages redirect to accounts.google.com OIDC signin. /MicroStrategyLibrary/api/status → 200 {"upSince":1783458549431,"upTime":"947 Hours","isIServerConfigured":true,"deploymentType":"mce"} — UNAUTH info endpoint. Everything else on Library REST API (/api/help, /api/sessions, /api/objects, /api/plugins, swagger) → 401 ERR009 "session expired" (auth-gated). /MicroStrategyLibrary/api-docs/ 200 (Swagger UI shell) but swagger.json 404.
- 2026-08-16 TRIAGER NOTE: /api/status unauth disclosure = uptime + deploymentType only. Per Strategy policy EXCLUSIONS: "Information disclosures" and "Software version disclosure without POE" are NOT eligible. Do NOT report as standalone. Value is in AUTHENTICATED testing (IDOR/privesc on reports/dashboards/folders, auth bypass, SQLi in filter/expression params) - requires Gmail OIDC login to the provided env (HUMAN step: user logs in with own Gmail, then provides session/token for analysis, or tests manually).
- 2026-08-16 NEXT (HUMAN): user logs into https://bugbounty.cloud.microstrategy.com/MicroStrategyLibrary/ with Gmail OIDC → then we can map authenticated surface (projects, folders, documents, REST API with session) and hunt IDOR/privesc/business-logic. Everything passive is exhausted (all auth-gated).
