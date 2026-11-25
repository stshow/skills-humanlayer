# esr review: plugins/design-control-loop/skills/design-control-loop

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: a8a023f8f62a384a76297b31d9f844c5ad3adf95
- Date: 2026-11-25
- Choice: fix (re-checked and applied)

## What it does (claude review, model text)

    The skill interviews the user to design an agentic control loop (set point,
    sensor, controller, coding-agent actuator) for their codebase. It then
    builds the loop in the user's repo: local scripts, a repo-local actuator
    skill, a memory file, an /iterate helper script and a scheduled GitHub
    Actions workflow that runs a coding agent and opens PRs.

## What it could do (static profile)

- reaches https hosts: agentskills.io, github.com
- ships scripts: references/agent-iteration.ts

## Public scores (outside opinions)

- public scores (outside opinions): Gen safe, Socket critical (2 alerts), Snyk critical; BAD: Socket critical (2 alerts), Snyk critical

## Findings

- medium mutable_dependency (esr rules; rule unpinned-install in references/workflow-template.yml line 161): references/workflow-template.yml installs a package without pinning its version, so its code can change at any time
- medium prompt_injection (claude review: model text, it can be wrong): The /iterate helper builds the agent prompt from the raw PR body and from every issue comment and review comment on the PR, whoever wrote them. Only the author of the /iterate comment is checked for OWNER/MEMBER/COLLABORATOR. The prompt goes to a coding agent that runs with ANTHROPIC\_API\_KEY and GH\_TOKEN in its environment, is told to commit and push, and is told to write durable guidance into the memory file. Every later scheduled run loads that memory file. A comment from an outside user on a public repo can therefore inject instructions that the agent may act on, and those instructions can be stored in memory and steer future runs.
- medium excessive_capability (claude review: model text, it can be wrong): The recommended agent invocations turn off every permission prompt (\`--permission-mode bypassPermissions\`, \`--sandbox danger-full-access --ask-for-approval never\`, \`--dangerously-skip-permissions\`). The prompt template tells the agent never to ask for approval. The workflow grants contents, pull-requests and issues write access. The file does say this is only for trusted, isolated runners. Combined with the injectable /iterate prompt, though, it gives an unrestricted agent push rights and API secrets.
- medium mutable_dependency (claude review: model text, it can be wrong): The workflow template and runner templates run coding-agent CLIs fetched at run time without a fixed version: \`bunx @humanlayer/cli@latest\`, \`npm install -g @anthropic-ai/claude-code\`, \`@openai/codex\` and \`opencode-ai\`. Each is executed in a step whose environment holds ANTHROPIC\_API\_KEY and a repo-write GH\_TOKEN. The example sensor also runs \`bunx react-doctor\` unpinned. A compromised or changed release would run with those secrets.
- low mutable_dependency (esr rules; rule unpinned-install in SKILL.md line 151): SKILL.md installs a package without pinning its version, so its code can change at any time
- low mutable_dependency (esr rules; rule unpinned-install in references/agent-runner-templates.md line 25): references/agent-runner-templates.md installs a package without pinning its version, so its code can change at any time
- low mutable_dependency (esr rules; rule unpinned-install in references/example-control-loop.md line 16): references/example-control-loop.md installs a package without pinning its version, so its code can change at any time
- low unsafe_execution (claude review: model text, it can be wrong): ghJson builds a single string \`gh api ${pathAndArgs}\` from the --repo and --pr-number arguments and runs it with \`bash -lc\`. Shell metacharacters in those values would be executed, and the login shell also loads profile files. Separately, the workflow interpolates \`${{ steps.agent\_gate.outputs.head\_ref }}\` (a PR branch name) directly into a \`run:\` script. In the template these inputs come mostly from GitHub-controlled values, so exploitability is unclear.
- low input_validation (claude review: model text, it can be wrong): /iterate routing trusts any PR whose body contains the \`\<\!-- codelayer-agent:workflow=... --\>\` marker. Anyone who opens a PR can add that marker to its body. If a maintainer then comments /iterate on such a PR, the workflow checks out that PR's head ref and runs the agent with secrets on it.
- low persistence (claude review: model text, it can be wrong): The skill writes lasting automation into the user's repo: a cron-scheduled GitHub Actions workflow that runs a coding agent, a new repo-local agent skill under \`.claude/skills/\` or \`.agents/skills/\`, and a memory file that is loaded into every future run. This is the stated purpose, but it creates recurring agent execution that outlives the session.
- low input_validation (claude review: model text, it can be wrong): The review reports an unquoted shell variable at line 263 of references/workflow-template.yml.
- low input_validation (claude review: model text, it can be wrong): The review reports an unquoted shell variable at line 123 of references/agent-iteration.ts.
- low prompt_injection (claude review: model text, it can be wrong): The review reports fetched text used as instructions at line 104 of references/agent-iteration.ts.
- low other (claude review: model text, it can be wrong): The skill tells the agent to run the actuator locally through a paid model API, and to dry-run the workflow by adding a push trigger, pushing the branch and letting CI open a PR. These steps use the user's API credits and git identity. The text does not tie them to an explicit request from the user.

## Fix applied

- Fix label: security
- Targets: medium mutable\_dependency
- Targets: medium prompt\_injection
- Targets: medium mutable\_dependency
- Targets: low mutable\_dependency
- Targets: low mutable\_dependency
- Targets: low mutable\_dependency
- Targets: low unsafe\_execution
- Targets: low input\_validation
- Targets: low input\_validation
- Targets: low input\_validation
- Targets: low prompt\_injection
- Targets: low other
- After the fix: medium mutable\_dependency (esr rules): ✗ still there
- After the fix: medium prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: medium excessive\_capability (claude review): – not addressed
- After the fix: medium mutable\_dependency (claude review): \~ targeted, not re-verified
- After the fix: low mutable\_dependency (esr rules): ✗ still there
- After the fix: low mutable\_dependency (esr rules): ✗ still there
- After the fix: low mutable\_dependency (esr rules): ✗ still there
- After the fix: low unsafe\_execution (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low persistence (claude review): – not addressed
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low input\_validation (claude review): \~ targeted, not re-verified
- After the fix: low prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: low other (claude review): \~ targeted, not re-verified
- Fix id: sha256:3221f657af477a08ce0875cab459f2ad49c63ace55cd9e4f8d9306ae12b3f519
- Changed: SKILL.md
- Changed: references/agent-iteration.ts
- Changed: references/agent-runner-templates.md
- Changed: references/example-control-loop.md
- Changed: references/workflow-template.yml
- Fix offered: security, checked
- medium excessive\_capability (claude review): No fix. The claude review says this is what the skill is for. esr still flags it.
- low persistence (claude review): No fix. The claude review says this is what the skill is for.

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
