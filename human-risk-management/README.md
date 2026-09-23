# Human Risk Management Program

## Context
A mid-size organization ran monthly phishing simulations and annual security awareness training campaigns for all staff, using Microsoft Defender for Office 365's Attack Simulation Training. Results were exported monthly and visualized in Power BI for leadership reporting. The reporting process was heavily manual, and the program itself was operating largely at a "compliance tracking" level rather than a data-driven risk-management level.

## Role
Owned the monthly Human Risk Management (HRM) program end-to-end — running simulations and training campaigns, producing the recurring reporting pipeline, and later redesigning both the data pipeline and the underlying program framework.

## What I Did

**Program operation**
- Ran monthly phishing simulations and annual security awareness training campaigns for the organization using Microsoft Defender for Office 365 (Attack Simulation Training).
- Owned the monthly reporting cycle: exporting simulation and training-completion data, enriching it with organizational attributes (department, location) sourced from HR data, and delivering it to leadership via Power BI dashboards.

**Pipeline redesign (in progress)**
- Identified that the existing process — dozens of manual spreadsheet exports each reporting period, manual cleanup, manual enrichment against a separately-maintained staff list, manual reload — was the main bottleneck limiting the program's maturity, and that exported figures were stale the moment they were pulled since people kept completing training afterward.
- Authored a full design & implementation plan (in review, not yet built) for a scheduled, source-driven pipeline to replace it entirely:
  - **Ingest** — a scheduled cloud automation flow pulls phishing-simulation results, training-campaign status, and directory attributes (department, location, title) directly from source via API, eliminating both the manual export and the dependency on a separately-maintained HR spreadsheet.
  - **Multi-entity design** — the organization runs multiple business units on separate identity tenants, and directory/security APIs are tenant-scoped, so the design uses a "fan-in" pattern: an independent, least-privilege service connection per tenant, each stamped with its source, landing in one shared governed data store rather than one direct multi-tenant query.
  - **Store & model** — raw pulls land in staging tables; a modeled schema (canonical person, monthly training facts, monthly phishing facts, an identity-alias crosswalk, and a run-audit table) preserves person-level grain so multi-month accountability metrics can be computed.
  - **Transform** — a documented, evidence-derived rule set (e.g. collapsing many per-module rows into one monthly completion status, handling excluded/broken training content, defining phishing funnel signals, and multi-month non-completion and high-risk-cohort logic) specifies the transform layer precisely, rather than leaving cleanup as tribal knowledge.
  - **Identity reconciliation** — designed a crosswalk process to resolve the same person appearing under different logins/domains across systems (cross-entity aliases, directory typos, duplicate accounts), plus automated match-rate monitoring so a data-quality regression is caught immediately rather than discovered in a report.
  - **Serve & orchestrate** — a BI layer reads the governed store and reproduces the existing reporting with an explicit "as-of" timestamp; the pipeline runs unattended on a schedule with failure and data-quality alerting.
  - **Governance built in from the design stage** — flagged the cross-tenant/cross-entity data consolidation as a privacy-impact-assessment question (not just a technical one) requiring sign-off before go-live, scoped per-tenant admin consent as a parallel (not sequential) workstream, and defined a phased rollout with named ownership at each phase.

**Program & framework design**
- Built a maturity model for the program across four stages — Compliance → Behavior → Culture → Quantified Risk — and assessed where the existing program sat on that curve.
- Defined a recurring operating loop for the program: Measure → Score → Segment → Intervene → Assure.
- Designed a weighted Human Risk Score model combining phishing susceptibility, reporting behavior, training currency, and other risk signals, normalized and trackable at individual and department level.

## Tools & Technology
Microsoft Defender for Office 365 (Attack Simulation Training) · Microsoft Graph API · Power Automate · Dataverse · Power BI · Power Query (M) · Entra ID (Azure AD) · Excel

## Outcome / Impact
- Ran the monthly reporting cycle manually for a multi-entity organization across several hundred users and multiple business tenants, while designing its replacement.
- Authored a complete, phase-by-phase pipeline design (data model, transformation rule set, identity reconciliation approach, and governance/privacy plan) now in stakeholder review — the automation build itself is a work in progress, not yet live.
- Interim manual reconciliation work directly informed the pipeline design: real edge cases (duplicate identities, directory typos, broken training content, cross-entity logins) were caught and solved by hand first, so the automated rules are evidence-derived rather than theoretical.
- Moved the program from pure compliance tracking toward a measurable, segmented risk-management model with a defined score and escalation path.
- Currently use organizational AI tools (e.g. Claude) to accelerate sifting through raw monthly data and drafting reports that capture the relevant human risk metrics — cutting the time spent on manual synthesis before it reaches leadership.

