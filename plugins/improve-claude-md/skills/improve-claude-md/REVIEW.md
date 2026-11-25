# esr review: plugins/improve-claude-md/skills/improve-claude-md

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: 742389d936458a62c303c5625232038788a9588f
- Date: 2026-11-25
- Choice: keep

## What it does (claude review, model text)

    The skill tells the agent how to rewrite a CLAUDE.md file that the user
    provides. Foundational context stays as plain text, and task-specific rules
    are wrapped in conditional <important if="..."> blocks so the agent follows
    them more reliably.

## What it could do (static profile)

- no network hosts, tool grants, scripts or binaries found

## Public scores (outside opinions)

- public scores (outside opinions): Gen safe, Socket safe (0 alerts), Snyk low

## Findings

- none

## Fix applied

- none
- Fix offered: none
- No fix needed. esr found no weakness to fix.

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
