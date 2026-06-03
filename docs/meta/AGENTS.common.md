# Shared Codex Rules

Codex must read these shared rules before working in any adopted `okmrtis`
project. They apply unless a project-local `AGENTS.md` adds a stricter or more
specific rule.

## Git And Reproducibility

- Durable work should end as committed and pushed repository state, not as
  chat-only guidance.
- A project should be reproducible from Git plus documented authorized external
  inputs. If an input or generated output is not committed, record enough
  manifest information to obtain it, rerun the procedure, and verify the result.
- Treat the user's Downloads/downloads folders as volatile handoff scratch, not
  durable project state. Files there may be deleted immediately after a task, so
  reusable docs, memories, scripts, and automations must not assume those paths
  will remain available.
- Do not commit machine-local runtime state, credentials, tokens, browser
  profiles, local cache folders, dependency snapshots, or temporary upload state.
- Prefer small focused commits named by the durable behavior or rule being added.
- Before declaring GitHub management done, check the current branch, remote,
  worktree status, and whether durable files are still untracked.

## Local Project Rules

- Read this shared file, then the project-local `AGENTS.md`, README, and
  workflow docs before changing files.
- On this Windows Codex host, recheck current tooling before applying older
  workaround history. GitHub CLI, Python, npm/npx, Git LFS, PowerShell policy,
  and Git long-path behavior were refreshed on 2026-05-20; the current baseline
  lives in `okmrtis/meta` at `knowledge/codex-windows-tooling.md`.
- Project-local rules win when they are more specific to that repository,
  especially around real data, workbook edits, deployment, or safety constraints.
- If shared and local rules conflict, follow the stricter rule and update the
  relevant documentation when the conflict reveals a reusable lesson.

## Autonomy And Stop Points

- Treat task requests as requests to complete the objective, not only to explain
  how the user could complete it. When the user asks Codex to fix, update,
  create, inspect, run, or verify something, do the work directly when feasible.
- When a task is large or difficult enough that tool calls risk timing out,
  divide it into smaller staged batches, verify each batch, and continue from
  recorded intermediate state instead of attempting one broad run.
- If the practical effect is limited to the user's own local files, private
  workspace state, or other reversible personal scope, proceed through execution
  and verification after creating a suitable rollback path when risk warrants it.
- For deletion, overwrite, settings, account, payment, or other sensitive
  changes, first make the operation reversible with a backup, branch, copy,
  export, snapshot, log, or staged change. If the reversible effect remains in
  the user's own scope, proceed instead of stopping at instructions.
- For apparently irreversible work, first redesign the action so it is staged or
  reversible. Stop only when a meaningful irreversible external impact remains.
- Authentication trouble is not enough reason to stop. Check current CLI auth,
  connector state, local credentials, existing authorized app sessions, and
  alternate authorized paths before asking the user to intervene. Ask only when
  an interactive user-only step such as consent, 2FA, or a password is required.
- Stop before sending, posting, sharing, inviting, mentioning, notifying, or
  otherwise affecting other people or shared environments. In those cases,
  present the exact proposed action, content, recipients, and likely impact for
  user approval first because the user holds real-world accountability.

## Evidence And Verification

- Do not trust script success alone when a task changes durable artifacts. Verify
  the actual saved output or pushed remote state.
- For generated documents, spreadsheets, images, or uploads, keep enough compact
  evidence to explain what was checked without committing bulky scratch data.
- Before changing Excel, Word, or PowerPoint artifacts, choose the tool path and
  final verification gate from `okmrtis/meta` at
  `knowledge/office-artifact-workflow.md`; do this before the first write.
- Local Office automation must avoid interfering with the user's active work
  whenever possible. Do not open files in an existing visible Office session
  unless the target file is already open there and reuse is necessary; create a
  separate hidden COM instance for background work, and only close or quit COM
  objects that Codex itself opened or created.
- For private or large external inputs, record path/URL, file name, modified
  time, size, and hash when feasible.
- When a task starts from a file in Downloads/downloads, copy or stage anything
  needed for the run into a stable work root before relying on it, and record
  the original location only as a temporary handoff source.
- When a task reveals a missing local tool or runtime, finish the active task
  first, then repair the host baseline if it is safe and local. Prefer
  user-local, reversible installs and verify the command becomes available. If
  the repair needs administrator rights, account consent, payment, or another
  user-only step, record the exact install command and follow-up instead of
  silently dropping the lesson.

## Cross-Repository Updates

- Reusable lessons belong in `okmrtis/meta`; project-specific details belong in
  the project repository.
- When changing common behavior, update `okmrtis/meta` first, then refresh each
  project's vendored `docs/meta/AGENTS.common.md` snapshot.
- Do not store raw transcripts, secrets, customer data, or full local paths in
  `meta` unless they are deliberately sanitized examples.
