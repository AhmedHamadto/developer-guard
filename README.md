# Developer Guard

Pre-commit scanning and pre-launch audits for Claude Code. Catches secrets, personal data, hardcoded paths, and .env files before they reach git history.

## Install

```
/plugin install developer-guard@AhmedHamadto/developer-guard
```

## What It Does

### Pre-Commit Gate (every commit)

Two-layer system that blocks sensitive content from reaching git history:

**Layer 1: Fast Hook** (automatic, ~200ms, zero tokens)
- Regex scan of staged files for API keys, tokens, private keys
- Blocks personal/company references, hardcoded user paths, .env files
- Runs automatically on every `git commit` via PreToolUse hook

**Layer 2: Smart Skill** (invoke `/pre-commit-review`)
- Contextual review of the staged diff using Claude
- Catches non-standard secrets, internal project references, sensitive business logic
- Recognizes false positives (example keys in docs, placeholder paths)

### Pre-Launch Audit (once, before publishing)

Four-step audit orchestrated by `/pre-launch-audit`:

1. **`/repo-scan`** — Secrets, PII, hardcoded paths, attribution gaps (8 categories)
2. **`/legal-audit`** — License compliance, IP risks, trademark concerns (6 categories)
3. **`/first-run-audit`** — First-time user experience simulation (8 categories)
4. **`/launch-ready`** — README, metadata, contributing guide, professional polish (7 categories)

Produces a single GO/NO-GO launch readiness report.

## Slash Commands

| Command | What It Does |
|---------|-------------|
| `/pre-commit-review` | Contextual review of staged changes |
| `/repo-scan` | Full repository scan for sensitive content |
| `/legal-audit` | Legal, IP, and attribution audit |
| `/pre-launch-audit` | All four audits in sequence with consolidated report |

## Companion

Pairs with [software-forge](https://github.com/AhmedHamadto/software-forge) for full software development lifecycle — design, implementation, security, and review phases.

## License

MIT
