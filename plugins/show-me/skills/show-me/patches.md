# esr Patch log: plugins/show-me/skills/show-me

Every change made to this Upstream skill on this branch, in the order esr applies them to the Upstream text. Written by esr; esr reads the block at the end on the next run, and asks about any entry it did not record itself.

The lines an entry replaces are kept only as a hash, never as text. To see a whole change, run git diff from the entry's Upstream commit to this branch, limited to this skill's folder.

## 1. fix (hygiene)

- Id: sha256:1e9ace6881e8f0821565b26e890098df360b5d101ba473d17f57f1aad05fc0f6
- Reason: Model-proposed fix for l1-fnd-d69d59ccdd3b0d0b3ffbd662, l1-fnd-d266265826a6cb229cd5fab7
- Upstream commit: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Resolves: rule Shell command template interpolates a free-text filename segment without quoting or sanitisation in SKILL.md
- Resolves: rule raised weakness unquoted\_shell in SKILL.md

Changes SKILL.md: 2 line(s) removed and 2 line(s) added, in 1 place(s).

Added:

    +- For a visual UI, layout, state comparison, or concept too dense for Mermaid, write one focused HTML file — a diagram, an infographic, or a short slide deck, whichever fits the point. Match the product's colors, type, spacing, and components; use real labels and data; support desktop and mobile. Then open it for the user, using only lowercase letters, digits and hyphens for `{description}`:
    +Bash(open "path/to/show-me-{description}.html")

```esr-patches
{"entries":[{"entry":{"changes":[{"hunks":[{"edits":[{"add":"- For a visual UI, layout, state comparison, or concept too dense for Mermaid, write one focused HTML file \u2014 a diagram, an infographic, or a short slide deck, whichever fits the point. Match the product's colors, type, spacing, and components; use real labels and data; support desktop and mobile. Then open it for the user, using only lowercase letters, digits and hyphens for `{description}`:\n","at":3,"remove":1},{"add":"Bash(open \"path/to/show-me-{description}.html\")\n","at":6,"remove":1}],"old_bytes":403,"old_digest":"sha256:42981ee798fe9375b47bdf5ffbe948d14415546668e83a8165681b0eb9d70500","old_lines":10}],"old_bytes":null,"old_digest":null,"op":"change","path":"SKILL.md"}],"kind":"fix (hygiene)","reason":"Model-proposed fix for l1-fnd-d69d59ccdd3b0d0b3ffbd662, l1-fnd-d266265826a6cb229cd5fab7","resolved":[{"file":"SKILL.md","rule":"Shell command template interpolates a free-text filename segment without quoting or sanitisation"},{"file":"SKILL.md","rule":"raised weakness unquoted_shell"}],"upstream_commit":"4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329"},"id":"sha256:1e9ace6881e8f0821565b26e890098df360b5d101ba473d17f57f1aad05fc0f6"}],"skill":"plugins/show-me/skills/show-me","version":1}
```
