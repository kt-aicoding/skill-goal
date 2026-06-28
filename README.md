# skill-goal

Standalone Codex skill project for the `goal-prompt` skill.

`goal-prompt` turns broad work requests into durable, verifiable long-running goal prompts. It is designed for tasks that need to survive context compaction, run item-by-item for hours, handle external blockers, and close only after a current-state completion audit.

## Contents

```text
goal-prompt/
  SKILL.md
  agents/openai.yaml
  references/goal-prompt-patterns.md
```

## Use Cases

- Convert a broad request into an active-goal objective.
- Design a long-running migration, deployment, QA, audit, or documentation goal.
- Define per-project execution loops and blocker policy.
- Write completion criteria that require real evidence rather than intent.
- Produce a shorter `create_goal` objective string from a full goal prompt.

## Validation

```bash
python3 /path/to/skill-creator/scripts/quick_validate.py goal-prompt
```

## Notes

- The repository name is `skill-goal`.
- The actual skill name is `goal-prompt`.
- Do not place secrets, private paths, tokens, or real API keys in skill files.
