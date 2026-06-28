---
name: goal-prompt
description: Draft or improve durable Codex `/goal` prompts and active-goal objectives for long-running agent work. Use when the user asks for a goal prompt, objective, definition of done, success criteria, constraints, milestones, verification plan, blocker policy, completion audit, or compaction-safe execution prompt. Avoid using for ordinary short task plans or implementation unless the user wants a reusable goal prompt.
---

# Goal Prompt

## Purpose

Turn a broad work request into a goal prompt that an agent can execute for hours, resume safely, verify objectively, and close only when evidence proves completion. Codex `/goal` objectives have a hard length limit, so this skill must produce a short executable objective and move detailed instructions into a referenced file when needed.

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

7. Choose inline vs file-backed delivery.
   - If the complete goal would likely exceed 3,500 characters, do not output it as the `/goal` objective.
   - Use a file-backed goal: provide or create a `docs/goal.md`, `docs/goals/<slug>.md`, or similarly stable instruction file, then make the `/goal` objective a short command to follow that file.
   - Keep the actual `/goal` command and `create_goal objective` comfortably under 4,000 characters; target 500-1,200 characters.
   - The short objective must include the outcome, scope, required instruction file, verification requirement, secret safety, blocker policy, and completion audit trigger.

8. Specify final reporting.
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

For complex work, put these sections in the instruction file rather than the `/goal` objective itself.

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

1. `Instruction file`: for complex goals, a recommended path and complete file content in a fenced code block. If the user asked you to update the current workspace and it is safe, create the file instead of only showing it.
2. `Short /goal command`: a paste-ready objective that is always under 4,000 characters and normally under 1,200 characters. If an instruction file exists, this should say to follow that file.
3. `create_goal objective`: a compact single-string objective for `create_goal`; keep it under 1,200 characters and never put the full long plan here.
4. `Why this is measurable`: one short explanation of the completion signal.
5. `Assumptions`: only material assumptions the agent may proceed under.
6. `Pause conditions`: when the agent should stop and ask the user.
7. `Optional refinements`: 2-3 questions only if they materially improve the goal.

Never return a `/goal` objective longer than 4,000 characters. If the full goal is long, the correct output is a short objective that references a durable instruction file.
