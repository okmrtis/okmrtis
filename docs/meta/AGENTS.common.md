# Shared Codex Rules

Codex must read these shared rules before working in any adopted `okmrtis`
project. They apply unless a project-local `AGENTS.md` adds a stricter or more
specific rule.

## Git And Reproducibility

- Durable work should end as committed and pushed repository state, not as
  chat-only guidance.
- Every repository under `okmrtis` is in scope for GitHub/meta adoption,
  including repositories created in the future. For any new `okmrtis` project,
  adoption is part of project creation, not a later cleanup task. Do not start
  normal project work until the project has a Git repository, root `AGENTS.md`,
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
- Treat non-main branch creation as a tracked decision, not disposable scratch.
  Before declaring repository or GitHub management done, check branch hygiene
  for the active repository or `okmrtis` scope when available, such as with
  `<meta-clone>\scripts\check_branch_hygiene.ps1`. Resolve or explicitly
  report remote branches without open PRs, branches already contained in the
  default branch, local branches without upstreams, and local branches ahead of
  upstream. Do not delete, force-push, or otherwise retire branches until the
  target branch, ownership, review path, and rollback posture are explicit.

## Local Project Rules

- Read this shared file, then the project-local `AGENTS.md`, README, and
  workflow docs before changing files.
- Treat `okmrtis/meta` as the normative source for Codex behavior in every
  Codex chat that involves `okmrtis` work. On each shared device, a local
  `okmrtis/meta` clone and workspace-level `AGENTS.md` entrypoint should be
  bootstrapped with `scripts/bootstrap_codex_meta_environment.ps1` before
  substantive work. If a chat starts outside a bootstrapped workspace or cannot
  read meta, restore the bootstrap or move the work under a bootstrapped
  workspace before relying on shared behavior.
- If a project has README/workflow docs saying work is GitHub-managed but lacks
  `AGENTS.md` or `docs/meta/AGENTS.common.md`, treat that as a setup defect:
  pause normal work, adopt `okmrtis/meta`, run the adoption check, and only then
  continue.
- On this Windows Codex host, recheck current tooling before applying older
  workaround history. GitHub CLI, Python, npm/npx, Git LFS, PowerShell policy,
  and Git long-path behavior were refreshed on 2026-05-20; the current baseline
  lives in `okmrtis/meta` as cards under topic `codex-windows-tooling` in
  `knowledge/cards.jsonl`.
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
- When the user explicitly delegates `okmrtis` repository maintenance authority,
  such as applying meta rules to every current and future `okmrtis` repository,
  treat that as approval to complete the necessary commits and pushes within
  the authorized `okmrtis` scope after verifying the target list, keeping changes
  reviewable, and reporting per-repository results. Do not stop merely because
  multiple `okmrtis` repositories are affected.
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

- Apply this protocol to every user Codex chat execution where `okmrtis/meta`
  is available, across projects, devices, current repositories, and future
  repositories. Scale the depth to task risk, but do not skip the protocol
  silently.
- Before execution, report the request interpretation, concrete requirements,
  material unknowns, working hypotheses, and the plan. Build the plan from an
  appropriate task standard or frame. If the task has a recognized standard
  planning or delivery method, follow it or explicitly adapt it before falling
  back to generic structures, such as Agile or waterfall for application
  development, WBS for task or project breakdown, decomposition, a structured
  checklist, a logic tree, a decision matrix, a calculation, a test matrix, or
  another explicit structure that fits the work. For trivial one-command
  requests, this can be a compact sentence; for ambiguous or high-impact work,
  make the checklist explicit before editing or acting. This is not a
  pause-for-approval gate by default: after reporting the interpretation,
  requirements, unknowns, hypotheses, and plan, choose the best supported
  working hypothesis, state any material assumptions, and proceed to execution
  unless a user-only decision, approval-required external impact, or unsafe
  ambiguity remains.
- During execution, test hypotheses against current authoritative state. When
  an interesting hypothesis, contradiction, or reusable lesson appears, inspect
  evidence and revise the plan instead of continuing from stale assumptions.
- After execution, verify against the original objective. Check for
  hallucinated facts or unsupported claims, run relevant tests or validators,
  inspect saved artifacts or runtime behavior, and confirm reproducibility from
  Git plus documented external inputs.
- Stabilize before declaring success: reduce avoidable flakiness, keep changes
  bounded, refactor where it improves correctness or maintainability, and
  record residual risks or "not applicable" checks explicitly.
- Use external evaluators when proportionate: automated tests, linters, schema
  validators, CI, browser screenshots, independent command output, subagent
  review, or human review after approval. Do not notify or involve other people
  without the user's approval.
- Define task-relevant monitoring or regression signals when the work will run
  again, such as pass/fail gates, runtime, coverage, candidate counts, error
  rates, freshness, drift, or artifact hashes.
- Perform layered meta-verification until confidence saturates for the task:
  check whether the plan fits the requirements, whether the evidence actually
  supports the conclusion, whether tests cover the risky paths, whether
  remaining uncertainty is explicit, and whether another reasonable evaluator
  would likely reach the same result. Extra time spent on this reliability loop
  is acceptable when it materially increases confidence.
- In user-facing updates and final responses, report the evidence produced by
  this protocol: request interpretation, requirements, unknowns, hypotheses,
  plan, checks run, results, stability/reproducibility status,
  external-evaluation path, monitoring signals, objective alignment,
  meta-verification confidence, and any remaining uncertainty. Do not expose
  hidden chain-of-thought; summarize verifiable reasoning outcomes and evidence.
- For every task, translate results into the user's decision language before
  presenting technical detail. Start from what the user should worry about,
  decide, change, approve, ignore, or review next. When reporting findings,
  separate real concerns from review candidates and non-concerns, and attach
  impact, affected scope, and recommended action instead of leaving the user to
  infer meaning from raw diffs, sheet names, warnings, logs, or counts.
- Treat completion as unproven until requirement-by-requirement evidence shows
  the requested end state is satisfied. If evidence is weak, indirect, missing,
  or contradicted, continue working or state the remaining gap.

## User-Facing Output

- For Japanese-speaking users, use Japanese by default in user-facing
  responses, summaries, reports, and deliverable text unless the user asks for
  another language, the artifact has a required language, or preserving source
  wording is necessary.
- Human-facing deliverables should present a readable review surface first. Do
  not make raw CSV, JSON, logs, hashes, or full diffs the primary handoff when
  Markdown, HTML, a formatted Excel workbook, or another readable artifact is
  more appropriate; keep machine evidence as supporting material.
- Keep submitted files few and purpose-specific. When the same content can be
  delivered in one suitable format, produce one format instead of parallel
  Markdown, HTML, Excel, CSV, or log copies. Create multiple formats only when
  the user requests them or each format has a distinct review, execution, or
  archival purpose.

## Evidence And Verification

- Do not trust script success alone when a task changes durable artifacts. Verify
  the actual saved output or pushed remote state.
- For generated documents, spreadsheets, images, or uploads, keep enough compact
  evidence to explain what was checked without committing bulky scratch data.
- For visual design deliverables or design-sensitive edits, including slides,
  document pages, dashboards, charts, UI, diagrams, posters, thumbnails, and
  images, read and apply the `visual-design` topic in `knowledge/cards.jsonl`
  before the first write. Verify the final render with a four-principles pass:
  proximity, alignment, repetition, and contrast.
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
- When a task reveals a missing, stale, or weaker local tool/runtime, prefer a
  beneficial environment update when it is user-local, reversible, and likely
  to improve capability, reliability, verification quality, or future
  reproducibility. Run or rerun the relevant environment checker after the
  update. If the update is not needed for the active task, finish the task
  first when that is safer; if the update materially improves the active task,
  update before continuing. If the repair needs administrator rights, account
  consent, payment, or another user-only step, record the exact install command
  and follow-up instead of silently dropping the lesson.

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
- When changing common behavior, update `okmrtis/meta` first, then refresh every
  current `okmrtis` repository's vendored `docs/meta/AGENTS.common.md` snapshot.
  Future repositories under `okmrtis` must receive the same snapshot during
  creation. Use `scripts/sync_all_okmrtis_meta_adoption.ps1 -CheckOnly` to
  audit current GitHub repositories, then rerun it with `-Apply`, `-Commit`, or
  `-Push` when intentionally refreshing adopted repositories.
- Do not store raw transcripts, secrets, customer data, or full local paths in
  `meta` unless they are deliberately sanitized examples.
