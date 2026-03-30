# Git Archaeologist

**A git history forensics skill for Claude Code. Excavates secrets, permanent TODOs, fragile subsystems, and dead code from your full commit history.**

Free skill for Claude Code, Claude Desktop, and any OpenClaw-compatible agent.

---

## What This Does

Installs a systematic git history investigation capability on your agent. The Git Archaeologist reads the full history — not just HEAD — and surfaces what actually happened in a codebase versus what the README says happened.

The Archaeologist runs five procedures and produces one report:

1. **Secrets Archaeology** — Scans every commit (including amended and reverted ones) for credentials, tokens, and keys. Secrets deleted in commit B are still present in commit A. The Archaeologist finds them.
2. **Temporary Code Survey** — Finds TODOs, FIXMEs, HACKs, and WORKAROUNDs in current HEAD, then determines when each was introduced. A TODO from last week is noise. A TODO from two years ago is a finding.
3. **Commit Message Pattern Analysis** — Extracts vocabulary from all commit messages to identify recurring fix patterns, "temporary" commits that shipped, revert chains, and after-hours fire-fighting clusters.
4. **Dead File Survey** — Identifies files that have never been modified since creation. These are either stable infrastructure or forgotten code. The Archaeologist tells you which.
5. **Hall of Shame** — The five most instructive findings from the full excavation: the most sensitive history finding, the oldest surviving TODO, the most reverted file, the longest after-hours streak, and the commit message that most clearly contradicts its actual content.

---

## Install

Add `SKILL.md` to your project's `.claude/` directory, or paste the contents into your Claude system prompt.

```bash
curl -O https://raw.githubusercontent.com/mattmayberry/git-archaeologist/main/SKILL.md
```

---

## Usage

Once installed, run an excavation with a single prompt:

```
"Run a git archaeology report on this repo."

→ Archaeologist scans full history across all branches
→ Identifies secrets that were committed and deleted
→ Ages every TODO/FIXME/HACK still in HEAD
→ Extracts commit message patterns and fragility signals
→ Produces Archaeology Report with Hall of Shame
```

Requires local git access with full history (not a shallow clone).

---

## Sample Output

```
GIT ARCHAEOLOGY REPORT
Repository: my-saas-app
Commits scanned: 847
Date range: 2022-08-14 to 2025-11-14
Report date: 2025-11-14

FINDINGS SUMMARY
CRITICAL: 0  HIGH: 2  MEDIUM: 3  LOW: 7  INFO: 12

SECRETS ARCHAEOLOGY
[HIGH] API key pattern — introduced 2023-01-09 — removed in commit a4f2b91
[HIGH] Bearer token — introduced 2022-11-30 — removed in commit 7c83dd2
Note: Both are in history and recoverable by anyone with repo access.

TEMPORARY CODE (sorted by age)
[HIGH]  src/auth/middleware.ts:44 — "// HACK: remove before launch" — introduced 2022-09-01 — 2y 3m
[HIGH]  src/billing/stripe.ts:118 — "// TODO: handle webhooks properly" — introduced 2023-02-15 — 1y 9m
[MEDIUM] src/api/rate-limit.ts:7 — "// FIXME: this breaks under load" — introduced 2024-03-20 — 7m

COMMIT PATTERN FINDINGS
- Recurring fix commits: 34 instances, clustered in src/billing/ — indicates systemic fragility
- Temporary commits that shipped: 8 instances of "quick fix" / "just for now" still at HEAD
- Most fragile surface: src/billing/stripe.ts (12 fix/hotfix commits)

HALL OF SHAME
Most sensitive history finding: Bearer token in commit 7c83dd2 (2022-11-30) — pattern suggests production credential
Oldest surviving TODO: src/auth/middleware.ts:44 — 2y 3m — "// HACK: remove before launch"
Most reverted file: src/billing/stripe.ts — 4 reverts
Longest after-hours streak: 6 consecutive commits, 2024-07-18 to 2024-07-19
Best accidental self-documentation: a1b2c3d — "fix: fix the fix from yesterday's fix"

ACTION ITEMS
| Finding | Severity | Action | Owner Role |
|---------|----------|--------|------------|
| API key in history | HIGH | Rotate key immediately, verify not in use | Security |
| Bearer token in history | HIGH | Rotate token, audit for unauthorized use | Security |
| 2y-old HACK in auth middleware | HIGH | Review and remove or formalize | Backend |
```

---

## Limitations

- Requires full git history (not a shallow clone)
- Pattern matching is heuristic: will produce false positives on example/placeholder values
- Binary files are excluded from scanning
- Commit message analysis indicates tendencies, not certainties

---

## Want More?

This free skill is a sample of the full **[Git Archaeologist: Full Program Edition](https://www.shopclawmart.com/listings/9d9f2537-c618-436b-a657-c32be157aecb)** ($49 on ClawMart) — adds a persistent FINDINGS.md log with structured triage workflow, a credential rotation checklist generator for every secret found in history, Greyline: Warden integration so findings route into your security program, and scheduled delta scanning that only surfaces what's new since your last run.

---

## License

MIT. Free to use, fork, and extend.

A [Meridian Lab](https://themeridianlab.com) product.
