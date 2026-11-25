# esr review: plugins/build-iterated-agentic-loop/skills/build-iterated-agentic-loop

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: fa04a5cc4842e561e7182b3a074b91c18ffbc1df
- Date: 2026-11-25
- Choice: fix (re-checked and applied)

## What it does (claude review, model text)

    Helps an agent create, in the user's repository, a repo-local skill plus a
    GitHub Actions workflow that runs a headless coding agent on a schedule,
    manually, or when someone comments /iterate. It also creates a memory file
    and an optional helper script, then commits and pushes them to bootstrap the
    workflow.

## What it could do (static profile)

- reaches https hosts: agentskills.io
- ships scripts: references/agent-iteration.ts

## Public scores (outside opinions)

- public scores (outside opinions): Gen medium, Socket critical (2 alerts), Snyk critical; BAD: Gen medium, Socket critical (2 alerts), Snyk critical

## Findings

- medium mutable_dependency (esr rules; rule unpinned-install in references/workflow-template.yml line 164): references/workflow-template.yml installs a package without pinning its version, so its code can change at any time
- medium mutable_dependency (claude review: model text, it can be wrong): The generated CI installs and runs coding-agent CLIs without pinned versions: @humanlayer/cli@latest via bunx, plus npm/bun global installs of @anthropic-ai/claude-code, @openai/codex and opencode-ai. They run with API keys and a write token. A compromised or changed release would run in the user's CI with those secrets.
- medium excessive_capability (claude review: model text, it can be wrong): Every agent runner template turns off the agent's permission system (bypassPermissions, --sandbox danger-full-access with --ask-for-approval never, --dangerously-skip-permissions). The agent runs with API keys and a write-scoped GITHUB\_TOKEN in its environment. Combined with untrusted prompt content, the agent can run arbitrary commands and push code. The file notes this is appropriate only on trusted, isolated runners, but the templates default to ubuntu-latest with secrets attached.
- medium prompt_injection (claude review: model text, it can be wrong): The /iterate prompt builder pastes the raw PR title, PR body, all issue comments and all review comments, from any GitHub user, into the prompt of a coding agent. That agent runs with permission checks disabled and holds ANTHROPIC\_API\_KEY and a GITHUB\_TOKEN that can write contents, pull requests and issues. Only the /iterate comment itself has to come from an OWNER, MEMBER or COLLABORATOR. Anyone who can comment on the PR can plant instructions that the agent may follow: changing code, pushing commits or reading secrets from its environment.
- medium persistence (claude review: model text, it can be wrong): The iteration agent is told to distill feedback into the agent-memory file and commit it. That file is then interpolated into the prompt of every future scheduled run. Instructions injected through PR comments can therefore persist in the repository and steer later unattended runs.
- low mutable_dependency (esr rules; rule unpinned-install in SKILL.md line 67): SKILL.md installs a package without pinning its version, so its code can change at any time
- low mutable_dependency (esr rules; rule unpinned-install in references/agent-runner-templates.md line 17): references/agent-runner-templates.md installs a package without pinning its version, so its code can change at any time
- low secret_exposure (claude review: model text, it can be wrong): The full verbose agent output is uploaded as a workflow artifact on every run. With permissions bypassed and prompt-injectable input, the agent could print environment secrets into that output. Artifacts can be read by anyone with read access to the repository.
- low input_validation (claude review: model text, it can be wrong): The head\_ref and PR number outputs, which come from PR data, are interpolated with ${{ }} directly into run scripts instead of being passed through environment variables. The marker gate only greps the PR body, so any PR author can add the marker. If a maintainer comments /iterate on such a PR, a crafted branch name could inject shell commands into a job that holds a write token.
- low input_validation (claude review: model text, it can be wrong): The review reports an unquoted shell variable at line 123 of references/agent-iteration.ts.
- low prompt_injection (claude review: model text, it can be wrong): The review reports fetched text used as instructions at line 100 of references/agent-iteration.ts.
- low other (claude review: model text, it can be wrong): The skill tells the local agent to commit and push the new workflow and to add a temporary push trigger so the workflow runs at once. That uses the user's GitHub identity, and the run spends money from the repository's LLM API key. The skill gives no explicit confirmation point before pushing.
- low input_validation (claude review: model text, it can be wrong): The review reports an unquoted shell variable at line 249 of references/workflow-template.yml.
- low unsafe_execution (claude review: model text, it can be wrong): ghJson builds a command string from the repo and PR number arguments and runs it through bash -lc. This defeats Bun's argument escaping, so shell metacharacters in those values would run. Current callers pass values from the GitHub context, which limits exposure.

## Fix applied

- Fix label: security
- Targets: medium mutable\_dependency
- Targets: medium mutable\_dependency
- Targets: medium prompt\_injection
- Targets: medium persistence
- Targets: low mutable\_dependency
- Targets: low mutable\_dependency
- Targets: low input\_validation
- Targets: low input\_validation
- Targets: low prompt\_injection
- Targets: low other
- Targets: low input\_validation
- Targets: low unsafe\_execution
- After the fix: medium mutable\_dependency (esr rules): ✗ still there
- After the fix: medium mutable\_dependency (claude review): \~ targeted, not re-verified
- After the fix: medium excessive\_capability (claude review): – not addressed
- After the fix: medium prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: medium persistence (claude review): \~ targeted, not re-verified
- After the fix: low mutable\_dependency (esr rules): ✗ still there
- After the fix: low mutable\_dependency (esr rules): ✗ still there
- After the fix: low secret\_exposure (claude review): – not addressed
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: low other (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low unsafe\_execution (claude review): \~ targeted, not re-verified
- Fix id: sha256:91df3e02fc86a142aa59809bb01cbfa134d4239d7f8d507c465e559ef68bb276
- Changed: SKILL.md
- Changed: references/agent-iteration.ts
- Changed: references/agent-runner-templates.md
- Changed: references/workflow-template.yml
- Fix offered: security, checked
- medium excessive\_capability (claude review): No fix. The claude review says the only fix is a confirmation step or an allowlist. esr does not add these.
- low secret\_exposure (claude review): No fix by esr. Remove the secret and rotate it yourself.

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
