# esr review: plugins/visual-pr/skills/visual-pr

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: 2104d685d33ba9594a4dfe13241f0f7f02190401
- Date: 2026-11-25
- Choice: fix (re-checked and applied)

## What it does (claude review, model text)

    An explicitly invoked skill that creates or updates the GitHub pull request
    for the current branch. It writes a short, structured description (why the
    change exists, notes for reviewers, and a visual outline of the change),
    saves it under the local .humanlayer/tasks directory and publishes it with
    the gh CLI.

## What it could do (static profile)

- no network hosts, tool grants, scripts or binaries found

## Public scores (outside opinions)

- public scores (outside opinions): Gen safe, Socket safe (0 alerts), Snyk medium; BAD: Snyk medium

## Findings

- low other (claude review: model text, it can be wrong): When the current branch has no PR, the skill tells the agent to commit changes, push the branch with an upstream and open a PR on its own. It then overwrites the PR body with gh pr edit. All of these actions use the user's GitHub identity and publish code and text to a remote service. The skill only defers to the repository's git safety protocol. It decides itself which changes count as task-related, so unrelated or sensitive local changes could be committed and pushed.

## Fix applied

- Fix label: hygiene
- Targets: low other
- After the fix: low other (claude review): \~ targeted, not re-verified
- Fix id: sha256:3c86de7ffe4684e17375dbc4ba942c72272f9dddfc1bfc89674195af5def2a6e
- Changed: SKILL.md
- Fix offered: hygiene, checked

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
