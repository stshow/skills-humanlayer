# esr review: plugins/show-me/skills/show-me

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: 362200ac2fb3c17b765049444e88de73767e6d08
- Date: 2026-11-25
- Choice: fix (re-checked and applied)

## What it does (claude review, model text)

    A user-invoked skill that explains the current conversation topic visually.
    It uses pseudocode, call trees, component and file trees, Mermaid diagrams
    and diffs, and for dense topics it writes one focused HTML file and opens it
    for the user.

## What it could do (static profile)

- no network hosts, tool grants, scripts or binaries found

## Public scores (outside opinions)

- public scores (outside opinions): Gen safe, Socket safe (0 alerts), Snyk low

## Findings

- low input_validation (claude review: model text, it can be wrong): The review reports an unquoted shell variable at line 121 of SKILL.md.
- low input_validation (claude review: model text, it can be wrong): The skill tells the agent to run a shell command that opens an HTML file whose name includes a free-text description placeholder. The command template does not quote the path or limit the description to safe characters. If the description comes from conversation content and contains spaces or shell metacharacters, the command could fail or run unintended shell syntax.

## Fix applied

- Fix label: hygiene
- Targets: low input\_validation
- Targets: low input\_validation
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- Fix id: sha256:875e3598f8500aa7beefa0690aa423658038bbe4663426aed57d88a2c0e21e8f
- Changed: SKILL.md
- Fix offered: hygiene, checked

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
