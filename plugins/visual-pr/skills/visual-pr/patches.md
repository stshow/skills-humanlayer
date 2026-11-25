# esr Patch log: plugins/visual-pr/skills/visual-pr

Every change made to this Upstream skill on this branch, in the order esr applies them to the Upstream text. Written by esr; esr reads the block at the end on the next run, and asks about any entry it did not record itself.

The lines an entry replaces are kept only as a hash, never as text. To see a whole change, run git diff from the entry's Upstream commit to this branch, limited to this skill's folder.

## 1. fix (hygiene)

- Id: sha256:c78ba7ffcf5382e922db966dcbe758fbfcd81d5ad9cf2510db4d2bf67bf829a3
- Reason: Model-proposed fix for l1-fnd-24054eb19521187b99e5a4e0
- Upstream commit: 4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329
- Resolves: rule Autonomous commit/push/PR creation under the user's identity without a stated limit on which changes are included in SKILL.md

Changes SKILL.md: 0 line(s) removed and 1 line(s) added, in 1 place(s).

Added:

    +   - Stage only files this task changed, each by explicit path (never `git add -A`, `git add .` or `git commit -a`); leave unrelated, untracked or secret-bearing changes unstaged.

```esr-patches
{"entries":[{"entry":{"changes":[{"hunks":[{"edits":[{"add":"   - Stage only files this task changed, each by explicit path (never `git add -A`, `git add .` or `git commit -a`); leave unrelated, untracked or secret-bearing changes unstaged.\n","at":3,"remove":0}],"old_bytes":563,"old_digest":"sha256:15ac50d40a188d4a3cd79e8c38835decd836de5e13bcf353ce725f7b13f32aa6","old_lines":6}],"old_bytes":null,"old_digest":null,"op":"change","path":"SKILL.md"}],"kind":"fix (hygiene)","reason":"Model-proposed fix for l1-fnd-24054eb19521187b99e5a4e0","resolved":[{"file":"SKILL.md","rule":"Autonomous commit/push/PR creation under the user's identity without a stated limit on which changes are included"}],"upstream_commit":"4d69d20f6e9b57b4fcf0d501ccfb1be961f0f329"},"id":"sha256:c78ba7ffcf5382e922db966dcbe758fbfcd81d5ad9cf2510db4d2bf67bf829a3"}],"skill":"plugins/visual-pr/skills/visual-pr","version":1}
```
