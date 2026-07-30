# Build-Day Runbook — Continuity Command Center

**Clock:** Build today · Polish Thursday · Mentors (Ian + James) EOD Friday.
**Cut:** Power BI. Charts come from SharePoint's built-in Quick Chart web part.
**Rule:** No decisions during the build — everything to type or paste is in this file.

---

## TODAY — the build (5 focused hours)

### Hour 1 — Site shell
1. SharePoint home → **Create site → Communication site**.
2. Name: `WCS Continuity & CI Command Center`. Description: paste →
   > Ensuring the WCS continuous-improvement operating model — DRIPS, front-door intake, risk register, and runbooks — survives any staffing change. Part of Proof to Possibility.
3. Settings → Change the look → Theme: **Teal** (closest native match to the brand).
4. Top navigation → add five links (pages come in Hour 3): `Identify · Protect · Respond · Recover · Roadmap`.

### Hour 2 — Risk register list (import, don't type)
1. Site contents → **New → List → From CSV** → upload `risk-register-starter.csv` (companion file).
2. Confirm column types when prompted:
   - RiskID = single line · Description = multi-line · PIPlanningDuty = single line
   - Impact = **Choice** (High / Medium / Low) · Status = **Choice** (Open / In Progress / Mitigated / Decision Required)
   - Owner = single line (switch to Person column later if time allows) · JiraLink = Hyperlink · LastVerified = Date
3. Replace the three `[YOUR RISK]` placeholder rows with real items from your existing register. **Do not edit row BCP-001.**
4. Create two views: **All Risks** (default, group by Status) and **Leadership View** (filter: Impact = High).

### Hour 3 — Pages (paste-ready copy below)
Create 5 pages (New → Page → blank). For each: title, one text web part, paste, publish.

**Identify** —
> **Know the risk before it knows you.** This register reconciles WCS risks against post-PI-planning duties. Every row has an owner, an impact rating, and a live status. High-impact items surface automatically in the Leadership View.
Then: add **List web part** → select the risk register → Leadership View.

**Protect** —
> **If it isn't written down, it isn't protected.** Runbooks and operating guides for the systems this hub covers. Every document lists an owner and a last-verified date — continuity means someone else can run it.
Then: add **Document library web part**. Upload today if you have them handy: MRTS runbook, DRIPS operating guide, front-door SOP. (Uploading can slip to Thursday.)

**Respond** —
> **When something breaks, nobody should be searching for a phone number.** Escalation paths and automated alerts. High-priority risks entered in the register notify the WCS Teams channel automatically — no manual forwarding.
(The flow that makes this true is Thursday's first task.)

**Recover** —
> **The handoff plan.** What each artifact is, who inherits it, and the estimated time-to-competency. A system is only continuous if it survives its author.
Then: add a table web part →

| Artifact | What it is | Inheritor | Time to competency |
|---|---|---|---|
| DRIPS operating model | 5-stage CI loop across 21 product teams | TBD — decision required | 2–3 sprints shadowing |
| Front-door intake | Categorization, routing, ownership model | TBD — decision required | 1–2 sprints |
| Risk register + this hub | Architecture, views, automation | TBD — decision required | 2 weeks |
| MRTS runbook | Meeting-room audit procedure (244 delivered vs 100 target) | Documented — self-serve | Days |

**Roadmap** —
> **Phase 2 — the agentic layer (DESIGNED, pending access).**
> • Jira auto-sync: Power Automate webhook updates the register when issues change — removes the manual link-back step that Phase 1 adoption data flagged.
> • Triage agent (Copilot Studio): intercepts risk mentions in Teams and drafts register entries.
> • Governance guardrails informed by AB-900 (Microsoft 365 Copilot & agent administration — certification in progress).
> Status labels used across this hub: **LIVE** = demoable now · **DESIGNED** = architecture ready, pending connector access · **ROADMAP** = pending decision.

### Hour 4 — Home page
Layout top to bottom:
1. **Hero web part** (single image or color block): headline →
   > Continuity is a deliverable.
   Sub-headline →
   > The WCS CI operating model, packaged to survive any staffing change.
2. **Quick Chart web part** → Data from: risk register → Bar chart → by Status. Title: `Open risks vs. mitigation progress`. *(This replaces Power BI. Two minutes.)*
3. **Text web part** — the DRIPS-on-itself story:
   > **What Phase 1 taught us.** After PI planning, the link-back rate from teams to the register was 0%. Not a people problem — an architecture signal: manual cross-tool steps add friction exactly when teams are busiest. Phase 2 removes the human step entirely. Detect → Root Cause → Improve → Prove → Share — applied to our own system.
4. **List web part** → risk register → Leadership View.
5. **About panel** (text, bottom):
   > Architected and maintained by **Fal Mensah**, Technical Business Integration, Digital Core / WCS. Part of *Proof to Possibility* — an AI-native body of work. Built with the M365 + Claude Enterprise stack it documents. vNext decisions tracked in BCP-001.

### Hour 5 — Buffer + review pass
Walk every page as a stranger. Fix titles, broken web parts, nav order. Set site sharing so Ian and James can view.

---

## THURSDAY — polish (4–5 hours, in this exact order)

### 1. The guaranteed flow first (60 min — this is the live demo, protect it)
Power Automate → Automated cloud flow → trigger **"When an item is created or modified
(SharePoint)"** → site + register → Condition: `Impact = High AND Status = Open` →
action **"Post card in a chat or channel (Teams)"** → WCS channel → card: Risk title,
Owner, link to item. Test with a dummy High risk; delete it after the ping lands.
**Do not start task 2 until this works.** Standard connectors only — nothing can block it.

### 2. Power BI dashboard (90 min, since you have access)
1. Power BI Desktop → **Get Data → SharePoint Online List** → paste the site URL
   (site root, not the list URL) → select the risk register → Load.
2. Build exactly three visuals, no more:
   - **Donut:** risks by Status (Open / In Progress / Mitigated / Decision Required)
   - **Bar:** risks by Impact, colored by Status
   - **Card:** count of Status = "Decision Required" — titled **"Decisions needed"**
     (this is BCP-001 staring at leadership from the dashboard)
3. Theme the visuals: background `#0B1116`, text `#F2EDE4`, accents `#35C4BD` / `#E8A25C`
   — the dashboard matches the Proof to Possibility world.
4. Publish → your workspace → SharePoint Home page → replace the Quick Chart web part
   with the **Power BI web part** → paste report link.
**Safety rule:** build the Quick Chart today regardless. It stays until the Power BI
embed renders for a colleague who isn't you (license/permission check). Never delete
the fallback before the upgrade is verified.

### 3. Jira → register sync: ONE timeboxed attempt (45 min hard stop)
The goal from the Google notes: Jira issue updated → register row updates automatically.
- Attempt: Power Automate → new flow → search connector **"Jira"** → trigger
  **"When an issue is updated"** (needs your Jira Cloud site URL + an API token from
  your Atlassian account settings) → action **"Update item (SharePoint)"** mapped by RiskID.
- **If you hit any of these, stop immediately and ship it as DESIGNED:** the Jira
  connector is marked Premium and blocked by policy · admin consent screen appears ·
  token auth loops. That wall is a tenant-governance decision, not a skills gap —
  45 minutes of fighting it buys you nothing by Friday.
- Either way, the Roadmap page already documents the architecture. If it works: move it
  to LIVE and you have TWO live automations. If not: it's your Phase 2 headline, and the
  ask for connector enablement becomes part of the conversion conversation — "the
  design is done; unblocking it needs someone with tenure past August 15."

### 4. Content pass (60 min)
Upload remaining runbooks; add LastVerified dates; convert Owner column to Person type
if time allows.

### 5. Dry run (30 min)
Walk the 10-minute script below once, out loud, clicking everything — including one
live dummy-risk → Teams ping.

## FRIDAY — into mentors' hands

Teams message to Ian + James:
> Ian, James — the pivot we talked about is live. I turned the CI operating model into a **Continuity Command Center**: risk register reconciled against PI-planning duties, runbook library, automated Teams alerts on high-priority risks, and a handoff plan. One row to look at first: **BCP-001**. Link: [site]. 15 minutes for your read before I take it to [manager]? I want the framing airtight.

**10-minute walkthrough order:** Home (chart + Phase 1 story) → Identify (Leadership View) → live demo: create a High risk, Teams ping lands → Recover (handoff table, TBD rows do the talking) → Roadmap (honest labels) → BCP-001 → the question:
> "What would need to be true for this to get to HR before the 8th?"

---

## Scope discipline (what is IN and what is CUT this week)
- **IN:** Power BI dashboard (Thursday, 90 min, Quick Chart kept as fallback until embed verified).
- **IN (timeboxed):** Jira → Power Automate sync — one 45-minute attempt Thursday, hard stop, ships as DESIGNED if blocked.
- CUT: Copilot Studio agent — Roadmap page only. Do not attempt a build before Friday.
- CUT: Video/Synthesia/comic art — resumes after Aug 8.
- CUT: fancy formatting, custom SPFx, anything requiring admin tickets — no.
