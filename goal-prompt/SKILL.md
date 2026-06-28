---
name: goal-prompt
description: Draft or improve durable Codex `/goal` prompts and active-goal objectives for long-running agent work. Use when the user asks for a goal prompt, objective, definition of done, success criteria, constraints, milestones, verification plan, blocker policy, completion audit, or compaction-safe execution prompt. Avoid using for ordinary short task plans or implementation unless the user wants a reusable goal prompt.
---

# Goal Prompt

## Purpose

Turn a broad work request into a goal prompt that an agent can execute for hours, resume safely, verify objectively, and close only when evidence proves completion.

For detailed templates and examples, read `references/goal-prompt-patterns.md`.

## Use When

- The user explicitly asks for a goal prompt, active-goal objective, `/goal` text, or long-running objective.
- The task must survive context compaction, resumes, or interruptions.
- The work needs measurable "done when" criteria, validation evidence, and stop conditions.
- The request spans many projects, deployments, audits, docs, research items, or QA flows.
- The user wants blocker handling, autonomy boundaries, or a completion audit.

## Do Not Use When

- The user wants a normal short plan, brainstorm, one-off code fix, or direct implementation.
- The task is small enough to execute immediately without a durable objective.
- The user asks for generic prompt writing unrelated to long-running agent work.
- The output would encourage unsafe autonomy, secret inspection, irreversible actions, or broad changes without confirmation.

## Workflow

1. Identify the real finish line.
   - Name the concrete state that must be true at the end.
   - Prefer deliverables and observable behavior over activity words like "improve", "continue", or "handle".
   - Preserve the user's full scope; do not shrink it to the easiest passing subset.

2. Define the working surface.
   - Include root directories, repositories, branches, domains, services, docs, dashboards, issue/PR links, and required access prerequisites.
   - Say which files or systems must be read before continuing after a resume.
   - Use secret names or environment variable names only. Do not include raw values, private cookie paths, account tokens, or instructions to print secrets.

3. Decompose into repeatable loops.
   - For multi-project work, use one complete loop per project or module.
   - A strong loop has inventory, targeted reads, implementation, local validation, live validation when applicable, commit/push or artifact publication, and ledger update.
   - Add 2-5 milestone outcomes for large or ambiguous work; avoid turning milestones into brittle microsteps.
   - State selection priority so the agent can choose the next item without asking.

4. Write evidence-based completion criteria.
   - List exact commands, checks, API calls, browser flows, remote refs, rendered outputs, or review states that prove success.
   - Require a completion audit before marking the goal complete.
   - Treat missing, indirect, narrow, or stale evidence as incomplete.

5. Add blocker policy.
   - Define which blockers are external and acceptable to record.
   - Use if-then wording for predictable blockers.
   - Require blocker evidence: owner/system, timestamp, attempted action, current error, why safe local work cannot continue, next action, and whether the item still counts as acceptable for completion.
   - Require the agent to finish safe local work, record the blocker, then continue other items.
   - Reserve "blocked" for true impasse, not inconvenience.

6. Add change-safety rules.
   - Protect user work in dirty repositories.
   - Stage and commit only goal-related files.
   - For deployment work, prefer deploying the exact commit that was pushed.
   - Separate autonomous actions from pause-for-confirmation actions.
   - Never output or commit secrets.

7. Specify final reporting.
   - Require a concise per-item summary with repo, branch, commit, deployment target, validation results, unresolved blockers, and dirty-worktree notes.
   - Include ledger/audit file paths updated during the run.

## Secret Handling

- Prefer secret manager names, env var names, or platform names over local file paths.
- Do not ask an agent to read, summarize, or print private credential files unless the user explicitly requests local configuration diagnosis.
- If a credential file must be inspected, require redacted output and field-level verification only.
- Public goal prompts must never include raw keys, cookies, account tokens, personal access tokens, or copied `.env` contents.

## Goal Sections

Every full generated goal should include these sections unless the user asks for a compact objective only:

- `Outcome`: the final observable state.
- `Context`: where to work and what to read first.
- `Constraints`: safety, scope, dirty-worktree, secret, and autonomy limits.
- `Milestones`: 2-5 proximal outcomes for larger work.
- `Done when`: measurable completion criteria.
- `Verification`: commands, live checks, audits, or review states that prove completion.
- `If blocked`: if-then rules for common blockers and pause conditions.

## Quality Bar

A generated goal prompt should answer:

- What concrete outcome must be true?
- What artifacts, repos, services, or environments are in scope?
- What evidence proves each requirement?
- What repeatable loop should the agent run for each item?
- What must happen after context compaction or interruption?
- What may be skipped, deferred, or recorded as blocked?
- What must never happen, such as leaking secrets or reverting user work?
- What exact condition allows `complete`?

## Ask Clarifying Questions When

Ask at most two concise questions before generating the goal only when missing information changes the finish line, safety boundary, or validator. Otherwise state assumptions and proceed.

Useful clarifications:

- Which environment must prove success: local, staging, production, or all?
- What validator is authoritative: tests, CI, browser flow, API response, benchmark, review state, or user approval?
- Are deployments, payments, destructive data changes, or broad rewrites allowed autonomously?

## Anti-Patterns

Avoid these weak goal shapes:

- "Make progress on X."
- "Keep working until it looks done."
- "Fix all issues" without an issue source, validator, or stop condition.
- "Deploy everything" without naming environments or verification.
- "Use best judgment" without guardrails for dirty worktrees, secrets, or blockers.
- "Mark complete when tests pass" when the scope also includes deployment, docs, or live behavior.

## Output Shape

When the user asks for a goal prompt, output:

1. `Final goal objective`: a full reusable goal prompt in a fenced code block.
2. `create_goal objective`: a shorter single-string objective when useful.
3. `Why this is measurable`: one short explanation of the completion signal.
4. `Assumptions`: only material assumptions the agent may proceed under.
5. `Pause conditions`: when the agent should stop and ask the user.
6. `Optional refinements`: 2-3 questions only if they materially improve the goal.
