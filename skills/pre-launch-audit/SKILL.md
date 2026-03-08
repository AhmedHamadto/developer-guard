---
name: pre-launch-audit
description: Run a complete pre-launch audit by orchestrating repo-scan, legal-audit, first-run-audit, and launch-ready in sequence. Produces a single consolidated GO/NO-GO launch readiness report with prioritized action plan.
license: MIT
---

Run a complete pre-launch audit for this repository by orchestrating all audit skills in sequence. Produce a single consolidated launch readiness report at the end.

## When to Use This Skill

- Before promoting a repo publicly (LinkedIn, Twitter, Product Hunt, community programs)
- Before submitting to ambassador programs or external reviews
- Before onboarding external contributors to an open source project
- As a final gate before any public-facing launch

## Execution Sequence

Run the following audits in order. Complete each fully before moving to the next. Carry context forward — if an earlier audit finds an issue, later audits should account for it.

### Step 1: Repository Scan

Invoke the `/repo-scan` skill. Scan the entire repository for confidential data, secrets, personal information, hardcoded paths, and attribution gaps. All 8 scan categories.

**Gate logic:**
- If CRITICAL findings are found: **STOP.** Do not proceed to Step 2. Report the critical findings immediately — these must be fixed before any other audit is meaningful.
- If no CRITICAL findings: continue, carrying any HIGH/MEDIUM/LOW findings into the final report.

### Step 2: Legal & PR Audit

Invoke the `/legal-audit` skill. Audit for legal, IP, attribution, accuracy, PR, and trademark risks. All 6 categories.

**Gate logic:**
- If CRITICAL findings are found: **STOP.** Report alongside any Step 1 findings.
- If no CRITICAL findings: continue.

### Step 3: First-Run Audit

Invoke the `/first-run-audit` skill. Simulate first-time user experience. Test install paths, first interaction, feature discoverability, workflow paths, stateful features, resumption, error handling, and documentation completeness. All 8 categories.

**Gate logic:**
- If BLOCKING findings are found: flag them but **continue** to Step 4 (usability issues don't prevent the launch-ready check from running).

### Step 4: Launch Readiness

Invoke the `/launch-ready` skill. Audit and improve the repo for public launch. README improvements, metadata suggestions, contributing guide, issue templates, code quality signals, professional polish. All categories.

**Gate logic:** No gate — this is the final step. Collect all findings.

---

## Consolidated Report

After all four steps complete, produce a single launch readiness report with the following structure.

### Launch Verdict: GO / NO-GO / CONDITIONAL

**GO** = No critical or blocking findings across any audit. Ready to promote publicly.

**CONDITIONAL** = No critical findings, but blocking or high-severity issues exist that should be fixed first. List the specific conditions that must be met.

**NO-GO** = Critical findings exist. Do not promote until resolved.

### Findings Summary Table

List ALL findings from all four audits in a single table, sorted by severity (CRITICAL first, then HIGH, MEDIUM, LOW), then by fix effort (quick wins first).

```
| # | Finding | Source Audit | Severity | Category | Fix Effort |
|---|---------|-------------|----------|----------|------------|
```

### Priority Action Plan

Group findings into three tiers:

**Fix Before Launch** (do these now):
- All CRITICAL and BLOCKING findings
- Any HIGH finding that affects first impressions (README, install, first interaction)

**Fix Within First Week** (do these soon after launch):
- Remaining HIGH findings
- MEDIUM findings that affect usability

**Fix When Convenient** (backlog):
- LOW findings
- Polish items
- Nice-to-have improvements

### Strengths

List 3-5 things the repo does well that should be highlighted in public promotion. These inform what to emphasize when sharing publicly.

### Launch Checklist

Generate a numbered checklist of every action item from the Priority Action Plan, formatted as a TODO list the developer can work through sequentially. Each item should be actionable in a single sitting (break large items into sub-tasks if needed).

```
- [ ] Fix: <specific actionable item with file/line if applicable>
- [ ] Fix: <next item>
- [ ] Add: <something missing>
- [ ] ...
```

---

## Important Notes

- Do NOT skip audits or combine them. Each audit skill has its own methodology. Run them fully and sequentially.
- If the repo-scan or legal-audit gates trigger a STOP, the report should clearly state which audits were completed and which were skipped due to the gate, so the developer knows to re-run after fixing critical issues.
- Findings from earlier audits inform later ones. If repo-scan finds a hardcoded path, first-run-audit should check whether that path would break on another machine.
- The final report replaces individual audit reports — the developer should only need to read one document.
