# esr review: plugins/rpi/skills/rpi

Written by esr (`esr owner/repo`) from its own records. It never quotes the skill beyond what a finding's text holds; the summary and the claude review's findings below are model text.

- Upstream: humanlayer/skills
- Upstream commit reviewed: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Skill tree reviewed: 66f95187819ade74e6193bf3af7b51ae6c3d11df
- Date: 2026-11-25
- Choice: fix (re-checked and applied)

## What it does (claude review, model text)

    A multi-phase software workflow skill (research questions, research, design
    discussion, PRD, TDD, outline, implementation). It guides the agent through
    each phase, writes workflow documents into a task directory under
    .agents/artifacts, and hands work to bundled subagent prompts for code
    search, web research and implementation.

## What it could do (static profile)

- reaches https hosts: vite.dev

## Public scores (outside opinions)

- public scores (outside opinions): Gen safe, Socket safe (0 alerts), Snyk medium; BAD: Snyk medium

## Findings

- medium undeclared_egress (claude review: model text, it can be wrong): The web-search-researcher subagent is told to use Bash \`curl -sL\` instead of the WebFetch tool for llms.txt, .md and .txt URLs. It is told to always probe \`https://\<site\>/llms.txt\` for any site it knows of, and to curl sub-pages it finds inside fetched results. These are network requests that go through a general shell rather than the host's scoped web tools, to hosts picked by the model and by fetched content. The functionality contract lists no external services. The research phases call this subagent "when needed" without requiring the user to ask for web access; only iterate-research limits it to cases where the user requests web research.
- medium prompt_injection (claude review: model text, it can be wrong): Web content (search results, WebFetch pages and curl'd llms.txt files with their linked sub-pages) is loaded into a subagent that also holds Bash, Read, Grep, Glob and Skill tools. The file calls llms.txt a "VERY valuable source of optimized information for claude" and never says to treat fetched text as data. Injected instructions in a fetched page could lead the subagent to run shell commands or read local files. Its findings also flow into research documents that later sessions treat as authoritative context, and the implementer acts on those documents.
- low unsafe_execution (claude review: model text, it can be wrong): The implementer subagent runs every automated verification command listed in the outline's Validation sections, using Bash with Edit and Write access. These commands come from workflow documents saved under .agents/artifacts. Those documents persist between sessions, can be edited by anyone with repository access, and may contain content derived from earlier web research. Running repository checks is in scope for implementation, but the commands are not tied to a fixed or reviewed list.
- low prompt_injection (claude review: model text, it can be wrong): The review reports fetched text used as instructions at line 42 of references/subagents/web-search-researcher.md.
- low excessive_capability (claude review: model text, it can be wrong): The codebase-locator, codebase-analyzer and codebase-pattern-finder subagents are described as read-only documentarians. The locator is even told not to read file contents. Even so, each is given Bash and Skill tools, which allow arbitrary command execution and invoking other skills. In the fallback path, SKILL.md passes these instructions into a generic subagent, which may receive whatever tools the host gives it.
- low excessive_capability (claude review: model text, it can be wrong): The review reports a read or permission that is too broad at line 4 of references/subagents/codebase-analyzer.md.
- low excessive_capability (claude review: model text, it can be wrong): The review reports a read or permission that is too broad at line 4 of references/subagents/codebase-locator.md.

## Fix applied

- Fix label: security
- Targets: medium undeclared\_egress
- Targets: medium prompt\_injection
- Targets: low unsafe\_execution
- Targets: low prompt\_injection
- Targets: low excessive\_capability
- Targets: low excessive\_capability
- Targets: low excessive\_capability
- After the fix: medium undeclared\_egress (claude review): \~ targeted, not re-verified
- After the fix: medium prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: low unsafe\_execution (claude review): \~ targeted, not re-verified
- After the fix: low prompt\_injection (claude review): \~ targeted, not re-verified
- After the fix: low excessive\_capability (claude review): \~ targeted, not re-verified
- After the fix: low excessive\_capability (claude review): \~ targeted, not re-verified
- After the fix: low excessive\_capability (claude review): \~ targeted, not re-verified
- Fix id: sha256:18f001a6e2153a8abadf6c77d566c15c7fc86622355dd5a7968f5a84b822eae6
- Changed: references/subagents/codebase-analyzer.md
- Changed: references/subagents/codebase-locator.md
- Changed: references/subagents/codebase-pattern-finder.md
- Changed: references/subagents/outline-implementer-agent.md
- Changed: references/subagents/web-search-researcher.md
- Fix offered: security, checked

## Claude Code reported

During this run Claude Code reported these values, which differ from what esr expects. None of them lets the reviewer act; the review went ahead.

- init.plugins: cc-plugin-privacy@builtin
