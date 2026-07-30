# WCS Continuity Package — Plan & Blueprint

**Owner:** Fal Mensah · Technical Business Integration · Digital Core / WCS
**Clock:** Fixed duration ends August 15. Today is July 30. Working days available: ~11.
**Positioning:** *Proof to Possibility* is the brand. The Continuity Command Center is the centerpiece proof.

---

## 1. What this is

A SharePoint-based **Continuity Command Center** that does three jobs at once:

1. **Operational:** ensures the CI operating model (DRIPS, front door, risk register,
   runbooks) survives any staffing change.
2. **Strategic:** makes the continuity risk of an unfilled conversion visible as *data*,
   not as a complaint.
3. **Portfolio:** every page is a live artifact of Fal's architecture, automation,
   and governance capability.

## 2. Honest phase labels (use these exact words)

| Label | Meaning | Never |
|---|---|---|
| **LIVE** | Running now, can be demoed clicking it | Label anything LIVE that needs a manual step behind the curtain |
| **DESIGNED** | Architecture complete, pending access/connector | Present DESIGNED work as running |
| **ROADMAP** | Phase 2+, needs decision or budget | Promise dates for ROADMAP items |

## 3. Hub structure (SharePoint Communication Site)

**Site name:** WCS Continuity & CI Command Center

| Page | Contents | Status target by Aug 11 |
|---|---|---|
| **Home** | Hero: mission + 3 KPIs (open risks, mitigations in motion, runbooks published). Embedded Power BI chart. Continuity-risk banner. | LIVE |
| **Identify** | Risk register (SharePoint List): Risk ID, PI-planning duty, impact, owner, Jira link, status. Includes the Continuity Risk row. | LIVE |
| **Protect** | Runbook library — MRTS runbook, DRIPS operating guide, front-door SOP. Each doc: owner, last-verified date. | LIVE |
| **Respond** | Escalation paths + the one real Power Automate flow (new high-priority risk → Teams channel alert). | LIVE (the flow is the demo moment) |
| **Recover** | Handoff plan: what each artifact is, who inherits it, time-to-competency estimate per system. | LIVE |
| **Roadmap** | Agentic phase 2: triage agent (Copilot Studio), Jira auto-sync (Power Automate webhook), Cortex governance layer, AB-900-informed guardrails. | DESIGNED / ROADMAP |

## 4. The one real automation (build this, demo this)

**Flow:** When an item is created or modified in the Risk Register with
Priority = High → post an adaptive card to the WCS Teams channel with risk title,
owner, and link.

- Needs: standard Power Automate + SharePoint + Teams connectors (no Jira, no admin).
- Build time: ~1 hour including testing.
- Demo script: create a risk live in the meeting; the Teams ping lands while
  they watch. That ten seconds beats any architecture diagram.

## 5. The DRIPS-on-itself slide (adoption story, told straight)

- **Detect:** Post-PI launch, link-back rate to the register: 0%. Signal, not failure.
- **Root cause:** Manual cross-tool steps (copy Jira link → paste into register) add
  friction at exactly the moment teams are busiest.
- **Improve:** Remove the human step — automation syncs state; people just work in Jira.
- **Prove:** Phase 1 flow live (Teams alert); Phase 2 designed (Jira webhook sync).
- **Share:** This hub. The lesson generalizes to every cross-tool workflow in WCS.

## 6. The continuity risk — as a register row, not an ultimatum

| Field | Value |
|---|---|
| Risk ID | BCP-001 |
| Description | CI operating model, front-door routing, and hub architecture have a single named owner on fixed-duration assignment ending 2026-08-15 |
| Impact | High — orphaned register, unowned runbooks, stalled automation roadmap |
| Mitigation options | (a) Convert role to FTE; (b) extend + name successor with 4-week shadow; (c) accept orphaning, archive hub read-only |
| Status | **Open — decision required by 2026-08-08** |

The decision date sits one week before the end date on purpose: HR paperwork needs lead time,
and the row makes the deadline theirs, not yours.

## 7. Manager conversation (non-adversarial script)

> "I've packaged the CI operating model into a continuity hub so nothing depends on
> what's in my head. Walking the register, there's one risk I can't mitigate myself —
> BCP-001. I'd rather surface it as data than as a hallway conversation.
> What would need to be true for this to get to HR before the 8th?"

Then stop talking. The silence is the ask.

If the answer is headcount again: "Understood. Then option (b) — who should I start
shadowing on the handoff plan?" — either he names someone (real handoff, professional
exit, reference secured) or he feels the cost of naming no one.

## 8. Two-week execution plan

| Dates | Focus | Output |
|---|---|---|
| Jul 30–31 | Site shell + navigation + risk register list (import existing register) | Hub skeleton LIVE |
| Aug 1 | Runbook library + handoff page | Protect/Recover LIVE |
| Aug 4 | Build + test the Teams alert flow | The demo moment |
| Aug 5 | Power BI: risks by status/owner chart, embed on Home | Leadership candy |
| Aug 6 | Roadmap page (agentic phase 2, honest labels) + polish pass | Hub complete |
| Aug 7 | Dry-run demo with a friendly colleague; fix rough edges | Confidence |
| Aug 8 | **Demo to manager. Ask the question.** | The decision |
| Aug 11–14 | Either: conversion paperwork support · or: execute handoff plan + finalize external portfolio | Both outcomes covered |

## 9. What stays from Proof to Possibility

- **Brand:** the hub's About panel: "Part of Proof to Possibility — an AI-native body
  of work by Fal Mensah." One line. Consistency compounds.
- **External edition:** the interactive showcase, comic, deck, and hero canvas are the
  sanitized portfolio if Aug 15 stands — LinkedIn, interviews, LegacyOps. Keep building
  the video track after the 8th, not before; the hub owns the next 9 days.
- **AB-900:** listed as *in progress* on the Roadmap page only. Never "certified" until passed.

## 10. Rules for the next two weeks

1. Nothing fake in a demo. One real flow beats ten simulated ones.
2. Internal data stays in M365. External tools get sanitized content only.
3. Every artifact gets an owner and a last-verified date — that's what "continuity" means.
4. The hub is finished when someone else could run the model without you. That sentence
   is also the portfolio's thesis.
