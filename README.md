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

`agents/openai.yaml` belongs inside the `goal-prompt/` skill directory. It is not a repository-root agent config.

## Installation / Packaging

One-line install:

```bash
tmpdir="$(mktemp -d)" && git clone --depth 1 https://github.com/kt-aicoding/skill-goal.git "$tmpdir" && mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills" && rm -rf "${CODEX_HOME:-$HOME/.codex}/skills/goal-prompt" && cp -R "$tmpdir/goal-prompt" "${CODEX_HOME:-$HOME/.codex}/skills/goal-prompt"
```

For local development, copy the `goal-prompt/` directory into a Codex skills directory:

```bash
cp -R goal-prompt "${CODEX_HOME:-$HOME/.codex}/skills/"
```

The actual skill name is defined in `goal-prompt/SKILL.md` as `goal-prompt`.

## Use Cases

- Convert a broad request into an active-goal objective.
- Design a long-running migration, deployment, QA, audit, or documentation goal.
- Define per-project execution loops and blocker policy.
- Write completion criteria that require real evidence rather than intent.
- Produce a short `/goal` command and compact `create_goal` objective from a full goal prompt.
- Split complex goals into a durable instruction file plus a short objective when the `/goal` limit would be exceeded.

## Trigger Examples

Good trigger requests:

- `Use $goal-prompt to design a resumable goal for migrating every repo in this workspace.`
- `Create a /goal objective with done criteria and blocker policy for this deployment audit.`
- `Turn this vague multi-project cleanup into a long-running goal prompt.`
- `My /goal objective exceeded 4,000 characters; rewrite it as a file-backed goal.`

Non-trigger requests:

- `Fix this lint error now.`
- `Make a short checklist.`
- `Brainstorm app names.`
- `Write generic prompt copy that is not meant to become a long-running goal.`

## Validation

Local structural checks:

```bash
test -f goal-prompt/SKILL.md
test -f goal-prompt/agents/openai.yaml
test -f goal-prompt/references/goal-prompt-patterns.md
python3 - <<'PY'
from pathlib import Path
for path in [
    "goal-prompt/SKILL.md",
    "goal-prompt/agents/openai.yaml",
    "goal-prompt/references/goal-prompt-patterns.md",
]:
    text = Path(path).read_text()
    assert text.strip(), path
print("basic structure ok")
PY
```

Optional Codex skill validation, if you have the `skill-creator` system skill available:

```bash
python3 "${CODEX_HOME:-$HOME/.codex}/skills/.system/skill-creator/scripts/quick_validate.py" goal-prompt
```

Expected output:

```text
Skill is valid!
```

## Safety

- Do not include real secrets, cookies, private tokens, or copied `.env` values in generated goals.
- Use secret names, env var names, secret-manager references, or access prerequisites instead.
- Require redacted field-level verification if local credential diagnosis is explicitly needed.
- Pause before destructive actions, paid operations, broad rewrites, or irreversible production changes unless the user already authorized them.
- Keep paste-ready `/goal` and `create_goal` objectives below platform limits; put long instructions in a referenced file.

## Notes

- The repository name is `skill-goal`.
- The actual skill name is `goal-prompt`.
- Do not place secrets, private paths, tokens, or real API keys in skill files.
