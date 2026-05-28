# Routing Matrix

This file is the reviewed routing layer for `chooseable-options/SKILL.md`.

Use it together with your project skill index and `references/context-normalization.md`.

### Markers

- `[G]` - generated or directly derivable from the current skill inventory
- `[R]` - reviewed chooseable-options routing annotation

### Routing Roles

- `PRIMARY_ROUTER` - first-pass next-step router
- `DIRECT_TARGET` - can be recommended directly as the next action
- `SPECIALIST_ESCALATION` - relevant only when the domain lane is already explicit
- `OPTION_PRODUCER_ONLY` - may emit its own next-step menus, but is not the first route in general routing
- `OUT_OF_SCOPE_FOR_FIRST_PASS` - inventory-aware but intentionally not a first-pass recommendation

### Review Status

- `REVIEWED_CURRENT` - row or bridge is current
- `REVIEW_REQUIRED` - routing coverage must be updated
- `DEFERRED_WITH_NOTE` - review identified but intentionally deferred

### Material-Change Review Rule

Chooseable-options review is required when a skill changes routing semantics through:

1. discovery-surface change (added, removed, renamed, re-partitioned index/metadata)
2. trigger-surface change (frontmatter description, explicit trigger language, `AUTO_TRIGGER_WHEN`, `AUTO_SUGGEST_WHEN`)
3. handoff or ownership change (`Cross-Skill Coordination`, `HANDOFF_OUTPUTS`, workflow ownership)
4. user-choice-surface change (`CHOOSEABLE_OPTIONS`, closeout menus, selector-token affordances)

Routing-neutral changes normally do not require matrix edits: typo cleanup, formatting cleanup,
example refresh with unchanged semantics, bundled-doc refreshes with unchanged routeability,
internal refactors that preserve the same purpose, trigger surface, and handoff role.

When uncertain, bias toward review.

## Scenario Routing Lanes

| `SCENARIO_KEY` | [R] Intent Signals | [R] Required Evidence Inputs | [R] Recommended First Route | [R] Secondary Routes | [R] Do Not Route To | [R] Escalation Rule | [R] User-Facing Option Token Family |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `BLOCKING_DECISION` | approval, tradeoff, blocker, ambiguous implementation direction | open questions, blockers, current recommendation, owning artifact path | `decisions/SKILL.md` | `explain/SKILL.md`; `study/SKILL.md`; `research-online/SKILL.md` | `step-by-step/SKILL.md`; specialist execution skills | if the decision set is still under-evidenced, deepen through study or research before returning to decisions | `CHOOSEABLE_OPTIONS` |
| `EXPLANATION_SURFACE_REQUIRED` | explain, summarize visually, make this digestible, show the flow, clarify what this artifact means | source artifact or code surface, exact question, intended downstream choice or action | `explain/SKILL.md` | `decisions/SKILL.md`; `plan/SKILL.md`; `study/SKILL.md` | open-ended research when the source is already known; direct implementation before the explanation target is clear | if the explanation exposes missing or conflicting evidence, escalate to study or research before continuing | `CHOOSEABLE_OPTIONS` |
| `EXECUTION_ARTIFACT_READY` | plan, study, spec, audit, or session artifact already points toward action | artifact path, recommendation summary, known blockers, validation surface | `plan/SKILL.md` unless the artifact already names a better direct target | specialist target named by the artifact; standalone task-tracking workflow when the artifact explicitly needs it; `explain/SKILL.md`; `study/SKILL.md` when the artifact still needs bounded deepening before planning | open-ended router loops with no artifact use | if blockers remain unresolved, downgrade to `BLOCKING_DECISION` first; if the artifact is a study but the concrete change inventory or regression/doc blast radius is still fuzzy, keep a bounded study-deepening option visible before planning; if the artifact is a study with a chosen direction and concrete implementation-ready inventory, keep an explanation option visible for "what can already be implemented now?" even while planning remains the recommended first route | `IMPLEMENTATION_OPTIONS` |
| `MANUAL_QUEUE_ACTIVE` | explicit checklist, rollout queue, operator sequence, shared verification steps | ordered step source, current active step, verification method | `step-by-step/SKILL.md` | `plan/SKILL.md`; standalone task-tracking workflow when queue state must be mirrored externally | `decisions/SKILL.md` unless a blocker actually emerges | if a step becomes a blocker choice, hand off to decisions and return after the choice | `CONTROL_OPTIONS` |
| `SKILL_AUTHORING_GOVERNANCE` | new skill, skill update, lifecycle policy, publish/validation flow, routing review | target skill path, namespace choice, validation/publish needs, routing-review need | project-local skill manager for the relevant namespace | `chooseable-options/SKILL.md`; project documentation-sync workflow | unrelated deploy, quality, or product specialist skills | if the change is routing-semantic, update this matrix in the same turn | `GOVERNANCE_OPTIONS` |
| `SPECIALIST_SURFACE_KNOWN` | clear domain lane already exists: deploy, worktrees, browser evidence, shadcn, visual review, backend debugging | domain signal, target app/system, current artifact if any | direct specialist project skill or expert bridge | `plan/SKILL.md`; `study/SKILL.md` | broad open-ended menus that hide the known specialist lane | if the specialist lane still has multiple blocker choices, escalate to decisions | `SPECIALIST_OPTIONS` |
| `CURRENT_EXTERNAL_FACTS_REQUIRED` | current docs, standards, product behavior, or vendor state are decisive | exact uncertainty, sources/domains, why local evidence is insufficient | `research-online/SKILL.md` | `study/SKILL.md`; relevant expert bridge | purely local implementation workflows as the sole recommendation | keep the base scenario visible and return after research with a tighter route | `RESEARCH_OPTIONS` |
| `OPEN_ENDED_NEXT_STEP` | user asks what to do next, but no stronger artifact or lane is ready yet | request summary, known repo state, weak evidence summary | `chooseable-options/SKILL.md` | `plan/SKILL.md`; `study/SKILL.md`; `research-online/SKILL.md`; `explain/SKILL.md` | specialist execution routes presented as if they were already validated | prefer artifact creation or blocker resolution before direct implementation | `CHOOSEABLE_OPTIONS` |

## In-Batch Workflow Skills

These rows cover the workflow skills shipped alongside this one. They are the canonical
first-pass routing targets the matrix is designed for.

| [G] Skill Name | [G] Description Summary | [R] Routing Role | [R] Review Status | [R] Route When | [R] Avoid When | [R] Typical Upstream Scenarios | [R] Typical Downstream Skills / Artifacts | [R] Last Reviewed |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| `chooseable-options/SKILL.md` | compact next-step router and option presenter | `PRIMARY_ROUTER` | `REVIEWED_CURRENT` | the user asks what to do next or wants chooseable options | a deeper workflow is already explicitly chosen and should run directly | `OPEN_ENDED_NEXT_STEP`, `EXECUTION_ARTIFACT_READY`, `SKILL_AUTHORING_GOVERNANCE`, `EXPLANATION_SURFACE_REQUIRED` | planning; decisions; step-by-step; study; explain; research-online; skill managers | `2026-05-03` |
| `decisions/SKILL.md` | blocking user-choice workflow | `DIRECT_TARGET` | `REVIEWED_CURRENT` | an approval, tradeoff, or blocker choice must be resolved | the next work is an ordered manual queue or already-selected implementation path | `BLOCKING_DECISION`, `EXECUTION_ARTIFACT_READY` | `explain/SKILL.md`; `plan/SKILL.md`; `study/SKILL.md` | `2026-04-09` |
| `explain/SKILL.md` | concise visual explanation packets for dense artifacts or code flows | `DIRECT_TARGET` | `REVIEWED_CURRENT` | a known artifact or code path needs lower-cognitive-load explanation before the next decision or execution lane | the task really needs new evidence, new planning, or direct implementation instead of explanation | `EXPLANATION_SURFACE_REQUIRED`, `BLOCKING_DECISION`, `EXECUTION_ARTIFACT_READY`, `OPEN_ENDED_NEXT_STEP` | `plan/SKILL.md`; `decisions/SKILL.md`; `study/SKILL.md`; standalone task-tracking workflow when explicitly needed | `2026-04-09` |
| `plan/SKILL.md` | durable implementation planning and execution orchestration | `DIRECT_TARGET` | `REVIEWED_CURRENT` | a study, spec, audit, or accepted scope is ready for execution planning | blocking choices remain unresolved or deeper study is still needed | `EXECUTION_ARTIFACT_READY`, `OPEN_ENDED_NEXT_STEP` | decisions; explain; project quality skills; documentation-sync; standalone task-tracking workflow only when explicitly requested outside planning | `2026-04-10` |
| `research-online/SKILL.md` | external research workflow grounded in current sources | `DIRECT_TARGET` | `REVIEWED_CURRENT` | current external information is the missing evidence | local repo evidence is already sufficient | `CURRENT_EXTERNAL_FACTS_REQUIRED`, `OPEN_ENDED_NEXT_STEP`, `EXECUTION_ARTIFACT_READY` | study; planning; decisions | `2026-04-08` |
| `specs/SKILL.md` | investigation-ready spec creation for findings | `DIRECT_TARGET` | `REVIEWED_CURRENT` | a discovered issue or opportunity needs a structured spec | the work is already execution-ready and should move straight to planning | `EXECUTION_ARTIFACT_READY`, `OPEN_ENDED_NEXT_STEP` | explain; planning; standalone task-tracking workflow when explicitly requested | `2026-04-09` |
| `step-by-step/SKILL.md` | single-active-step manual execution workflow | `DIRECT_TARGET` | `REVIEWED_CURRENT` | the next work is an ordered checklist or human-in-the-loop queue | the user still needs a blocker decision or deeper study first | `MANUAL_QUEUE_ACTIVE`, `EXECUTION_ARTIFACT_READY` | planning; standalone task-tracking workflow when queue status must be mirrored externally | `2026-04-08` |
| `study/SKILL.md` | evidence-backed technical study creation | `DIRECT_TARGET` | `REVIEWED_CURRENT` | the context is too ambiguous for confident execution routing, or a completed study still needs bounded deepening before planning | the work already has a durable plan or explicit manual queue and no meaningful evidence gaps remain | `OPEN_ENDED_NEXT_STEP`, `CURRENT_EXTERNAL_FACTS_REQUIRED`, `EXECUTION_ARTIFACT_READY` | planning; decisions; explain; research-online; specs-creator; bounded study deepening | `2026-04-21` |
| `delegate/SKILL.md` | delegated execution runtime for external providers | `SPECIALIST_ESCALATION` | `REVIEWED_CURRENT` | the next action requires delegate runtime or provider fan-out | local-only execution is sufficient | `SPECIALIST_SURFACE_KNOWN`, `EXECUTION_ARTIFACT_READY` | provider-specific runs; downstream quality or research skills | `2026-04-08` |

## Expert Capability Bridges

An expert capability is covered when its section appears below and the bridge still matches the
current reusable-skill inventory and first-pass routing needs.

| [G] Expert Section | [R] Use When | [R] How To Discover Specific Skills | [R] Typical Upstream Skills | [R] Review Trigger | [R] Review Status | [R] Last Reviewed |
| --- | --- | --- | --- | --- | --- | --- |
| Manager Skills | reusable-skill namespace governance or reusable-skill creation is the real next lane | shortlist the project-local skill manager first, then inspect the exact reusable skill only if the task is not repo-specific | `chooseable-options/SKILL.md` | new reusable skill; manager-policy change; reusable-skill routing change | `REVIEWED_CURRENT` | `2026-05-09` |
| Coding Tools | the user already needs a reusable coding-tool skill rather than a repo-specific workflow | read the Coding Tools section in the reusable-skill index, then inspect the most relevant skill descriptions or `SKILL.md` files on demand | `chooseable-options/SKILL.md`; `delegate/SKILL.md` | generated index change; semantic description change; bridge becomes newly first-pass relevant | `REVIEWED_CURRENT` | `2026-05-09` |
| Deploy | a reusable deploy-tool capability is needed beyond repo-local promotion or parity workflows | read the Deploy section in the reusable-skill index, then inspect the specific deploy skill on demand | `chooseable-options/SKILL.md`; project deploy skills | generated index change; semantic description change; deploy bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| Infra | an external infra platform or tool is explicit and repo-local wrappers are insufficient | read the Infra section in the reusable-skill index, then inspect the shortlist on demand | `chooseable-options/SKILL.md`; project infra skills; project deploy skills | generated index change; semantic description change; infra bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| Auth | the next action is a reusable authentication or OAuth capability decision | read the Auth section in the reusable-skill index, then inspect the specific auth skill on demand | `chooseable-options/SKILL.md`; planning; study | generated index change; semantic description change; auth bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| UI | the user needs a reusable UI system or terminal-UI capability beyond repo-local UI workflows | read the UI section in the reusable-skill index, then inspect the shortlist on demand | `chooseable-options/SKILL.md`; project UI skills; planning | generated index change; semantic description change; UI bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| Accessibility | accessibility guidance or WebAIM-specific expertise is explicitly needed | read the Accessibility section in the reusable-skill index, then inspect the shortlist on demand | `chooseable-options/SKILL.md`; project quality skills | generated index change; semantic description change; accessibility bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| Research | the next step is reusable external-research capability selection | read the Research section in the reusable-skill index, then inspect the shortlist on demand | `chooseable-options/SKILL.md`; `research-online/SKILL.md`; study | generated index change; semantic description change; research bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| Workflow | a reusable workflow automation capability is needed beyond repo-local workflow skills | read the Workflow section in the reusable-skill index, then inspect the shortlist on demand | `chooseable-options/SKILL.md`; planning | generated index change; semantic description change; workflow bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |
| Linting | the user explicitly needs reusable linting expertise rather than repo-local workflow guidance | read the Linting section in the reusable-skill index, then inspect the shortlist on demand | `chooseable-options/SKILL.md`; project UI skills; planning | generated index change; semantic description change; linting bridge becomes newly routeable | `REVIEWED_CURRENT` | `2026-04-08` |

## Pending Review Queue

Current status: empty on `2026-05-03`.

Use this section only when:

1. a skill was added or materially changed,
2. the current turn could not safely update the relevant row or bridge, and
3. the missing review must remain visible for the next workflow.
