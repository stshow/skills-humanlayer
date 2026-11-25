# esr review: plugins/narrow-react-prop-types/skills/narrow-react-prop-types

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: 172dfb205f5f1419354b431cbb903e1afd55ad87
- Date: 2026-11-25
- Choice: fix (re-checked and applied)

## What it does (claude review, model text)

    Guides an agent to tighten React component prop types so they match only the
    states used by live (non-test, non-Storybook) call sites, then to update
    stories and tests and run package typechecks. It also ships an example
    GitHub Actions workflow that runs the same task on a schedule through a
    CodeLayer CLI agent and opens pull requests.

## What it could do (static profile)

- no network hosts, tool grants, scripts or binaries found

## Public scores (outside opinions)

- public scores (outside opinions): Gen safe, Socket critical (1 alert), Snyk medium; BAD: Socket critical (1 alert), Snyk medium

## Findings

- medium mutable_dependency (esr rules; rule unpinned-install in references/agent-narrow-component-props.yml line 154): references/agent-narrow-component-props.yml installs a package without pinning its version, so its code can change at any time
- medium mutable_dependency (claude review: model text, it can be wrong): The example CI workflow runs the agent CLI with \`bunx @humanlayer/cli@latest\` in both the scheduled and the /iterate paths. The step's environment holds the Anthropic API key and a GitHub token with contents, pull-requests and issues write permissions. Whatever version is newest on the registry at run time executes with those secrets.
- medium prompt_injection (claude review: model text, it can be wrong): In /iterate mode, the example workflow builds the agent prompt from the PR body, PR comments, review comments, the triggering comment and a memory file. The agent then receives that prompt while it has write access through the GitHub token and holds the Anthropic API key in its environment. Only the triggering comment is limited to owners, members and collaborators. Other PR comments and review comments can come from any user who is able to comment, and they would become agent instructions.
- low input_validation (claude review: model text, it can be wrong): The PR head branch name and PR number come from the GitHub API, and the workflow interpolates them directly into shell scripts with \`${{ }}\` expressions, for example in \`git push origin HEAD:${{ steps.iterate.outputs.head\_ref }}\`. A branch name containing shell metacharacters could inject commands into the step that holds the write token.
- low input_validation (claude review: model text, it can be wrong): The review reports an unquoted shell variable at line 280 of references/agent-narrow-component-props.yml.
- low path_handling (claude review: model text, it can be wrong): The review reports an unsafe temp file at line 152 of references/agent-narrow-component-props.yml.
- low prompt_injection (claude review: model text, it can be wrong): The review reports fetched text used as instructions at line 146 of references/agent-narrow-component-props.yml.

## Fix applied

- Fix label: security
- Targets: medium mutable\_dependency
- Targets: medium mutable\_dependency
- Targets: medium prompt\_injection
- Targets: low input\_validation
- Targets: low input\_validation
- Targets: low path\_handling
- Targets: low prompt\_injection
- After the fix: medium mutable\_dependency (esr rules): ✗ still there
- After the fix: medium mutable\_dependency (claude review): \~ targeted, not re-verified
- After the fix: medium prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low path\_handling (claude review): \~ targeted, not re-verified
- After the fix: low prompt\_injection (claude review): \~ targeted, not re-verified
- Fix id: sha256:e8f1aa988fe52a27bc45c65eaf530cd698aaa79f05652c0d948e8442b42e5c40
- Changed: references/agent-narrow-component-props.yml
- Fix offered: security, checked

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
