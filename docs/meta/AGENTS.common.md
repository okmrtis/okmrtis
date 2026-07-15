# Shared Codex Rules

Codex must read these shared rules before working in any adopted `okmrtis`
project. They apply unless a project-local `AGENTS.md` adds a stricter or more
specific rule.

## Git And Reproducibility

- Durable work should end as committed and pushed repository state, not as
  chat-only guidance.
- For `okmrtis` repositories, a user request that creates or updates durable
  repository state normally authorizes Codex to commit and push the verified
  result to the repository's ordinary remote branch. Do not stop at a local
  commit unless the user explicitly asks for local-only work, the push is
  blocked by authentication or branch divergence, or the candidate push would
  include secrets, unrelated user work, generated scratch, or another unsafe
  change. If Codex leaves a durable commit unpushed, report that as a blocker
  with the exact reason.
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
  Immediately after creating a branch and after material head changes, record
  its repository, branch, full head/base object IDs, and current Codex task ID
  in the local branch-integration provenance ledger with the installed
  `record_branch_provenance.ps1`. Store pointers only: never copy raw chat text,
  hidden reasoning, credentials, or private exports into the ledger or GitHub.
  A hexadecimal string is not provenance proof: the recorder must resolve the
  supplied head and base as readable commit objects in the named worktree,
  require the head to equal the current local branch ref, and append nothing
  when object or ref validation fails.
  Before declaring repository or GitHub management done, check branch hygiene
  for the active repository or `okmrtis` scope when available, such as with
  `<meta-clone>\scripts\check_branch_hygiene.ps1`. Resolve or explicitly
  report remote branches without open PRs, branches already contained in the
  default branch, local branches without upstreams, and local branches ahead of
  upstream. Do not delete, force-push, or otherwise retire branches until the
  target branch, ownership, review path, and rollback posture are explicit.
- Use the installed `okmrtis` branch-integration controller as the deterministic
  cross-repo safety layer when it is available. It must identify repositories by
  GitHub `owner/name`, not by whichever duplicate local clone happens to be
  found. It may open or update PRs, merge with expected-head guards, and retire
  contained refs after cooldown. It must still fail closed for conflicts,
  draft or do-not-merge signals, changed heads, failed or pending checks,
  requested changes, sensitive or high-risk paths, oversized diffs, uncertain
  API state, and untested code. A ready PR intentionally handed to that lane
  must include `<!-- okmrtis-branch-integration:v1 -->`; absence routes it to
  semantic convergence instead of implying deterministic merge consent. Never
  force-update branch content. A contained ref may be deleted only with an
  expected-head lease and a recovery SHA recorded first.
- A deterministic `waiting`, `review_needed`, or `error` result is not a terminal
  hold. The scheduled Codex semantic branch-convergence automation must consume
  every such result and every local or remote non-default branch. Only an active
  task, changed head, cooldown/post-mutation interval, or named external retry
  may be monitored, and every monitoring item needs an exact recheck time.
- Before deterministic create/update/merge/delete, run an audit and correlate the
  exact head with its provenance and current Codex task. Grant a short-lived
  exact-head `clear` preflight only when the owning task is inactive or complete
  and no matching worktree contains uncommitted or unpushed intent. Record an
  `active_task` lease otherwise. Missing, expired, active, or wrong-head
  preflight must prevent mutation and produce bounded monitoring.
- For semantic convergence, inspect evidence in this order: matching Codex task
  history; GitHub Issue/PR timeline, reviews, and checks; commit, diff, reflog,
  worktree, and timestamp chronology relative to current `main`; current rules
  and regression tests; then prior integration/recovery records. Correlate at
  least two independent identifiers before assigning task ownership. Store only
  task IDs, hashes, and privacy-safe summaries, never raw chat transcripts,
  hidden reasoning, credentials, or full machine paths.
- Semantic convergence must preserve the newest verified repository invariants
  and integrate the unique intent of older work. Do not blindly merge an obsolete
  implementation merely to remove a branch. Port still-useful behavior and tests
  onto current `main`, resolve conflicts by the reconstructed intent and timeline,
  add or repair tests, rerun risk-matched checks, and keep iterating until the
  result is verified. A failing change is work to repair or safely supersede, not
  permission to merge a regression and not a reason to abandon the branch.
- An actively changing branch is owned work, not an abandoned hold. Continue or
  message its owning Codex task with an explicit completion instruction; if the
  task becomes idle or loses ownership, take over in an isolated worktree. The
  only terminal outcomes are `merged_semantically`,
  `semantic_noop_obsolete_retired`, or
  `declared_long_lived_with_authoritative_evidence`. A long-lived declaration
  must cite the current default-head authority and expire for revalidation.
  Any unresolved external dependency remains an active retry item with a next
  action and owning task, and must be revisited automatically rather than parked.
- Branch automation complements task completion. At the end of branch-based work,
  commit and push durable work, converge it into `main`, close or merge its PR,
  and retire the ref after recording recovery evidence. Keep
  `delete_branch_on_merge` enabled where GitHub supports it. A ready PR handed to
  the deterministic controller may retain the exact body marker
  `<!-- okmrtis-branch-integration:v1 -->`, but absence of that marker routes the
  work to semantic convergence instead of becoming a permanent hold.

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

## Browser And Desktop Interaction

- For browser work, unless the user explicitly requests another surface, first
  discover and try the Codex in-app browser (`iab`). Keep browser activity
  inside Codex and in the background so it does not steal focus from the user's
  desktop work. Apparent capability gaps or a presumed need for an existing
  external-browser profile are not reasons to skip this first attempt.
- If the in-app browser disconnects or loses its tab, attempt a bounded recovery
  on the same `iab` backend, such as reconnecting and opening a new in-app tab,
  before changing tools. Do not treat one detached tab or timeout as proof that
  the in-app browser is unavailable.
- If `iab` appears to lack a required capability or authenticated state, first
  attempt bounded, user-local, reversible improvements on the in-app path. As
  applicable, inspect its current tool documentation and available backends,
  reconnect or recreate the in-app tab, complete supported authentication in
  `iab`, and install, enable, update, or repair a relevant Codex browser plugin,
  extension, or connector. Re-test the required workflow in `iab` after the
  improvement. Do not use a capability gap or external-profile requirement as
  an escalation reason until these applicable in-app improvements have been
  attempted or a concrete blocker makes them unavailable.
- Escalate to another authorized browser or desktop-control surface only when
  the in-app browser is undiscoverable, remains unavailable after bounded
  recovery and applicable improvement attempts, or those improvements require
  a user-only approval, administrator privilege, unsupported authentication, or
  another concrete blocker. Record the attempted in-app repairs and the exact
  remaining reason for escalation. Then choose the least disruptive suitable
  option, such as a purpose-built connector or API, a browser extension, or
  Computer Use. Launching, foregrounding, focusing, or switching a normal
  Edge/Chrome window is a last resort because it interrupts the user's work.

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
  Normal fast-forward `git push` of verified commits inside an authorized
  `okmrtis` repository is governed by the Git and Reproducibility rules above,
  not by this people-notification stop point. Force-pushes, deletions, public
  releases, deployment promotions, or pushes containing unclear/unrelated
  changes still require the relevant explicit approval or rollback path.

## Execution Reliability Protocol

- Apply the following eight gates to every user Codex task where `okmrtis/meta`
  is available, across projects, devices, current repositories, and future
  repositories. Scale depth to risk, but classify every gate as `complete`,
  `not applicable` with a reason, or `blocked` with the missing evidence.
- Gate 1, scope: before acting, report the request interpretation, concrete
  requirements, material unknowns, constraints, and the objective-level done
  criteria. Distinguish user facts, authoritative evidence, assumptions,
  hypotheses, and unresolved claims.
- Gate 2, plan: select and name an appropriate task standard or structure. Use
  a recognized delivery method when one fits, such as Agile or waterfall for
  application delivery, WBS for project decomposition, a logic tree, decision
  matrix, calculation, test matrix, phase gates, or another explicit model.
  State the working hypothesis and the evidence that would confirm or reject it.
  This report is not an approval pause by default; proceed unless a user-only
  decision, approval-required external impact, or unsafe ambiguity remains.
- Gate 3, execution: work from authoritative current state, keep changes bounded,
  and update the plan when a contradiction, interesting hypothesis, or reusable
  lesson appears. Do not continue from an assumption after evidence invalidates
  it.
- Gate 4, truth and tests: check every material factual claim for source, test,
  or direct observation; label inference and uncertainty. Run risk-matched tests,
  validators, render checks, saved-artifact reads, runtime probes, and negative
  cases. A successful command is evidence only for the behavior it actually
  covers.
- For exact Codex Desktop app tools, distinguish four facts: installed app
  capability, the current task's persisted thread-start dynamic-tool snapshot,
  a bounded recent cohort of non-automation task starts, and any explicit live
  callable inventory. A resumed turn cannot replace thread-start dynamic tools.
  Within the recent cohort, parse `source.subagent.thread_spawn.depth` and
  report primary provisioning (user tasks plus first-level subagents) separately
  from nested-subagent provisioning (depth 2 or greater). A latest nested miss
  remains a nested reliability warning, but it does not by itself prove global
  user-task failure or justify a Codex Desktop restart. Retained history contains
  both successful and missing nested starts, so do not classify every nested
  absence as intentional minimization either. When nested app tools matter,
  retry one nested start after host load subsides; otherwise continue from a user
  or first-level task while preserving the nested warning.
  If the current task lacks a tool but recent task creation is healthy, classify
  that task as immutable missing state and use a new or forked task when the
  exact tool is required. Exclude intentionally minimized automation tasks from
  the normal provisioning denominator. Treat a missing start followed by later
  successful same-version starts as transient registration evidence, not global
  absence. PATH, TOML, SQLite, full-access, approval-policy, Startup, or helper
  edits cannot inject an app tool into an existing task.
- Gate 5, stability and reproducibility: reduce avoidable flakiness, refactor
  where it improves correctness or maintainability, verify rerun behavior, and
  record the exact Git state plus authorized external inputs needed to reproduce
  the result.
- Gate 6, independent evaluation: use at least one proportionate evaluator that
  is independent of the implementation path, such as tests, linters, schema
  validators, CI, browser screenshots, independent commands, subagent review,
  or human review after approval. Do not involve other people without approval.
- Gate 7, monitoring and alignment: define task-relevant regression signals for
  recurring work, such as pass/fail gates, runtime, coverage, counts, error rate,
  freshness, drift, hashes, or worker health. Recompare the result with the
  original objective and identify work that is useful but out of scope.
- Gate 8, meta-verification and reporting: audit the requirement-to-evidence
  mapping, test coverage of risky paths, evaluator independence, remaining
  uncertainty, and whether another reasonable evaluator would reach the same
  conclusion. Confidence is saturated only when independent evidence agrees,
  no material requirement or high-risk path lacks evidence, and another check
  has low expected information gain. Report verifiable outcomes and evidence,
  not hidden chain-of-thought.
- User-facing updates and final responses must expose the protocol results:
  interpretation, requirements, unknowns, hypotheses, plan, execution result,
  factual checks, tests, stability and reproducibility, refactoring decision,
  external evaluation, monitoring signals, objective alignment, confidence,
  and residual uncertainty.
- Keep the local automation that supports this protocol under the component
  ownership defined in `scripts/codex_reliability/manifest.json`. Use
  `scripts/codex_reliability/install-codex-reliability-control-plane.ps1` as the
  unified install and health-check entrypoint. Do not add another Startup item
  or scheduler for an existing component without first updating that manifest
  and proving that the existing owner cannot carry the responsibility.
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
- Before creating any customer-facing system implementation proposal, read and
  apply all four canonical cards from `knowledge/cards.jsonl`:
  `project-delivery-methods/customer-system-proposal-phase-gates`,
  `project-delivery-methods/japanese-system-proposal-standard-structure`,
  `project-delivery-methods/pmo-management-system`, and
  `visual-design/imagegen-consulting-body-eight-pattern-contract`. Enforce the
  six independent gates, canonical 25-module decision structure, exact
  Title/Key Message/Body semantics, eight-form-only ImageGen body contract, and
  full restart from Phase 1 after any rejection. Do not begin slide production
  while an upstream gate is unpassed.
- For the Phase 5 proposal BODY, use the committed asset with stable ID
  `customer-system-proposal/body-structure-reference/v1` at
  `assets/customer-system-proposal/body-structure-reference-v1.png` as the sole
  body-structure reference. Its immutable identity is SHA-256
  `1214c21578411dfb22885c3e9d1041640a0a78b54797b5761761273aef3122ca`,
  MIME `image/png`, 4032 x 2283 pixels, 12,958,793 bytes, preserved byte for
  byte from the user-supplied chat attachment. Additional or alternate
  body-structure references are a Gate 1 rejection.
- BODY production must use a Japanese BCG-like customer-proposal tone as a
  non-proprietary benchmark: claim-led, analytical, flat and precise, minimally
  ornamental, with rigorous alignment and evidence hierarchy. Do not copy
  proprietary BCG or other consulting template assets. Use only the eight named
  patterns or their combinations; every combination needs one dominant
  relationship and an unambiguous scan order.
- All non-color visual rules in this BODY contract are fixed and non-variable;
  only color choice may vary, including exact color values. Keep low-to-high
  importance ordered as pale/dark gray, pale/dark green, then pale/dark orange,
  with default dark guidance spanning approximately green `#134611` to orange
  `#C57B57`. Reversing the order or making layout, pattern, hierarchy, scan
  order, font, size floor, tone, or sole-reference identity variable rejects
  Gate 1.
- The size hierarchy is a separate fixed semantic hierarchy, independent of color: low-importance objects and text are smaller, while high-importance objects and text are larger.
  Object and text size move together in that fixed
  direction; flattening, reversing, decoupling, or making it variable rejects
  Gate 1. The Meiryo BODY text floor remains 12 pt at final slide size.
- BODY must remain one ImageGen-generated raster. Initial construction and repair must not use manually assembled PowerPoint shapes or tables, hand-typed or code-drawn labels, white patches, or overlays; correct defects only by image editing or regeneration.
- At Phase 1, keep an evidence ledger for every sampled source with publisher,
  formal title, URL, page or section, retrieval date, scope, and limitation; a
  search-result snippet or an unreachable URL is not a usable sample. Organize
  the resulting standard in three explicit layers: the system-delivery core
  (purpose through handover and TCO), the public-PoC/procurement overlay
  (scoring, agreement, reports, rights, payment, publicity, and non-guaranteed
  continuation), and the domain overlay (law/standards, expert rubric,
  inclusion/ethics, validity/KPI, and field constraints). A module may be
  marked not applicable only with a written reason and an alternative control;
  never silently delete migration, testing/acceptance, training/adoption,
  go-live/rollback, operations/handover, data exit, or lifecycle cost.
- Phase 1 sampling must include at least five publicly inspectable Japanese
  customer-facing system implementation proposal artifacts, or clearly labeled
  proposal templates that preserve the customer-facing page structure. Do not
  count RFPs, procurement instructions, evaluation sheets, or generic how-to
  articles as those five proposal artifacts; use them as separate requirement,
  evaluation, or delivery-standard evidence. Record whether each artifact is an
  actual submitted proposal, a disclosed exemplar, or a template, and state the
  limitation before deriving the standard architecture.
- Trace every canonical proposal module and every conditional overlay claim
  back to at least one source ID and an exact page or section; an uncited
  label or cover module is not an exception. Each citation must carry the
  smallest supporting locator and a short exact evidence phrase that can be
  re-extracted from that locator. A source-wide range copied across unrelated
  claims is not claim-level evidence. Unsupported claims about
  agreements, reports, acceptance/payment, rights, publicity, continuation,
  ethics, or domain controls are a Gate 1 rejection even when the overall
  structure appears reasonable.
- Preserve each source's exact visible formal title and exact heading byte for
  byte after ordinary HTML entity decoding, including full-width characters,
  numbering, and punctuation; keep shortened aliases and contextual project
  names in separate fields. Re-extract titles, headings, and cited evidence
  phrases from the frozen raw source during validation rather than only
  comparing two derived artifacts. Declare one page convention,
  normally one-based PDF file pages, and use it identically in the human
  ledger, manifest, module crosswalk, and overlay crosswalk. Reject or narrow a
  claim when the cited page does not contain it.
- Proposal Gate 1 negative tests must cover a normalized-but-not-exact formal
  title, a shortened or renamed heading, a zero-citation canonical module, a
  broad locator reused across unrelated claims, and an evidence phrase absent
  from the declared page or section while dependent hashes are updated. A
  validator that accepts any of these semantic drifts fails closed only in
  appearance and cannot authorize the next phase.
- Freeze customer requirement identifiers and meanings when Phase 2 passes.
  Downstream phases and restarted runs must preserve those identifiers; split a
  requirement only with child identifiers such as `R06-1`, append newly found
  requirements at the end, and keep version/change history. Validate the
  upstream/downstream ID sets mechanically. Reusing an old identifier for a new
  meaning, silently losing a requirement, or changing its interpretation
  without a recorded version and impact analysis is a phase-gate rejection.
- Phase 2 evidence controls must fail closed on meaning, not merely on IDs,
  hashes, counts, or favorable substrings. Represent every mandatory customer
  fact with canonical structured semantics and an exact source-ID set; reject
  negation, reversed rights or payment direction, changed actors, and weakened
  conditions. Re-extract live forms deterministically from their frozen raw
  source and compare the entire extracted schema. For conditional fields,
  verify the exact child, parent, triggering choice/key, required state, and
  limit. Recompute draft-versus-live differences from the original workbook or
  form instead of trusting a handwritten difference table. Negative tests must
  mutate each critical fact direction, source attribution, branch connection,
  attachment restriction, and raw-derived difference while updating dependent
  hashes, so synchronized tampering is rejected as well as stale hashes.
- Apply the Phase 2 canonical comparison to every registered customer fact, not
  only a hand-picked critical subset. For each fact, exact-compare subject,
  predicate, polarity, value, unit, conditions/qualifiers, the order-independent
  allowed source-ID set, and exact locators. A correct value attributed to the
  wrong official source is a rejection. Mutation coverage must include every
  fact ID across meaning/value, source set, and condition/qualifier classes.
- A live-form validator may claim full re-extraction only when it defines one
  canonical projection and proves a bijection between frozen raw elements and
  derived schema elements. Require unique stable IDs on both sides, consume
  every raw and schema row exactly once in both directions, and exact-compare
  all projected attributes, including name, label, type, required, parent,
  branch keys and choices, min/max, placeholder, default, description, outline,
  upload restrictions, page, and ordinal. Equal counts do not prove completeness;
  duplicate-one/drop-another mutations must fail.
- Make customer requirements, scoring criteria, and application fields a single
  typed traceability graph. Freeze exact CR, SC, and FM ID sets plus the exact
  CR-to-SC and SC-to-FM edge sets, with purpose and required proof on every edge.
  Reject unknown, missing, duplicated, or semantically reassigned edges even when
  all endpoint IDs exist. Store F/A/H/U as real records, not only class
  definitions: facts need direct sources, assumptions need an owner and confirm
  point, hypotheses need KPI/comparison/pass criteria, and unconfirmed items need
  a question and explicit no-assertion boundary.
- A cross-phase proposal contract is incomplete until an executable payload
  fixture proves the contract against real record shapes. Mutation tests must
  change fact values and source sets, raw and annotated form rows and attributes,
  graph edges, class records, and transitions themselves; changing only registry
  declarations or booleans does not close the downstream defect.
- Keep proposal claim-evidence classes and cross-phase record kinds in separate
  namespaces. Use `F/E/A/P/V/U` only for the epistemic status of a proposal claim;
  use explicit record kinds such as `OFFICIAL_FACT`, `PROPOSER_ASSUMPTION`,
  `PILOT_HYPOTHESIS`, and `UNCONFIRMED` for cross-phase data. Never overload `A`
  to mean both analysis and assumption, or silently map a hypothesis to evidence.
- Keep technical maturity and rights provenance as two separate axes in every
  proposal. Record maturity from unstarted/design/prototype/similar-project
  implementation/target-environment validation/operation, and record rights as
  self-existing, self-current-development, joint result, OSS, third-party
  license, or future-IP candidate. Do not convert an OSS component's maturity
  into a claim that the proposal integration is mature, and do not call an
  unvalidated design proprietary proof.
- For proposal Gate 1 reproducibility, separate the immutable meta evidence
  floor from a vendored meta snapshot that may advance. Record both commits and
  the required proposal-card IDs. Do not bind a validator permanently to exact
  equality with a floating meta pointer; accept only the same commit or a
  verified descendant that retains the required contracts, and evaluate the
  candidate commit separately from later worktree drift. Run the validator
  under both Windows PowerShell 5.1 and PowerShell 7 with equivalent counts,
  hashes, ancestry result, and final outcome. Frozen downloaded source bytes
  may be reused only after re-hashing and re-reading them as evidence; do not
  reuse old conclusions, phase decisions, slide bodies, or proposal text
  without regeneration or explicit re-evaluation in the new run.
- A complete Phase 1 source run must iterate the complete frozen source
  registry, not only sources reached by citations. After candidate freeze, open
  and read every registered source, recompute its raw-byte SHA-256 and size,
  record its media/source type, and run the deterministic type-appropriate
  extraction or record explicit binary/visual extraction evidence. Emit one
  per-source read/extraction result. A manifest hash, aggregate source count, or
  citation traversal cannot prove that an unreferenced registered source was
  read. Add mutations for an omitted registered source and a falsely claimed
  read/extraction result.
- Gate 1 validators must name the candidate Git commit and expected SHA-256
  values, compare scoped worktree artifacts with that candidate separately,
  enforce declared cache-root containment, and reject duplicate source IDs,
  canonical paths, or URLs. Negative tests must cover missing evidence,
  same-size byte modification, candidate drift, path traversal, nonexistent
  meta commits, reverted rules, and a valid later meta descendant.
- Compare raw binary worktree bytes for every scoped artifact with the exact
  candidate Git blob, including both byte count and SHA-256, using a path that
  bypasses Git text and end-of-line filters. A clean Git status is not byte
  equality. Normalize intended line endings before candidate freeze and reject
  LF/CRLF or other filter-hidden worktree drift with a dedicated mutation.
- Treat an HTML citation locator as a structural identity, not as displayed
  heading text. Record the exact decoded text together with the element tag,
  ordered occurrence, raw line or byte range, and deterministic DOM path; the
  frozen raw source must resolve that identity to exactly one element. Preserve
  duplicate-text elements as an ordered collection and reject ambiguity. A
  text-keyed map, silent overwrite, or last-write-wins parser cannot authorize
  Gate 1. Negative tests must add a duplicate same-text heading while updating
  dependent hashes and prove that the citation becomes ambiguous.
- Bind every Gate 1 decision to an immutable, non-circular candidate receipt.
  The receipt must name the frozen candidate path, exact candidate byte hash,
  candidate Git commit, scoped artifact hashes, and applicable meta references;
  the evaluated candidate must not try to contain its own hash. Validators and
  dual-host wrappers must receive the candidate path and expected candidate
  hash or receipt explicitly and must reject a mutable working-manifest
  substitute, worktree drift, a wrong candidate, or a receipt mismatch. Include
  positive coverage for an allowed descendant meta commit as well as negative
  coverage for candidate/worktree drift.
- Keep repeated proposal-gate runs lightweight and reproducible: cache frozen
  raw source bytes by content hash, re-hash and re-read them in each restarted
  run, execute focused mutation tests while iterating, and reserve one complete
  normal validation pass for the frozen gate candidate. Bound concurrency and
  avoid agent or browser fan-out when the host application is unstable. This
  permits transport reuse only; conclusions, gate decisions, proposal text,
  and slide bodies must still be regenerated or explicitly re-evaluated.
- Make the five-sample entrance test machine-readable. Each counted artifact
  must be classified as an actual submitted customer proposal, a disclosed
  customer-facing exemplar, or a proposal template that preserves the page
  structure delivered to a customer; attach the exact page or section proving
  that classification and state its limitation. RFPs, proposal instructions,
  evaluation sheets, procurement notices, and generic articles may support
  separate requirement or practice claims but never satisfy the five counted
  artifacts. Fewer than five eligible artifacts, including zero, is an
  automatic Gate 1 rejection.
- Decompose every canonical module and overlay claim into atomic claim elements
  and validate element-level coverage. Every element must have at least one
  direct raw-source citation or an explicitly identified normative meta
  citation. A qualified-partial citation may supplement a claim but cannot be
  the sole support for an uncovered element. Reject claims whose own
  `support_boundary` admits that RACI, Go/No-Go, exceptions, adoption checks,
  alternatives, owners/actions, data exit, or another stated element remains
  unsupported. Add zero-direct, uncovered-element, and weakened-boundary
  mutations.
- Atomicity is semantic, not punctuation-based. Manually curate each stable
  element as a complete, independently evaluable semantic requirement; do not
  split target lines mechanically by comma, conjunction, slash, or regex. Each
  element must retain the actor, object, action or prohibition, direction,
  condition, and lifecycle scope needed for its meaning. Grammatical fragments
  such as `decision`, `prohibit`, `object`, `repair`, or a dependent prepositional
  phrase are invalid elements and must not count toward coverage.
- Do not treat a target or module label as element-level normative evidence.
  A normative citation may support several atomic elements only when its exact
  target line explicitly names every supported semantic. Bind every element ID
  to element-specific semantic fingerprints and fail closed when the cited line
  omits any fingerprint. In particular, `workflow` alone does not prove an
  approval or exception path; `adoption` alone does not prove an adoption
  measure or remediation; `go/no-go` alone does not prove the No-Go action or
  decision owner; `risk` alone does not prove trigger, prevention, response,
  and owner; and `agreement` alone does not prove change control, inspection,
  acceptance, payment, or exit transfer. Keep the exact reusable target lines
  in the canonical proposal phase-gate card and validate them by target ID.
- Model every declared bidirectional proposal relation as one canonical typed
  edge set. This includes form, scoring, requirement, module, evidence, and
  control relations. Store source type/ID, relation type, target type/ID,
  provenance, and a short justification once; generate all forward and reverse
  views from those edges instead of maintaining independent arrays. Reject
  duplicate edges, unknown endpoints, orphans, and any forward/reverse pair
  whose exact edge set is not equal. Mutation tests must add, delete, and rewire
  one side while updating dependent hashes and prove that the mismatch fails.
  A derived aggregate fact must cite a reproducible derivation locator with the
  extractor/query version, exact input set or input hash, formula, and output;
  pointing at one member record does not support an aggregate count.
- Make unresolved proposal gates structurally stoppable. Every unresolved fact,
  assumption, eligibility item, score-critical claim, staffing commitment, or
  submission dependency must have a stable ID, severity, owner, due time,
  required deliverable, evidence IDs, acceptance criteria, fallback or scope
  reduction, impacted IDs, and an explicit `stop_if_unresolved` boolean.
  Mandatory eligibility, submission, privacy, safety, and named-team claims
  must stop submission when unresolved unless the affected scope is removed in
  a customer-valid way. A future confirmation, generic TODO, or narrative owner
  is not a gate. Validate required fields and state transitions, and add missing
  owner, expired due time, weakened acceptance, false-stop, and silent-removal
  mutations. Receipts must distinguish valid controls from adversarial
  mutations and report both counts without calling controls mutations.
- A frozen proposal standard must be self-identical. Mechanically compare its
  declared version, file name/self-reference, predecessor version/run, required
  meta-card count and IDs, and all manifest/sidecar bindings. Do not create the
  next standard by blind global string replacement; regenerate its front
  matter and provenance contract from structured inputs. Any internal version,
  predecessor, file-name, or card-count contradiction rejects Gate 1.
- Separate immutable candidate evidence from post-candidate execution evidence
  without pretending either can contain its own future hash. Commit the full
  mutation suite and dual-host wrapper before the candidate commit and include
  them in the candidate receipt scope. After executing them, commit the exact
  commands, host/runtime identities, exit codes, normalized outputs, case list,
  counts, control hashes, and final normal-pass result, then bind those results
  with a second non-circular gate-evidence receipt and commit. The evaluator
  must verify both receipts. The mutation matrix must cover missing evidence,
  same-size byte drift, path traversal, nonexistent meta commit, rules/card
  revert and decoy, exact title/heading drift, zero citation/direct support,
  broad locator reuse, absent evidence phrase, duplicate same-text headings,
  candidate path/SHA/worktree drift, wrapper wrong-candidate input, and a valid
  descendant meta commit that retains all required contracts.
- Every required mutation above must have a stable runnable case ID in the
  checked-in suite and exact post-candidate execution evidence. Citation and
  locator adversaries must mutate the frozen raw source plus all dependent
  hashes and be rejected by deterministic semantic re-extraction, rather than
  only by equality with a generated bundle snapshot. Missing or unexecuted
  required cases reject Gate 1 even when an aggregate mutation count is high.
- Prove meta lineage with a real or isolated clone of the recorded meta
  repository. Fail closed when it is unavailable. Confirm that the declared
  commit exists and descends from an evidence floor that already contains the
  current reproducibility controls; compare its shared-rules Git blob with the
  project's vendored blob; and inspect its canonical card bodies for required
  semantic fingerprints. A 40-character string or ancestry alone is not proof
  that the vendored rules came from that commit or retained their meaning.
- Make the Gate 1 ledger and citation crosswalk machine-readable. Validate
  exact expected ID sets, uniqueness, full source coverage, nonempty exact
  locations, source-to-manifest membership, and correspondence between every
  sidecar citation and its human-readable row. Row counts and whole-document
  string searches are insufficient. Parse canonical meta cards by exact ID and
  check each card's own body fingerprints so decoy cards cannot mask a revert.
- Validate candidate and worktree `meta_source.source_repository` and
  `common_rules_path` against the manifest and actual clone. Normalize
  equivalent GitHub HTTPS/SSH identities, but require the declared meta commit
  to be reachable from the authoritative remote ref. Cite explicit user/meta
  normative controls separately from public source evidence rather than
  overstating a public source.
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
- Whenever Codex writes a new durable knowledge record or materially updates an
  existing one in any `okmrtis` repository, add machine-readable environment
  provenance in the record's native metadata. Record the writing environment
  name, classify dependence as `dependent`, `partially-dependent`,
  `independent`, or `unknown`, record confidence as `high`, `medium`, or `low`,
  and add a short basis. Reassess these values on each material update; do not
  silently carry forward legacy `unknown` metadata. For shared cards, use the
  exact `environment` object required by `knowledge/schema.json`. For
  project-local records, use an equivalent metadata object or front matter.
  Never fabricate an original environment during backfill, and do not apply a
  dependent or partially dependent lesson to another environment without a
  current compatibility check.
- When changing common behavior, update `okmrtis/meta` first, then refresh every
  current `okmrtis` repository's vendored `docs/meta/AGENTS.common.md` snapshot.
  Future repositories under `okmrtis` must receive the same snapshot during
  creation. Use `scripts/sync_all_okmrtis_meta_adoption.ps1 -CheckOnly` to
  audit current GitHub repositories, then rerun it with `-Apply`, `-Commit`, or
  `-Push` when intentionally refreshing adopted repositories.
- Do not store raw transcripts, secrets, customer data, or full local paths in
  `meta` unless they are deliberately sanitized examples.
## Independent Proposal Oracle

- For customer-system proposal phase gates, a cross-phase oracle must be generated by a separate deterministic builder from the frozen official/raw source set. The builder must not import, read, clone, or structured-clone the candidate fixture or downstream proposal payload at runtime.
- Pin the oracle bytes, builder bytes, and source raw hashes in the candidate manifest and receipt. Regenerate the oracle at least twice and require byte identity; compare the normalized candidate payload to the independent oracle before any downstream phase.
- Coordinated mutations that change the fixture and candidate payload together must still be rejected by the pinned oracle SHA and builder output. Treat an oracle derived from the candidate fixture as a Gate 1 rejection even when ordinary validation and mutation counts pass.
## Customer-Facing Japanese Proposal Language

- Customer-facing proposal slides must use ordinary Japanese business language. Do not leave abbreviations, technical terms, coined expressions, or compressed noun strings unexplained; write the actor, action, timing, and benefit in plain Japanese or show the relationship in a diagram.
- Keep Title, Key Message, and Body distinct: Title states the question or topic, Key Message answers it, and Body proves or supplements it with relationships, process, comparison, numbers, or roles. Do not repeat the Key Message in a bottom conclusion box; use the Body to show the evidence and flow.
- For body visuals, prefer readable diagrams over slogan lists. Show who does what, when, what is recorded, and what changes. If a specialized term is necessary, attach a plain-language explanation directly in the same visual.
- When a proposal uses cloud hosting for this HANAMII work, default to Sakura domestic hosting and describe the data boundary, security review, and fallback in plain Japanese. Do not imply that a cloud choice is approved until the customer security review is complete.

## Customer Proposal Story and Decision Context

- Write for a first-time customer reader who knows only the competition brief, not the proposal-development history. Label the current state, the proposed future work, and the evidence to be verified explicitly; never require the reader to decode private shorthand such as `A/B`, an unexplained acronym, or a coined label.
- Give every slide a distinct decision job and one non-overlapping question. Proposal overview, future work, feature detail, differentiation, workflow, architecture, verification, cost, organization, and decision must each add new evidence at a deeper level. Reject a deck when several slides restate the same mechanism without advancing one connected story.
- A cost page must map each amount to concrete work and deliverables. An organization page must name the company and customer roles, state who creates, confirms, decides, and accepts each output, and quantify the customer meeting or work burden. Do not use generic labels such as `proposer` when the company name is known.
- Keep internal traceability in the working Markdown or evidence ledger. Customer-facing pages must not cite `the problem statement`, `the customer page`, internal source IDs, or production notes as though they were external evidence.
- When a customer specifies base, main, and accent colors, use only those approved color families for the body unless a separately approved exception is necessary. Verify the dominant/background, main-structure, and accent proportions across the rendered deck, not only in the source theme.
