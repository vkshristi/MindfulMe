# MindfulMe — Risk Register

**Purpose:** Track product & engineering risks with clear owners and mitigations.  
**Review cadence:** Weekly during MVP (then monthly).

## How to use

- Add one row per risk.
- Keep **Impact** and **Likelihood** in the scales below.
- Update **Status** promptly when mitigations ship.

### Scales

- **Impact:** Low / Medium / High / Critical  
  - Low: Minor inconvenience; no user data at risk  
  - Medium: Noticeable degradation; small user segment affected  
  - High: Major feature failing or sensitive data exposure risk  
  - Critical: Legal/compliance exposure, widespread outage, or confirmed breach
- **Likelihood:** Rare / Unlikely / Possible / Likely / Almost Certain

### Owners

- Assign a single DRI (Directly Responsible Individual).  
- For cross-cutting items, set Owner as **Engineering** or **Product** and note collaborators.

---

## Risk Table

| ID | Risk | Description | Impact | Likelihood | Mitigation | Owner | Status | Target Date |
|----|------|-------------|--------|------------|------------|-------|--------|-------------|
| R-001 | Data Privacy | Wellness logs, mood, sleep are sensitive PII; improper storage/ exposure could harm users and violate policies. | High | Possible | Encrypt in transit (TLS) and at rest (DB-level). Minimize PII. Add data export + deletion endpoints. Restrict prod DB access. Security review in CI. | Engineering | Open | 2025-09-20 |
| R-002 | Over-Nudging | Excessive notifications (morning briefs, pre-focus nudges) may annoy users and drive churn. | Medium | Likely | Notification center with user preferences, quiet hours, per-day nudge caps, relevance thresholds. Metrics to monitor opt-outs. | Product | Open | 2025-09-22 |
| R-003 | Model Bias (FlowScore v2) | Learned model may overweight certain routines, disadvantaging others; trust may drop. | Medium | Possible | Start with explainable rule-based v1; add model cards + offline validation; keep human-readable reasons; provide fallback to v1; A/B canary. | ML | Open | 2025-10-10 |
| R-004 | Cron Drift (Morning Brief) | Time-based jobs may drift or fail (timezones, DST, worker restarts). | Low | Possible | Use BullMQ with timezone-aware schedules; idempotent jobs; drift detection & retries; SLO alerts if run deviates >5 min; health dashboard. | Engineering | Open | 2025-09-25 |

---

## Monitoring & Alerts

- **Privacy:** Sentry for PII in logs (block), infrastructure IAM audits quarterly.
- **Nudges:** Track send rate, open rate, mute/opt-out rate; cap per user per day.
- **Models:** Track prediction drift and miscalibration; maintain a simple dashboard.
- **Jobs:** Alert if job runtimes deviate >5 minutes from schedule or job fails consecutively.

## Change Log

- 2025-09-09: Register created with initial four risks (R-001..R-004).
