---
name: repo-forensic-analysis-skill
description: "Perform a zero-assumption forensic analysis of a repo's recent history, conversation history, documentation drift, and current state, then verify anomalies against primary sources."
---

# Repo Forensic Analysis Skill

Use this skill when the user wants a systematic historical verification pass over a repository and expects evidence, not assumptions.

The analysis target is:

`$ARGUMENTS`

## Core Rules

- Trust nothing until verified from primary sources.
- Start from current repo reality, not prior summaries.
- Read raw history sequentially when reconstructing prior work.
- Cite exact commits, files, commands, and conversation sources.
- If the user wants execution mode, fix verified critical anomalies instead of stopping at reporting.

## Required Workflow

### 1. Build The Evidence Ledger First

Do not analyze commits or conversations until you inventory the sources.

Collect at minimum:

- git history for the last 4 weeks
- commit counts, authors, merge commits, revert commits
- highest-churn files
- Codex and Claude conversation files tied to the repo
- repo-local conversation exports and agent folders
- core docs such as `AGENTS.md`, `DESIGN.md`, `CLAUDE.md`, and `README.md`
- markdown files created in the last 4 weeks
- diff stats across the recent history window

Before moving on, report exact counts for:

- total commits analyzed
- total conversation files found
- total documentation files found
- files with highest churn

### 2. Analyze Every Commit Chronologically

For each commit in the target window, extract:

- SHA
- author
- date
- exact message
- parent commits
- files changed
- lines added and removed
- tests added or modified

Then classify the change:

- new feature
- bug fix
- refactor
- UI/style change
- config change
- docs only
- revert/correction
- merge

Also flag suspicious patterns:

- vague or typo-heavy commit messages
- churn on the same files across nearby commits
- large remove/add rewrites
- logic changes without tests
- unexplained config changes
- self-reverts or near-term corrections

Cross-reference each commit against:

- issue or PR references
- follow-up commits on the same files
- partial reverts
- contradictions with documentation claims

### 3. Analyze Conversation History For Real Outcomes

For each conversation file found:

- record source, path, date range, and message count
- read sequentially
- extract the user's normalized intent, constraints, files mentioned, and success criteria
- compare the agent's promises against what was actually done in the repo
- tag each turn as `OK`, `PARTIAL`, `FAIL`, `CORRECTED`, or `IGNORED`

Specifically detect:

- repeated user requests for the same issue
- "again", "still broken", "not working", or similar escalation signals
- cases where the agent claimed completion before the repo matched that claim

### 4. Build The Historical Change Map

Create a feature evolution map covering at least:

- UI components
- business logic
- data models
- API routes and handlers
- configuration
- styling
- analytics
- user-facing copy

For each important feature, document:

- first appearance
- last modification
- major transitions with commits
- current state
- whether the current state matches the apparent user intent

Flag anomalies such as:

- features added then removed quickly
- conflicting rewrites
- docs claiming completion while code says otherwise
- multiple agents or styles colliding on the same surface

### 5. Verify The Current Repo State

Do not stop at history. Check the live repo now.

Run the fastest real proof that fits the stack:

- typecheck
- lint
- tests
- targeted runtime checks
- direct code inspection

For each historically claimed bug fix, verify whether it is actually fixed in the current repo.

### 6. Produce The Anomaly Catalog

Group verified findings into:

- critical anomalies
- medium anomalies
- minor anomalies
- documentation drift
- agent behavior issues

Every anomaly entry should include:

- description
- concrete evidence
- affected files or commits
- recommended fix

### 7. Execute Recovery Work When Requested

If the user clearly wants fixes, do not stop at the report.

Fix verified critical anomalies first, then verify each batch with the relevant checks. Keep the repo stable after each batch.

### 8. Final Output Format

Return the work as a structured report with these sections:

- `# FORENSIC ANALYSIS REPORT`
- `## Evidence Ledger`
- `## Commit Analysis Summary`
- `## Conversation Analysis Summary`
- `## Historical Change Map`
- `## Consistency Verification Results`
- `## Current Repo State`
- `## Anomaly Catalog`
- `## Recovery Actions Taken`
- `## Final Assessment`
- `## Remaining Work`

Do not say the analysis is complete unless:

- the evidence ledger has exact counts
- all commits in scope were analyzed
- all conversation sources found were read
- current repo checks were run or explicitly blocked
- the anomaly catalog is populated with evidence
- any requested execution work is either finished or explicitly blocked
