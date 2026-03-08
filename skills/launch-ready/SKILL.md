---
name: launch-ready
description: Audit and improve a repository for public launch — README, metadata, contributing guide, issue templates, code quality, and professional polish. Use before promoting a repo on LinkedIn, Twitter, or submitting to community programs.
---

# Launch Readiness Audit

## Overview

Comprehensive audit and improvement pass to ensure a repository looks like a serious, well-maintained community project before public promotion. Covers repository metadata, README quality, contributor onboarding, issue templates, code quality signals, and professional polish.

## When to Use
- Before promoting a repo publicly on LinkedIn, Twitter, or Hacker News
- Before submitting a project as part of a community ambassador or developer advocate application
- Before requesting stars, contributors, or community engagement
- When transitioning a personal project into a community-facing open source project

## Audit & Improvement Procedure

Work through all 7 categories below. For categories that involve making changes, make them directly. For categories that require manual GitHub actions, provide a clear checklist. Prioritize by visibility — highest-impact improvements first.

Use parallel Agent subagents for independent categories where possible.

---

### 1. Repository Metadata (Advise Only — Manual GitHub Actions)

**Checks:**

a) **Topics/tags:** Suggest 5-8 GitHub topics that maximize discoverability. Consider:
- Primary technology (e.g., `claude-code`, `ai-agents`)
- Problem domain (e.g., `developer-tools`, `software-engineering`)
- Methodology (e.g., `test-driven-development`, `design-systems`)
- Community hooks (e.g., `anthropic`, `llm-tools`, `cli`)

b) **Repository description:** Review the one-liner shown on GitHub. Is it compelling, accurate, and under 120 characters? If not, suggest a better one.

c) **GitHub Releases:** Check if tags exist without release notes:
```bash
git tag -l
gh release list 2>/dev/null
```
If tags exist but releases lack notes, draft release notes based on commit history and any RELEASE-NOTES.md or CHANGELOG.

**Output:** A checklist of exact values to enter in GitHub Settings.

---

### 2. README Improvements

**Checks and actions:**

a) **Number verification:** Count actual items and compare to README claims:
```bash
# Count actual skills
ls -d skills/*/SKILL.md 2>/dev/null | wc -l

# Count actual phases
ls skills/software-forge/phases/*.md 2>/dev/null | wc -l

# Count actual commands
ls commands/*.md 2>/dev/null | wc -l

# Count concept/book reference files
ls skills/engineering-mentor/concepts/*.md 2>/dev/null | wc -l
```
Flag any discrepancy. Fix the README if counts are wrong.

b) **Skill table accuracy:** Cross-reference every skill listed in the README table against actual `skills/*/SKILL.md` files. Flag anything listed but missing, or present but unlisted.

c) **Phase table accuracy:** Verify every phase listed maps to a real file in `skills/software-forge/phases/`.

d) **Visual element:** Add a high-level flow diagram near the top of the README (after the project description, before "How it works"). Use either ASCII art or a Mermaid diagram showing the core flow:
- User describes project → Classification → Phase selection → Build/learn → Output

e) **Install instructions:** Review all listed install methods. Test the install.sh script:
```bash
bash -n install.sh  # Syntax check
```
Verify the instructions are correct and complete.

f) **Demo suggestion:** If there's no demo GIF or screenshot, suggest exactly what terminal session to record — which commands to run, what project to describe, what output to show — to create the most compelling 30-second demo.

g) **Information hierarchy:** Review the README structure from the perspective of someone landing from LinkedIn with 30 seconds to decide if it's worth starring. The first screen should answer: What is this? Why should I care? How do I try it?

---

### 3. Create CONTRIBUTING.md

Create a `CONTRIBUTING.md` that is concise (under 80 lines) and includes:

- Warm welcome message
- How to add a new skill (file structure, naming, SKILL.md format)
- How to propose a new phase
- How to suggest a new book grounding
- Specific areas where help is wanted (check repo for actual gaps, TODOs, or missing skills)
- Code of conduct reference (Contributor Covenant v2.1)

---

### 4. Create Issue Templates

Create `.github/ISSUE_TEMPLATE/` with three templates:

**skill-request.md:**
```yaml
---
name: Request a New Skill
about: Suggest a new skill for the library
labels: enhancement, skill-request
---
```
Fields: skill name, problem it solves, which phase it belongs to, suggested book grounding (optional)

**phase-suggestion.md:**
```yaml
---
name: Suggest a Phase Improvement
about: Improve an existing phase or suggest a new one
labels: enhancement, phase
---
```
Fields: which phase, what's wrong or missing, suggested book or source

**bug-report.md:**
```yaml
---
name: Bug Report
about: Report an issue with a skill or the install process
labels: bug
---
```
Fields: which skill, what happened, expected behavior, Claude Code version, install method used

---

### 5. Code Quality Signals

**Checks and actions:**

a) **EditorConfig:** Check for `.editorconfig`. If missing, create one with sensible defaults:
```ini
root = true

[*]
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
charset = utf-8

[*.md]
indent_style = space
indent_size = 2
trim_trailing_whitespace = false

[*.{sh,bash}]
indent_style = space
indent_size = 2

[Makefile]
indent_style = tab
```

b) **Broken links:** Scan all markdown files for internal references (`](./`, `](../`, `](skills/`) and verify the targets exist. Check external URLs if feasible.

c) **TODO/FIXME/HACK/XXX cleanup:** Search all files for these markers:
```
TODO
FIXME
HACK
XXX
WORKAROUND
```
For each: either resolve it, remove it, or convert it to a GitHub issue before going public.

d) **Formatting consistency:** Check all skill SKILL.md files for consistent structure:
- Frontmatter format (name, description)
- Heading levels
- Spacing between sections
- Consistent use of code blocks

e) **Empty/stub files:** Search for files under 10 lines or with placeholder content. Flag anything that looks unfinished.

---

### 6. Professional Polish

**Checks:**

a) **Typos and grammar:** Read through all markdown files and flag typos, grammatical errors, or inconsistent capitalization.

b) **LICENSE file:** Verify it exists, is correctly formatted MIT, and includes the correct year and copyright holder.

c) **Gitignore coverage:** Check `.gitignore` for common exclusions:
```
.env
.env.*
.DS_Store
node_modules/
*.log
.idea/
.vscode/
*.swp
```
Verify none of these are committed in the repo.

d) **Large files:** Check for binary files or unnecessarily large files:
```bash
git ls-files | xargs ls -la 2>/dev/null | sort -k5 -rn | head -20
```

e) **Commit history review:** Scan recent commit messages for unprofessional language, personal info, or internal project references:
```bash
git log --all --oneline | head -50
```

---

### 7. Competitive Positioning

**Actions:**

a) Search GitHub for similar projects to understand the landscape:
- Claude Code skills/extensions
- AI agent frameworks
- Software development lifecycle tools
- Engineering mentor/teaching tools

b) Based on what exists, suggest 1-2 differentiating sentences for the README that position the project without being disparaging. Focus on what makes it unique (e.g., book-grounded engineering, adaptive mentoring, full lifecycle coverage).

---

## Report Format

For each category, report:

### Changes Made
List every file created or modified, with a brief explanation of what and why.

### Manual Actions Required
Numbered checklist with exact values to enter in GitHub Settings or other platforms.

### Findings (No Action Needed)
Items that were checked and found to be fine.

## Summary

End with:

```
## Launch Readiness Summary

| Category              | Status | Changes Made | Manual Actions |
|-----------------------|--------|-------------|----------------|
| Repository Metadata   | ...    | X           | X              |
| README                | ...    | X           | X              |
| CONTRIBUTING.md       | ...    | X           | X              |
| Issue Templates       | ...    | X           | X              |
| Code Quality          | ...    | X           | X              |
| Professional Polish   | ...    | X           | X              |
| Competitive Position  | ...    | X           | X              |

**Verdict:** READY FOR LAUNCH / NEEDS WORK (with priority list)
```
