Design a modern, enterprise “Platform Ops Command Center” web application UI for a Platform Engineering / SRE team. The app unifies observability and incident response across ServiceNow, Splunk Observability (APM/infrastructure), Splunk (logs), BigPanda (event correlation), AppDynamics (APM), MongoDB (database health), and other sources. The UI should look like a professional Ops console (Grafana-style), clean and dense but readable, optimized for fast triage. Provide a full set of screens with consistent navigation, components, empty/loading/error states, and dark mode variants.

GLOBAL STYLE + LAYOUT
- Overall style: modern SaaS, “observability console” aesthetic similar to Grafana or Splunk dashboards; high information density; crisp typography; subtle shadows; rounded cards; accessible color contrast; responsive layout (desktop-first).
- Color: neutral slate/gray background; clear severity colors (critical=red, high=orange, medium=yellow, low=blue/gray, healthy=green).
- Layout: left vertical navigation bar + top header bar.
- Top header includes: global search, environment selector (Prod/Stage/Dev), time range picker, region selector, notifications icon, user menu.
- Use consistent component library: cards, tables, pills/tags, timeline, charts, sparklines, modals, side panels (drawers), toasts, skeleton loading.
- Support multi-tenant RBAC with “Team / Service Ownership” concepts in UI (show team badges, ownership chips).

APP NAVIGATION (LEFT SIDEBAR)
- Overview
- Services
- Incidents
- Triage Queue
- Event Correlation
- Changes (Release Guardrails)
- Dashboards
- Runbooks & Knowledge
- Automation
- Settings / Integrations

SCREEN 1: OVERVIEW (Ops Overview)
Purpose: single-pane view for current health and priorities.
Layout:
- Top row: 4 KPI cards with sparklines: Active Incidents, Critical Alerts, Services at Risk (SLO), Alert Volume (last 60m).
- Main grid:
  - “Service Health Map” heatmap or grid by business service with status (green/yellow/red) and quick drill-down.
  - “Top Incident List” table (ServiceNow) with columns: Priority, Title, Service, Start, Owner, Status, Link.
  - “BigPanda Correlated Clusters” list with cluster size, top symptoms, impacted services.
  - “Splunk Observability Highlights” panel: anomaly cards (latency spike, error rate, saturation).
- Right side fixed “AI Briefing” panel:
  - auto-generated summary for last 30–60 minutes with bullets: what changed, biggest risks, recommended focus.
  - Each bullet has evidence links (chips) to incident, cluster, dashboard, log query.
- Include empty state: “No active incidents—show proactive risks and upcoming changes.”

SCREEN 2: SERVICES (Service Catalog / My Services)
Purpose: service directory, ownership, dependencies, golden signals.
Layout:
- Header: search + filters (Owner Team, Tier, Env, Region, Tag, Language/runtime).
- Service list as cards or table with: service name, tier badge, owner team, current health, last deploy, SLO status, incident count (7d).
- Clicking a service opens “Service Detail” page (or split view):
  - Tabs: Overview | Golden Signals | Dependencies | Deployments | Incidents | Runbooks
  - Service Overview: mini dashboards (latency p95/p99, error rate, throughput, saturation) + current alert badges.
  - Dependency graph (interactive): upstream/downstream services, databases, queues; highlight impacted nodes.
  - “Related Telemetry” quick links: Splunk Observability APM view, Splunk logs saved searches, AppDynamics BTs.
  - AI Copilot mini-panel: “Explain current health” + “Suggest next checks”.

SCREEN 3: INCIDENTS (List + Incident Workspace)
A) Incidents List
- Table view from ServiceNow with advanced filters: Priority, Service, Team, Status, Assignment group, Time range, Major incident toggle.
- Row actions: open workspace, assign, add note, link cluster, create bridge link.

B) INCIDENT WORKSPACE (most detailed screen)
Purpose: unified triage view combining evidence + actions.
Layout: 3-column layout with timeline below.
Left column: Incident summary card
- Title, priority badge, status, impacted service(s), customer impact estimate, assignee, on-call team.
- Buttons: “Update ServiceNow”, “Create Status Update”, “Start War Room”, “Link BigPanda Cluster”.
- “Linked entities” chips: service, CI, deployment, dashboards, runbook.

Center column: Evidence panels (scrollable)
- Section: “Symptoms”
  - Top errors (from Splunk logs) as a ranked list with counts and trend arrows
  - APM anomalies (Splunk Observability and AppDynamics) as cards (latency spike, error rate, slow DB calls)
- Section: “Metrics”
  - 3–5 key time-series charts (latency, error rate, throughput, saturation)
- Section: “Logs”
  - Embedded log viewer snippet + suggested saved searches
  - Buttons: “Open in Splunk”, “Copy query”, “Pin to timeline”
- Section: “Database” (MongoDB health)
  - cards: connections, slow queries, replication lag, CPU/mem, lock %, cache evictions
- Each panel includes a “source badge” (SNOW / Splunk / SObs / BigPanda / AppD / MongoDB).

Right column: AI COPILOT (persistent drawer)
- Conversation UI with system actions:
  - Quick buttons: Summarize, Likely Root Cause, Next Best Actions, Draft Update, Recommend Mitigation, Find Similar Incident, Open Relevant Runbook
- Copilot outputs must include:
  - Confidence score
  - Evidence citations as clickable chips (log query, metric panel, incident, change record)
  - “Assumptions” section
- “Suggested Actions” checklist with one-click: create note, assign to team, open dashboard, run query (links), propose suppression request.

Bottom: Timeline (horizontal)
- Events: alert fired, correlation cluster formed, deploy occurred, change approved, incident created, notes added, mitigations executed.
- Filter by event type; click opens details.

Include states:
- Loading skeletons for evidence panels
- Partial data warning (“Splunk Observability API delayed”)
- Permissions banner (“You don’t have access to this service’s logs”)

SCREEN 4: TRIAGE QUEUE (Prioritization / What to work on next)
Purpose: reduce noise; rank clusters/incidents by impact.
Layout:
- Top: scoring explanation tooltip (Impact, Blast Radius, Novelty, Confidence).
- Two tabs: “Correlated Clusters” and “Incidents”
- Each item as a card:
  - Rank number, severity color bar
  - Impacted services, cluster size, start time, current trend
  - AI summary: 2–3 lines
  - Buttons: “Open Workspace”, “Mark Duplicate”, “Silence Request”, “Escalate”
- Right side: “Queue Insights” panel with AI: “Top 3 focus items and why.”

SCREEN 5: EVENT CORRELATION (BigPanda + topology)
Purpose: explore alert storms and relationships.
Layout:
- Split view:
  - Left: clusters list (filters: severity, service, time, noise level)
  - Right: correlation visualization (graph or grouped timeline) showing how alerts collapse into clusters
- Cluster detail shows: top signals, impacted services, probable root cause candidates, suppressed alerts, and evidence links.

SCREEN 6: CHANGES / RELEASE GUARDRAILS
Purpose: connect incidents with changes; risk scoring.
Layout:
- Calendar or timeline of changes (ServiceNow change records + CI/CD deploy feed)
- Change list table: Change ID, service, window, approver, risk score, status.
- Change Detail page:
  - Risk score breakdown (blast radius, change type, past failure history)
  - “Related Incidents” and “Related Alerts” panels
  - AI section: “Could this change explain current symptoms?” with evidence links
  - Buttons: “Request rollback”, “Notify change owner”, “Open runbook for rollback”

SCREEN 7: DASHBOARDS
Purpose: curated dashboards per team/service.
Layout:
- Dashboard gallery with tags (SLO, APM, Infra, DB, K8s)
- Each dashboard shows time-series panels, log panels, SLO panels.
- “Pin panel to incident” action to save evidence.

SCREEN 8: RUNBOOKS & KNOWLEDGE
Purpose: searchable runbooks/postmortems/known errors.
Layout:
- Search with filters (service, tag, incident type)
- Results show snippet + “last used” + “success rate”
- Document viewer with sections, steps, checklists
- AI helper: “Generate first 15 minutes triage checklist” and “Summarize relevant steps for this incident”
- “Create Known Error” flow that links to recurring patterns.

SCREEN 9: AUTOMATION
Purpose: run operational actions safely.
Layout:
- Catalog of automations (restart service, scale deployment, clear cache, feature flag toggle, run diagnostics)
- Each automation has:
  - description, required approvals, scope, rollback steps
  - dry-run preview
  - audit log
- Integration with incident: “Run mitigation and attach result to timeline”.

SCREEN 10: SETTINGS / INTEGRATIONS
Purpose: connect data sources and configure RBAC.
Layout:
- Integrations cards: ServiceNow, Splunk Observability, Splunk, BigPanda, AppDynamics, MongoDB, Others.
- Each card shows connection status, last sync, token expiry, permissions.
- Config pages for:
  - field mappings (service, env, region)
  - correlation rules
  - alert suppression policies
  - AI policies (redaction, allowed data types, prompt templates)
  - RBAC: teams, service ownership mapping

COMPONENTS TO INCLUDE ACROSS SCREENS
- Global search that returns services, incidents, clusters, dashboards, runbooks
- Severity badges and ownership chips everywhere
- Evidence “chips” that deep-link to sources
- “Create Evidence Bundle” action that saves key panels/queries for an incident
- Toast notifications and audit trail for any action

DELIVERABLES
Generate high-fidelity UI designs for all screens above (desktop), plus at least:
- one mobile responsive view for Overview and Incident Workspace
- dark mode versions for Overview and Incident Workspace
- a mini design system page: buttons, badges, tables, cards, timeline, charts, Copilot panel, modals, empty/loading/error states
