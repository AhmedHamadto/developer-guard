---
name: first-run-audit
description: Simulate a first-time user experience for any repo — verify install paths, discoverability, documentation, and error handling. Use before publishing, promoting, or onboarding users to a project.
license: MIT
---

Simulate the experience of a first-time user who has never seen this repo before. Your job is to verify that someone can go from discovery to productive use without hitting confusion, broken paths, or undocumented assumptions.

## When to Use This Skill

- Before promoting a repo publicly (LinkedIn, Twitter, community programs)
- Before onboarding a new contributor or user
- After major refactors that change install paths, skill structure, or documentation
- When users report confusion or broken install steps
- As a periodic health check on any project with external users

## How to Run

1. Read the repo's README and any install/setup documentation first
2. Walk through each section below sequentially
3. Report findings using the severity format at the bottom
4. Produce a summary report at the end

---

## 1. Install Path Verification

Test every documented install method end-to-end:

**For each install method documented in the README:**
- Are the exact commands correct and copy-pasteable?
- Are there undocumented prerequisites (runtime versions, OS requirements, env vars)?
- After install, does the user have everything the README promises?
- Do any steps fail silently (exit 0 but didn't actually work)?

**If a plugin/marketplace install exists:**
- Does the install command work as documented?
- Are all advertised features actually discoverable after install?

**If a script install exists (install.sh, setup.py, etc.):**
- Does it run without errors on a clean machine?
- Does it create the expected files/symlinks/configs?
- Does selective install work if documented?
- Does uninstall cleanly remove everything?

**For each method:** flag missing prerequisites, unclear instructions, or steps that would fail silently.

---

## 2. First Interaction Quality

Simulate what happens when a new user runs the primary entry point with no prior context:

- Is the first prompt/output clear and self-explanatory?
- Can the user understand what to do next without reading external docs?
- If the tool offers modes/options, do they make sense without prior knowledge?
- Test with a simple input — does it produce a sensible result?
- Test with an ambiguous input — does it handle it gracefully or fail silently?
- Test with an input outside the expected scope — is there a helpful fallback or error?

---

## 3. Feature Discoverability

Cross-reference documentation against reality:

- For each feature/skill/command mentioned in the README: does it actually exist and work?
- For each feature/skill/command that exists in the repo: is it documented in the README?
- Flag any **orphans** (exist but not documented) or **phantoms** (documented but missing)
- Check that each feature has a consistent structure and can be understood in isolation
- If there's a meta-command or index that lists available features, verify it's complete and accurate

---

## 4. Workflow Path Verification

For each primary workflow or use case the project supports:

- Verify the documented steps actually produce the claimed result
- Check that every step either invokes a real feature or has well-defined inline behavior
- Flag any step that references something that doesn't exist
- Flag any step that has no clear entry/exit criteria
- Verify that workflows compose correctly (output of step N is valid input for step N+1)

---

## 5. Stateful Feature Verification

If the project maintains state across sessions (profiles, caches, configs, logs):

- Does the state mechanism work (is there a file or DB that persists)?
- Is the state format documented or at least inspectable?
- Does the project handle missing state gracefully (first run)?
- Does the project handle corrupted or partial state?
- Are there clear thresholds or triggers for state transitions, or are they vague?

---

## 6. Resumption and Recovery

Test what happens when things go wrong or get interrupted:

- If the project supports resuming interrupted work, does detection work correctly?
- If the project has no prior state, does it start cleanly without errors?
- If prior state is partial or corrupted, does it handle the edge case or crash?
- What happens if the user interrupts mid-workflow and restarts?
- Are error messages helpful and actionable, or generic and confusing?

---

## 7. Error Handling and Edge Cases

- What happens if the user skips the recommended entry point and invokes a sub-feature directly?
- Check for hardcoded assumptions about directory structure, file locations, or environment
- Test on edge cases: empty input, very long input, special characters, missing dependencies
- Verify that errors don't expose internal implementation details to the user
- Check that failures are recoverable without manual cleanup

---

## 8. Documentation Completeness

- Can a user understand what each feature does from its own docs alone, without reading the README?
- Is there a clear mapping from "I have this problem" to "I should use this feature"?
- Are external references (books, specs, standards) actually applied in the implementation, or just name-dropped?
- Is the README length appropriate? Too long = nobody reads it. Too short = missing critical info.
- Are there any assumptions the docs make about the reader's knowledge level?

---

## Reporting Format

For each finding, report:

```
[CATEGORY] [SEVERITY] description
  Location: file/path or step
  Experience: what the user would see/feel
  Fix: recommended action
```

**Categories:** INSTALL, FIRST-RUN, DISCOVERY, WORKFLOW, STATE, RESUMPTION, ERROR, DOCS

**Severities:**
- **BLOCKING** — prevents the user from completing setup or using the tool at all
- **CONFUSING** — user can work around it but wastes time or loses trust
- **MINOR** — polish item, doesn't impede use

---

## Summary Report

After completing all sections, produce:

1. **Pass/Fail verdict** for each section (1-8)
2. **Total findings** by severity (BLOCKING / CONFUSING / MINOR)
3. **Top 3 priorities** — the fixes that would most improve the first-run experience
4. **Overall assessment** — would you recommend this repo to a colleague in its current state?
