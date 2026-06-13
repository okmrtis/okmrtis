# Shared Codex Rules

Codex must read these shared rules before working in any adopted `okmrtis`
project. They apply unless a project-local `AGENTS.md` adds a stricter or more
specific rule.

## Git And Reproducibility

- Durable work should end as committed and pushed repository state, not as
  chat-only guidance.
- For any new non-test `okmrtis` project, GitHub/meta adoption is part of
  project creation, not a later cleanup task. Do not start normal project work
  until the project has a Git repository, root `AGENTS.md`,
  `docs/meta/AGENTS.common.md`, `docs/meta/meta_source.json`, and the
  meta-adoption check passes.
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
- If a project has README/workflow docs saying work is GitHub-managed but lacks
  `AGENTS.md` or `docs/meta/AGENTS.common.md`, treat that as a setup defect:
  pause normal work, adopt `okmrtis/meta`, run the adoption check, and only then
  continue.
- On this Windows Codex host, recheck current tooling before applying older
  workaround history. GitHub CLI, Python, npm/npx, Git LFS, PowerShell policy,
  and Git long-path behavior were refreshed on 2026-05-20; the current baseline
  lives in `okmrtis/meta` as cards under topic `codex-windows-tooling` in
  `knowledge/cards.jsonl`.
- On Windows Codex hosts, before substantive work, ensure the Codex
  goal/automation taskbar-badge auto-sync is bootstrapped. If
  `%CODEX_HOME%\bin\codex-goal-badge-watch.py` or the user Startup shortcut
  `CodexGoalBadgeMetaSync.lnk` is missing, locate or fetch a local
  `okmrtis/meta` checkout, then run
  `powershell.exe -NoProfile -ExecutionPolicy Bypass -File scripts\codex_goal_badge_fix\bootstrap-codex-goal-badge-auto-sync.ps1`
  from that checkout. This is user-local and reversible; proceed without asking
  unless GitHub auth, clone access, or Windows execution policy blocks it.
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
  recorded intermediate state instead of attempting one broad run. Run batches
  in parallel only when they are independent, do not touch the same files or
  external state, and have a bounded concurrency limit; otherwise run them
  sequentially.
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

## Execution Reliability Protocol

- Apply this protocol to every Codex task in adopted projects and on every
  device where `okmrtis/meta` is available. Scale the depth to task risk, but
  do not skip the protocol silently.
- Before execution, report the request interpretation, concrete requirements,
  material unknowns, working hypotheses, and the plan. Build the plan from an
  appropriate frame for the task, such as decomposition, a structured checklist,
  a logic tree, a decision matrix, a calculation, or a test matrix. For trivial
  one-command requests, this can be a compact sentence; for ambiguous or
  high-impact work, make the checklist explicit before editing or acting.
- During execution, test hypotheses against current authoritative state. When
  an interesting hypothesis, contradiction, or reusable lesson appears, inspect
  evidence and revise the plan instead of continuing from stale assumptions.
- After execution, verify against the original objective. Check for hallucinated
  facts or unsupported claims, run relevant tests or validators, inspect saved
  artifacts or runtime behavior, and confirm reproducibility from Git plus
  documented external inputs.
- Stabilize before declaring success: reduce avoidable flakiness, keep changes
  bounded, refactor only where it improves correctness or maintainability, and
  record residual risks or "not applicable" checks explicitly.
- Use an external evaluator when proportionate: automated tests, linters,
  schema validators, CI, browser screenshots, independent command output,
  subagent review, or human review after approval. Do not notify or involve
  other people without the user's approval.
- Define task-relevant monitoring or regression signals when the work will run
  again, such as pass/fail gates, runtime, coverage, candidate counts, error
  rates, freshness, drift, or artifact hashes.
- In user-facing updates and final responses, report the evidence produced by
  this protocol: plan, checks run, results, stability/reproducibility status,
  external-evaluation path, monitoring signals, objective alignment, and any
  remaining uncertainty. Do not expose hidden chain-of-thought; summarize
  verifiable reasoning outcomes and evidence.
- Treat completion as unproven until requirement-by-requirement evidence shows
  the requested end state is satisfied. If evidence is weak, indirect, missing,
  or contradicted, continue working or state the remaining gap.

## Evidence And Verification

- Do not trust script success alone when a task changes durable artifacts. Verify
  the actual saved output or pushed remote state.
- For generated documents, spreadsheets, images, or uploads, keep enough compact
  evidence to explain what was checked without committing bulky scratch data.
- Before changing Excel, Word, or PowerPoint artifacts, choose the tool path and
  final verification gate from `okmrtis/meta` topic `office-artifact-workflow`
  in `knowledge/cards.jsonl`; do this before the first write.
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
- Keep reusable shared knowledge as structured JSONL cards in `okmrtis/meta` at
  `knowledge/cards.jsonl`, with `knowledge/topics.json` as the topic map. Use
  GitHub for synchronization and history, but do not maintain Markdown knowledge
  files or generated JSON indexes as parallel sources of truth.
- In `okmrtis/meta`, Markdown is reserved for Codex instruction files
  (`AGENTS.md` and `shared/AGENTS.common.md`). Treat other shared knowledge as
  JSON or JSONL.
- When using live shared knowledge from `okmrtis/meta`, read
  `knowledge/topics.json` for topic choice, then filter `knowledge/cards.jsonl`
  by topic, section, summary, body, and signals. Apply only cards that fit the
  current project.
- When adding shared knowledge, make the scope, source date, and future action
  explicit. Use GitHub Issues for open tasks and backlogs, not as the primary
  store for stable lessons.
- When changing common behavior, update `okmrtis/meta` first, then refresh each
  project's vendored `docs/meta/AGENTS.common.md` snapshot.
- Do not store raw transcripts, secrets, customer data, or full local paths in
  `meta` unless they are deliberately sanitized examples.
