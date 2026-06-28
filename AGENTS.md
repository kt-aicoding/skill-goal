# Repository Guidelines

- GitHub: `kt-aicoding/skill-goal`
- Category: standalone Codex skill repository.
- Actual skill name: `goal-prompt`; repository name is `skill-goal`.

## Editing Rules

- Keep skill implementation under `goal-prompt/`.
- Keep `goal-prompt/SKILL.md` concise; put longer patterns, examples, and references in `goal-prompt/references/`.
- Do not include real secrets, cookies, tokens, private API keys, or copied `.env` values in skill files or examples.
- Preserve file-backed goal guidance: long goals should live in a referenced instruction file, while `/goal` and `create_goal` objectives stay compact.

## Validation

Run the structural checks from `README.md` after edits:

```bash
test -f goal-prompt/SKILL.md
test -f goal-prompt/agents/openai.yaml
test -f goal-prompt/references/goal-prompt-patterns.md
```

If available, also run the local skill validator against `goal-prompt/`.
