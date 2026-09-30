# ZASS — KeraniClaw Core

**Project:** KeraniClaw — OpenClaw-Native Architecture Experiment  
**Repository:** `dzuddiyn/KeraniClaw-Core`  
**ZASS baseline:** Zero-to-Architecture Structured Sprint v0.1.18  
**Status:** DISCOVERY / ARCHITECTURE RECONNAISSANCE  
**Owner:** Project Owner  
**Created:** 2026-09-28

> **ZASS Principle #1 — Bukan potong fikir; potong ulang fikir.**
>
> **ZASS Principle #2 — Fikir bebas. Rekod keputusan. Kunci yang pasti. Bina dari yang terkunci.**
>
> **ZASS Principle #3 — AI menghasilkan kemungkinan. Evidence menguji. Manusia memutuskan. Architecture mematuhi keputusan.**

---

# 0. AI OPERATING RULES

This file is the engineering source of truth for the KeraniClaw track.

KeraniClaw is a separate architecture experiment from the main Kerani Core track.

AI may:

- generate ideas;
- challenge assumptions;
- identify risks;
- compare approaches;
- inspect OpenClaw documentation/source;
- inspect this repository;
- propose experiments;
- produce candidate decisions; and
- map Kerani behaviours onto OpenClaw extension surfaces.

AI may NOT:

- silently transfer LOCKED decisions from Kerani Core into KeraniClaw;
- silently transfer KeraniClaw decisions back into Kerani Core;
- treat an AI suggestion as an owner decision;
- modify a LOCKED decision without explicit owner approval;
- treat OpenClaw agent memory as authoritative business truth;
- treat OpenClaw runtime/session state as an authoritative business record;
- modify OpenClaw core when a stable plugin/tool/skill/hook/configuration/external-service boundary is sufficient;
- begin large-scale implementation before the relevant architecture gaps are understood.

Only the project owner may change a decision to `LOCKED`.

Decision states:

`RAW → CANDIDATE → TESTING → DECIDED → LOCKED`

Other states:

`REJECTED` · `DEFERRED` · `SUPERSEDED`

Evidence labels used in this project:

- `EXPLICIT` — directly stated by the project owner.
- `OBSERVED` — directly observed in repository/source/documentation.
- `INFERRED` — interpretation that still needs confirmation.
- `CANDIDATE` — proposed direction, not an architecture decision.
- `TESTED` — reproduced by a defined test or prototype.
- `LOCKED` — owner-approved decision recorded in this file.

---

# 1. RAW IDEA

## Original Idea

> **EXPLICIT — Project Owner**
>
> Test an architecture alternative to the main Kerani Core track where **OpenClaw is the main host/runtime/architecture captain** and Kerani business behaviour is adapted to live natively on top of OpenClaw rather than rebuilding generic agent infrastructure from first principles.

Working project name:

`KeraniClaw`

Primary experiment question:

> **What is the minimum adaptation required for the behavioural responsibilities of Kerani_Core_SuperBasic to work natively on top of OpenClaw?**

## Why I Want This

- Avoid rebuilding infrastructure OpenClaw already provides.
- Determine how much Kerani-specific code really remains after adopting OpenClaw.
- Test whether Kerani can preserve its business-truth discipline while using an existing agent runtime.
- Reduce owner maintenance burden if upstream infrastructure can be reused safely.
- Produce evidence for a later comparison between Kerani-native, OpenClaw-native and possible hybrid approaches.

---

# 2. GOALS

1. Perform architecture reconnaissance of current OpenClaw from primary sources.
2. Map Kerani_Core_SuperBasic behavioural responsibilities to OpenClaw equivalents.
3. Classify each responsibility as:
   - `REUSE`
   - `ADAPT`
   - `KEEP`
   - `REMOVE`
   - `UNKNOWN`
4. Identify which Kerani behaviours are genuinely business-specific.
5. Preserve the distinction between:
   - AI/runtime memory;
   - Client Knowledge Base; and
   - Authoritative Business Record.
6. Preserve controlled business truth progression:
   `raw inbound event → interpretation → clarification/validation → reporter confirmation → authoritative business record`.
7. Prefer stable OpenClaw extension mechanisms over modifying OpenClaw core.
8. Compare this track with Kerani Native only after evidence is sufficient.
9. Keep GitHub KeraniClaw as the engineering source of truth for this track.

---

# 3. NON-GOALS

- Do not redesign the main Kerani Core repository from this project.
- Do not assume Kerani Core LOCKED decisions automatically apply here.
- Do not fork or heavily modify OpenClaw core without evidence that extension mechanisms are insufficient.
- Do not begin broad implementation before understanding the architecture boundary.
- Do not turn Kerani into a general chatbot.
- Do not treat multi-model or multi-AI agreement as evidence.
- Do not select an architecture winner before experiments produce evidence.

---

# 4. CONSTRAINTS

## Existing Infrastructure

- Migration source / behavioural baseline: `dzuddiyn/Kerani_Core_SuperBasic`.
- Host/runtime under investigation: current official OpenClaw.
- KeraniClaw authoritative repository: `dzuddiyn/KeraniClaw-Core`.
- ZASS process baseline: `dzuddiyn/ZASS-Zero-to-Architecture-Structured-Sprint`.

## Operational Constraints

- UNDERSTAND → MAP → IDENTIFY GAP → DESIGN MINIMUM ADAPTATION → TEST → IMPLEMENT.
- Repository/source inspection takes priority over chat memory when repository facts are available.
- Current OpenClaw documentation and official source are architecture authority for OpenClaw behaviour.
- Separate documented OpenClaw behaviour from inference and KeraniClaw proposals.

## Security / Privacy

- OpenClaw runtime state must not be assumed to be a Kerani authoritative business database.
- Business writes require explicit business semantics, validation and authorization.
- AI must not silently transform interpreted information into authoritative business truth.
- Tenant isolation must be proven; multi-agent isolation must not be assumed to equal hostile multi-tenant isolation.

## Maintenance

Preference order before OpenClaw core modification:

1. configuration;
2. skill;
3. tool;
4. hook;
5. plugin;
6. adapter;
7. external service/storage;
8. OpenClaw core modification only when necessary and justified.

Any proposed OpenClaw core modification must document:

- upstream limitation;
- why extension surfaces are insufficient;
- maintenance cost;
- upgrade impact; and
- alternatives considered.

---

# 5. IDEA BLAST

| ID | Idea | Source | Status |
|---|---|---|---|
| I-001 | OpenClaw acts as the agent operating infrastructure; Kerani owns the business-truth lifecycle. | AI synthesis from STEP 1 | CANDIDATE |
| I-002 | KeraniClaw may primarily be an OpenClaw plugin plus Kerani tools, hooks, skills and external authoritative business storage. | AI synthesis from STEP 1 | CANDIDATE |
| I-003 | Generic Telegram transport, Gateway, agent loop, provider abstraction, queue, scheduler and operational logging may not need to be rebuilt by Kerani. | STEP 1 reconnaissance | CANDIDATE |
| I-004 | Kerani business validation, clarification, reporter confirmation, authorization, idempotent business commands and audit semantics remain a Kerani layer. | STEP 1 reconnaissance | CANDIDATE |
| I-005 | Use hooks to enforce boundaries before dangerous or authoritative tools execute. | STEP 1 reconnaissance | CANDIDATE |
| I-006 | Skills should encode workflow/SOP guidance, while hard integrity controls should live in tools/hooks/business services. | STEP 1 reconnaissance | CANDIDATE |

---

# 6. QUESTIONS / UNKNOWNS

| ID | Question | Why It Matters | Status |
|---|---|---|---|
| Q-001 | Can all required Kerani business controls be implemented through public OpenClaw plugin/tool/hook APIs without patching core? | Determines upgrade and maintenance cost. | OPEN |
| Q-002 | What exact hook/tool boundaries best enforce clarification, confirmation and authoritative write rules? | Prevents business truth from depending on prompt obedience alone. | OPEN |
| Q-003 | What should be stored inside OpenClaw session/memory versus Kerani Client Knowledge Base versus authoritative business storage? | Prevents state contamination and ambiguous truth. | OPEN |
| Q-004 | How should Kerani implement idempotency for external business writes when runtime retries occur? | Prevents duplicated stock, claims, invoices or other business actions. | OPEN |
| Q-005 | What tenant topology is safe if multiple mutually untrusted clients use KeraniClaw? | OpenClaw multi-agent separation may not equal a hostile multi-tenant trust boundary. | OPEN |
| Q-006 | What minimum second application or vertical slice can prove that Kerani business behaviour is reusable on OpenClaw? | Needed before claiming architecture success. | OPEN |
| Q-007 | Which SuperBasic TEST/production controls should be retained even when OpenClaw provides runtime isolation? | Protects production data and writes. | OPEN |
| Q-008 | What business storage technology is appropriate after the host/runtime boundary is proven? | Storage must support authoritative records and audit semantics. | OPEN |

---

# 7. RISKS & FAILURE SCENARIOS

| ID | Failure / Risk | Impact | Possible Mitigation | Status |
|---|---|---|---|---|
| R-001 | OpenClaw memory is mistaken for authoritative business truth. | Incorrect records and hidden truth changes. | Explicit storage boundary; authoritative writes only through controlled Kerani tools/services. | OPEN |
| R-002 | OpenClaw session state is mistaken for a business transaction store. | Lost or ambiguous business state. | Separate runtime/session state from business record state. | OPEN |
| R-003 | OpenClaw queue/concurrency is assumed to solve business idempotency. | Duplicate external side effects. | Business-level idempotency keys and transactional rules. | OPEN |
| R-004 | Skills are treated as security/integrity enforcement. | Model may bypass or misinterpret instructions. | Put hard controls in tools, hooks and business services. | OPEN |
| R-005 | Multi-agent isolation is assumed to equal hostile multi-tenant isolation. | Cross-client data/security risk. | Define and test trust boundaries and deployment topology. | OPEN |
| R-006 | Kerani imports or patches OpenClaw internals. | Upgrade fragility and maintenance burden. | Use public plugin SDK and external boundaries first. | OPEN |
| R-007 | Runtime retry replays a business side effect. | Duplicate stock movements, claims or other writes. | Idempotent business command contract. | OPEN |
| R-008 | OpenClaw operational audit is treated as business audit provenance. | Incomplete evidence of who confirmed and what changed. | Kerani-specific business audit trail. | OPEN |
| R-009 | Kerani-specific workflow leaks into generic infrastructure. | Harder upgrades and reuse. | Keep business logic behind explicit plugin/tool/service boundaries. | OPEN |

---

# 8. METHOD REVIEWS

## MR-001 — OpenClaw Architecture Reconnaissance

**Method:** Architecture reconnaissance + responsibility mapping  
**Scope:** OpenClaw subsystems relevant to KeraniClaw and Kerani_Core_SuperBasic behavioural responsibilities  
**Reviewer / Model:** ChatGPT GPT-5.6 Sol  
**Date:** 2026-09-28

### Evidence Scope

Primary-source reconnaissance focused on current official OpenClaw documentation/source for:

- Gateway;
- channel adapters and routing;
- agent runtime and agent loop;
- sessions;
- queue/concurrency;
- plugins;
- tools;
- skills;
- hooks;
- memory/context;
- state/storage;
- identity/authentication/authorization;
- multi-agent and trust boundaries;
- scheduler/automation;
- retries/recovery;
- backup;
- observability;
- model/provider abstraction;
- deployment and upgrade boundaries.

Kerani_Core_SuperBasic repository inspection established that it is currently an extraction landing zone / behavioural specification rather than a completed generic runtime.

### Findings

1. **OBSERVED** — Kerani_Core_SuperBasic currently describes candidate reusable responsibilities but states that no generic runtime has yet been extracted.
2. **OBSERVED** — OpenClaw already provides a substantial portion of generic agent infrastructure relevant to those candidate responsibilities.
3. **INFERRED** — KeraniClaw can likely remove or avoid implementing several generic SuperBasic infrastructure layers if OpenClaw is adopted as host.
4. **INFERRED** — The strongest remaining Kerani boundary is business-truth governance rather than agent runtime mechanics.
5. **INFERRED** — OpenClaw plugin/tool/hook surfaces appear promising enough to test before any core modification.
6. **INFERRED** — OpenClaw memory, session state and operational audit should remain conceptually separate from Kerani Client Knowledge Base and authoritative business records.

### Contradictions

- None currently recorded between the owner-stated KeraniClaw hypothesis and STEP 1 evidence.
- No comparison winner versus Kerani Native has been selected.

### New Questions

- Q-001 to Q-008.

### New Risks

- R-001 to R-009.

### Experiments Suggested

- E-001 to E-004 below.

### Candidate Decisions

- D-001 to D-008 below.

---

# 9. MULTI-AI REVIEW RULES

Agreement between AI models is not evidence.

Disagreement must be converted into:

- a question;
- an experiment;
- a trade-off; or
- a candidate decision.

No multi-AI review has yet been recorded for KeraniClaw.

| ID | Topic | Model / Reviewer Views | What Must Be Resolved | Result |
|---|---|---|---|---|
| MA-001 | — | — | — | OPEN |

---

# 10. OPTIONS

## Decision Topic: OpenClaw Integration Depth

### Option A — Stable Extension Layer

**Description:**  
Keep OpenClaw upstream intact and implement Kerani through public plugin/tool/hook/skill/configuration/external-service mechanisms.

**Advantages:**

- lower upstream maintenance burden;
- easier OpenClaw upgrades;
- clearer responsibility boundary;
- aligns with project preference.

**Disadvantages:**

- may expose limits in public extension APIs;
- business controls may need careful orchestration across multiple extension surfaces.

**Risks:**

- hidden dependency on undocumented behaviour if boundaries are not tested.

**Evidence:**

- STEP 1 reconnaissance suggests plugin/tool/hook capabilities are substantial but not yet proven with Kerani behaviour.

### Option B — OpenClaw Core Modification

**Description:**  
Patch/fork OpenClaw internals where extension surfaces are insufficient.

**Advantages:**

- maximum control.

**Disadvantages:**

- larger maintenance surface;
- upgrade conflicts;
- tighter coupling to upstream internals.

**Risks:**

- maintenance nightmare for a single maintainer.

**Evidence:**

- No current evidence requires this option.

---

# 11. ARCHITECTURE CANDIDATES

## AC-001 — OpenClaw-Native Kerani Business Layer

**Summary:**

OpenClaw owns agent operating infrastructure. KeraniClaw adds controlled business semantics through stable extension surfaces and separate authoritative business storage.

**Key characteristics:**

- OpenClaw Gateway and agent runtime;
- native channel integration;
- OpenClaw sessions, queue, scheduler, provider abstraction and observability;
- Kerani plugin;
- Kerani tools;
- Kerani hooks;
- Kerani skills for workflow guidance;
- external or explicitly separated authoritative business storage;
- Kerani validation, confirmation, authorization, idempotency and business audit semantics.

**Dependencies:**

- public OpenClaw extension APIs are sufficient;
- business storage and transaction contract remain under Kerani control.

**Advantages:**

- potentially much less generic infrastructure for Kerani to maintain;
- faster channel/provider/runtime reuse;
- preserves an explicit business layer.

**Trade-offs:**

- dependency on OpenClaw upstream;
- plugin API compatibility becomes important;
- tenancy/security topology still unresolved.

**Critical risks:**

- R-001 through R-009.

### Candidate Comparison

| Criterion | AC-001 |
|---|---|
| Maintainability | TO TEST |
| Cost | TO TEST |
| Complexity | TO TEST |
| Reliability | TO TEST |
| Offline resilience | TO TEST |
| AI portability | Promising; verify |
| Scalability | TO TEST |
| Security | TO TEST |
| Observability | Promising; verify Kerani business audit separately |
| Single-maintainer suitability | Promising; verify upgrade burden |

No architecture winner is selected.

---

# 12. DECISION LEDGER

## D-001 — OpenClaw as KeraniClaw host/runtime

**Status:** LOCKED

**Problem:**  
Should KeraniClaw use OpenClaw as the primary host/runtime instead of rebuilding generic agent infrastructure?

**Options considered:**

- OpenClaw as primary host/runtime;
- Kerani-built runtime.

**Decision:**  
Use OpenClaw as the primary KeraniClaw host/runtime for the experiment and intended production direction, subject to E-001 proving the boundary.

**Reason:**  
STEP 1 evidence shows significant infrastructure overlap, but the Kerani business layer still requires proof.

**Trade-offs:**  
Less Kerani infrastructure versus greater upstream dependency.

**Evidence / experiment:**  
MR-001; E-001.

**Affected modules:**  
Gateway, channels, sessions, queue, scheduler, providers, observability.

**Related risks:**  
R-005, R-006.

**Related questions:**  
Q-001, Q-005.

---

## D-002 — OpenClaw upstream-first extension policy

**Status:** LOCKED

**Problem:**  
How should KeraniClaw extend OpenClaw?

**Options considered:**

- public extension mechanisms;
- OpenClaw core fork/modification.

**Decision:**  
Use an upstream-first policy: configuration/skills/tools/hooks/plugin/adapter/external service before any OpenClaw core modification.

**Reason:**  
Public plugins/tools/hooks/skills/configuration/external services appear capable but require prototype proof.

**Trade-offs:**  
Extension stability versus unrestricted internal control.

**Evidence / experiment:**  
MR-001; E-001.

**Related risks:**  
R-006.

**Related questions:**  
Q-001.

---

## D-003 — Runtime state vs authoritative business state

**Status:** LOCKED

**Problem:**  
Can OpenClaw session/memory/state be treated as Kerani authoritative business storage?

**Options considered:**

- reuse OpenClaw runtime state as business truth;
- keep authoritative business storage logically separate.

**Decision:**  
Keep authoritative business state logically separate from OpenClaw session/memory/runtime state.

**Reason:**  
OpenClaw runtime state serves agent/session/runtime needs; Kerani requires explicit business provenance and controlled truth transitions.

**Trade-offs:**  
Separation adds a business storage boundary but reduces ambiguity.

**Evidence / experiment:**  
MR-001; E-002.

**Related risks:**  
R-001, R-002, R-008.

**Related questions:**  
Q-003, Q-008.

---

## D-004 — Kerani business-truth control layer

**Status:** LOCKED

**Problem:**  
Which behaviours remain Kerani-owned when OpenClaw provides the runtime?

**Options considered:**

- rely on agent prompt/workflow;
- enforce business truth through dedicated Kerani logic.

**Decision:**  
Kerani owns the business-truth control plane: interpretation → validation → clarification → reporter confirmation → authorization → idempotent business command → authoritative record → business audit provenance.

**Candidate boundary:**  
Interpretation → validation → clarification → reporter confirmation → authorization → idempotent business command → authoritative record → business audit provenance.

**Reason:**  
OpenClaw provides agent infrastructure but does not define Kerani-specific business truth semantics.

**Evidence / experiment:**  
MR-001; E-002.

**Related risks:**  
R-001, R-004, R-008.

**Related questions:**  
Q-002, Q-003.

---

## D-005 — Generic SuperBasic infrastructure reuse/removal

**Status:** LOCKED

**Problem:**  
Which SuperBasic responsibilities should KeraniClaw avoid rebuilding?

**Candidate removal/reuse scope:**

- Telegram transport;
- generic Gateway;
- generic agent loop;
- generic model/provider adapter;
- generic queue/concurrency;
- generic scheduler;
- generic reply routing;
- generic operational logging/telemetry;
- generic runtime backup mechanics.

**Decision:**  
Avoid rebuilding generic infrastructure already supplied by OpenClaw unless experiments expose a concrete gap.

**Reason:**  
OpenClaw already appears to provide these classes of infrastructure.

**Evidence / experiment:**  
MR-001; E-001.

**Related risks:**  
R-006.

---

## D-006 — Business idempotency remains Kerani-owned

**Status:** LOCKED

**Problem:**  
Can OpenClaw queue/retry semantics guarantee safe business side effects?

**Options considered:**

- rely on runtime serialization/retry;
- add business-level idempotency and transaction semantics.

**Decision:**  
Business idempotency and transaction semantics remain Kerani-owned.

**Reason:**  
Runtime execution coordination is not equivalent to safe replay of external business writes.

**Evidence / experiment:**  
MR-001; E-003.

**Related risks:**  
R-003, R-007.

**Related questions:**  
Q-004.

---

## D-007 — Skills vs enforcement boundary

**Status:** LOCKED

**Problem:**  
Should Kerani integrity rules be implemented primarily as skills/prompts?

**Options considered:**

- skills/prompts only;
- skills for guidance plus tools/hooks/services for hard controls.

**Decision:**  
Use skills/prompts for guidance; enforce integrity in deterministic tools/hooks/business services.

**Reason:**  
Business integrity must not rely exclusively on model obedience.

**Evidence / experiment:**  
MR-001; E-002.

**Related risks:**  
R-004.

**Related questions:**  
Q-002.

---

## D-008 — Tenant isolation topology

**Status:** LOCKED

**Problem:**  
How should KeraniClaw isolate mutually untrusted clients?

**Options considered:**  
PENDING research/experiment.

**Decision:**  
For mutually untrusted clients, use an isolated OpenClaw Gateway/cell per tenant trust boundary. The exact mechanism (Fleet, separate container, VM, or host) remains an implementation question.

**Reason:**  
Multi-agent isolation must not be assumed to equal a hostile multi-tenant security boundary.

**Evidence / experiment:**  
MR-001; E-004.

**Related risks:**  
R-005.

**Related questions:**  
Q-005.

---

# 13. LOCKED DECISIONS

On 2026-09-30 the project owner explicitly instructed **ZASS LOCK & COMMIT**.

The following decisions are now `LOCKED`:

- **D-001** — OpenClaw is the primary KeraniClaw host/runtime.
- **D-002** — Upstream-first extension policy; avoid OpenClaw core modification unless evidence proves it necessary.
- **D-003** — Authoritative business state remains separate from OpenClaw runtime/session/memory state.
- **D-004** — Kerani owns the business-truth control plane.
- **D-005** — Do not rebuild generic infrastructure already supplied adequately by OpenClaw.
- **D-006** — Business idempotency and transaction semantics remain Kerani-owned.
- **D-007** — Skills/prompts guide behaviour; deterministic tools/hooks/services enforce integrity.
- **D-008** — Mutually untrusted clients require an isolated Gateway/cell trust boundary.
- **D-009** — OpenClaw-facing integration remains a thin runtime adapter; durable Kerani business logic stays outside OpenClaw-specific internals.
- **D-010** — Initial pilot server baseline is 2 vCPU / 8 GB RAM / ~100 GB NVMe, with external model inference and no local LLM on the VPS.
- **D-011** — Conversation and relationship memory remain flexible, but authoritative business writes require explicit write intent through the controlled write gate; initial universal trigger: `/rekod`.

These locks establish the **design direction and starting operational baseline**. They do not claim that E-001 through E-006 have already passed. Experiments remain required to validate implementation details, capacity and assumptions. Any later change to a LOCKED decision must follow ZASS change control and explicit owner approval.

---

# 14. REJECTED IDEAS

| ID | Idea | Reason Rejected | Related Decision |
|---|---|---|---|
| — | — | — | — |

---

# 15. DEFERRED ITEMS

| ID | Item | Why Deferred | Revisit Trigger |
|---|---|---|---|
| DF-001 | Select KeraniClaw vs Kerani Native winner | Evidence is insufficient. | Complete architecture experiments and common evaluation criteria. |
| DF-002 | OpenClaw core modification | No demonstrated limitation requires it yet. | Public extension mechanism proven insufficient. |
| DF-003 | Broad coding/implementation | Premature before boundary experiments. | Minimum adaptation design is evidence-backed. |

---

# 16. OPEN LOOPS

- [ ] Prove public OpenClaw extension surfaces can implement Kerani hard-control boundaries.
- [ ] Define state/storage separation in executable terms.
- [ ] Test business idempotency under retry/replay.
- [ ] Define multi-client trust boundary/deployment topology.
- [ ] Map a concrete SuperBasic behavioural vertical slice into OpenClaw.
- [ ] Establish test strategy for authoritative business writes.
- [ ] Define evaluation measurements for later Track A vs Track B comparison.

---

# 17. EXPERIMENTS / EVIDENCE

## E-001 — Minimum Native OpenClaw Extension

**Question being tested:**  
Can KeraniClaw implement one complete Kerani interaction using only supported OpenClaw extension mechanisms?

**Hypothesis:**  
A plugin plus tool/hook/skill boundaries are sufficient without OpenClaw core modification.

**Method:**  
Design and later implement one minimal vertical slice using only public APIs.

**Success criteria:**  

- no OpenClaw core patch;
- channel input reaches Kerani logic;
- controlled tool boundary works;
- reply returns through OpenClaw;
- test is reproducible.

**Result:**  
PENDING.

**Conclusion:**  
PENDING.

**Affected decisions:**  
D-001, D-002, D-005.

---

## E-002 — Business Truth Gate

**Question being tested:**  
Can Kerani enforce clarification/confirmation before an authoritative business write independently of prompt compliance?

**Hypothesis:**  
Tools/hooks/business service can enforce the state transition.

**Method:**  
Build a TEST-only write path where insufficiently confirmed data is rejected by code-level rules.

**Success criteria:**  

- model cannot write authoritative data before required state is satisfied;
- approved state can write;
- audit evidence records the transition.

**Result:**  
PENDING.

**Conclusion:**  
PENDING.

**Affected decisions:**  
D-003, D-004, D-007.

---

## E-003 — Retry / Idempotency Test

**Question being tested:**  
Does repeated runtime execution produce only one business effect?

**Hypothesis:**  
A Kerani business-command idempotency key can prevent duplicate writes.

**Method:**  
Replay the same confirmed command and inject retry/failure conditions.

**Success criteria:**  
One authoritative business effect only.

**Result:**  
PENDING.

**Conclusion:**  
PENDING.

**Affected decisions:**  
D-006.

---

## E-004 — Tenant Trust Boundary Study

**Question being tested:**  
What deployment boundary is required for mutually untrusted Kerani clients?

**Hypothesis:**  
Agent separation alone may be insufficient for hostile multi-tenant isolation.

**Method:**  
Inspect OpenClaw trust-boundary documentation/source and design testable deployment alternatives.

**Success criteria:**  
A documented topology with explicit trust boundaries and no hidden cross-tenant state assumptions.

**Result:**  
PENDING.

**Conclusion:**  
PENDING.

**Affected decisions:**  
D-008.

---

# 18. ARCHITECTURE READINESS

- [x] Core experiment problem is clear.
- [ ] Primary production users are fully defined.
- [x] Experiment goals are defined.
- [x] Non-goals are defined.
- [x] Initial constraints are known.
- [ ] Critical workflows have been tested on OpenClaw.
- [x] Major initial failure scenarios have been identified.
- [x] Critical design-direction decisions D-001 through D-011 are LOCKED.
- [x] No current contradiction invalidates the experiment.
- [ ] Major candidate architecture evidence is sufficient.
- [x] Known blockers are documented.

**Readiness:** `NOT READY`

---

# 19. ARCHITECTURE GENERATION INSTRUCTION

When Architecture Readiness = READY and the owner explicitly confirms architecture, generate architecture using only:

1. goals;
2. constraints;
3. LOCKED decisions;
4. required workflows;
5. known risks;
6. validated evidence; and
7. explicitly accepted trade-offs.

Any missing architectural decision must be returned as:

`ARCHITECTURE BLOCKER`

Do not silently invent a KeraniClaw architecture.

---

# 20. CHANGE CONTROL

After architecture exists:

```text
New idea
↓
CANDIDATE
↓
Impact analysis
↓
DECISION
↓
Human approval
↓
LOCK
↓
Architecture update
```

LOCKED decisions must never be silently overwritten.

---

# 21. RECOMMENDED PROJECT STRUCTURE

Current stage:

```text
KeraniClaw-Core/
│
└── ZASS_KeraniClaw.md
```

Future structure is created only when evidence justifies it:

```text
KeraniClaw-Core/
│
├── ZASS_KeraniClaw.md
├── ARCHITECTURE.md
├── README.md
│
├── docs/
│   ├── adr/
│   ├── experiments/
│   └── reviews/
│
└── src/
```

Authority hierarchy:

```text
1. GitHub KeraniClaw-Core  ← authoritative
2. Local repository        ← working copy
3. AI project workspace    ← working context
4. AI memory               ← context only
5. Chat                    ← temporary thinking space
```

---

# 22. STEP 1 RESPONSIBILITY MAP

Initial mapping from Kerani_Core_SuperBasic behavioural responsibilities to OpenClaw.

| SuperBasic responsibility | OpenClaw equivalent | Initial classification |
|---|---|---|
| Telegram adapter | Native/channel plugin architecture | REUSE |
| Telegram receiving | Gateway/channel pipeline | REUSE |
| Message routing | Channel routing + bindings | REUSE |
| Basic message normalisation | Channel adapter/runtime context | REUSE / ADAPT |
| Apps Script runtime | OpenClaw Gateway + agent runtime | REMOVE candidate |
| Gemini API adapter | Model/provider subsystem | REMOVE candidate |
| Provider authentication | Model auth profiles | REUSE |
| Provider fallback | Model failover | REUSE |
| Generic agent loop | OpenClaw agent runtime | REMOVE candidate |
| Generic queue | OpenClaw queue/concurrency | REMOVE candidate |
| Generic worker | Gateway/background runtime | REMOVE / ADAPT |
| Concurrency control | Session/global lanes | REUSE |
| Scheduler | OpenClaw automations/cron | REUSE |
| Generic retry | Provider + automation retries | REUSE / ADAPT |
| Session state | OpenClaw sessions | REUSE |
| Conversation history | Session transcript | REUSE |
| Prompt assembly | OpenClaw runtime | REUSE |
| Tool invocation | OpenClaw tools | REUSE |
| Generic extension contract | Plugin SDK | REUSE |
| Runtime lifecycle interception | Plugin hooks | REUSE |
| Workflow instructions | Skills | ADAPT |
| AI memory | OpenClaw memory | REUSE selectively |
| Clarification | Agent + Kerani workflow | KEEP |
| Business validation | No generic Kerani semantics | KEEP |
| Reporter confirmation | No generic equivalent | KEEP |
| Human review | Partial runtime primitives | KEEP / ADAPT |
| Authoritative business write | No generic equivalent | KEEP |
| Business schema | No generic equivalent | KEEP |
| Business rules | No generic equivalent | KEEP |
| Client Knowledge Base | OpenClaw memory is insufficient as equivalent | KEEP |
| Business permissions | Partial runtime/tool policies | KEEP / ADAPT |
| Business identity | Partial OpenClaw identity | ADAPT |
| Business audit semantics | Operational audit only | KEEP |
| Business idempotency | Runtime dedupe insufficient | KEEP |
| Business data storage | Separate authoritative storage required for evaluation | KEEP |
| Deterministic reply mechanics | Channel/reply pipeline | REUSE / ADAPT |
| Safe fallback replies | Runtime/provider handling | REUSE / ADAPT |
| Logs | OpenClaw logging | REUSE |
| Operational telemetry | Diagnostics / telemetry | REUSE |
| TEST/runtime isolation | OpenClaw agents/workspaces/config/sandbox | ADAPT |
| Business TEST/PROD isolation | Not automatically guaranteed | KEEP discipline |
| Regression test harness | Kerani behaviour still needs tests | KEEP |
| Multi-agent infrastructure | OpenClaw agents | REUSE |
| SaaS tenant isolation | Requires explicit trust-boundary proof | UNKNOWN / GAP |
| Runtime backup | OpenClaw backup mechanisms | REUSE |
| Business-data backup | Business-store responsibility | KEEP |

---

# 23. LOCKED WORKING DIRECTION — IMPLEMENTATION EVIDENCE PENDING

```text
                    CHANNELS
        Telegram / WhatsApp / future
                       │
                       ▼
               ┌──────────────┐
               │   OpenClaw   │
               │   Gateway    │
               └──────┬───────┘
                      │
        routing / sessions / queue
        auth / scheduler / runtime
                      │
                      ▼
              OpenClaw Agent Loop
                      │
           ┌──────────┴───────────┐
           │                      │
           ▼                      ▼
     Kerani Skills          Kerani Plugin
                              │
                    tools + hooks + policy
                              │
                              ▼
                    ┌──────────────────┐
                    │ Kerani Business  │
                    │     Layer        │
                    ├──────────────────┤
                    │ interpretation   │
                    │ validation       │
                    │ clarification    │
                    │ confirmation     │
                    │ authorization    │
                    │ idempotency      │
                    │ audit semantics  │
                    └────────┬─────────┘
                             │
                    explicit approved write
                             │
                             ▼
                    ┌──────────────────┐
                    │ Authoritative    │
                    │ Business Store   │
                    └──────────────────┘
```

Current synthesis:

> **LOCKED DIRECTION:** OpenClaw owns how an agent runs. Kerani owns when a business statement becomes authoritative truth.

This locks the architecture direction, not the final implementation architecture. E-001 through E-006 remain required before Architecture Readiness can become READY.

---

# 24. LATER EVALUATION CRITERIA

When evidence is sufficient, compare Kerani Native and KeraniClaw using the same criteria:

- amount of Kerani-maintained code;
- complexity;
- owner maintainability;
- development speed;
- onboarding speed;
- channel integration effort;
- reliability;
- recovery;
- security;
- tenant isolation;
- business record integrity;
- module reuse;
- AI/provider portability;
- OpenClaw upgrade risk;
- vendor/project dependency;
- server/resource requirement;
- cost;
- debuggability;
- observability;
- testability; and
- migration effort.

Do not select a winner before evidence exists.

---

# 25. OWNER AGREEMENT — 2026-09-30

**EXPLICIT — Project Owner**

The owner agreed to continue KeraniClaw in this repository using the working direction discussed on 2026-09-30.

The agreed direction is:

> **OpenClaw = replaceable Agent OS / runtime.**  
> **Kerani Core = permanent Business Control Plane.**  
> **Kerani Modules = domain capabilities.**  
> **External systems (for example n8n, Node-RED, Home Assistant, databases and business systems) = specialised infrastructure/workers.**

This agreement was subsequently followed by an explicit owner instruction on 2026-09-30 to **ZASS LOCK & COMMIT**. D-001 through D-010 are therefore LOCKED as recorded in the Decision Ledger and Locked Decisions section. Evidence from the defined experiments is still required before final architecture confirmation.

## D-009 — Thin runtime adapter boundary

**Status:** LOCKED

**Problem:**  
How much Kerani-specific logic should live inside OpenClaw-native extension code?

**Decision:**  
Keep OpenClaw-facing code thin. The preferred boundary is:

```text
OpenClaw
   ↓
Thin Kerani Runtime Adapter
   ↓
Kerani Business Control Plane / Service
   ↓
Kerani Modules
   ↓
Authoritative Business Store
```

The adapter may normalize requests, invoke controlled Kerani services and return responses, but durable business rules, authoritative write lifecycle, audit semantics and module contracts should remain outside OpenClaw-specific internals.

**Reason:**  
This minimizes coupling to experimental or changing OpenClaw extension APIs and preserves replaceability of the agent runtime.

**Status note:**  
LOCKED by explicit owner instruction on 2026-09-30.

---

## D-010 — Initial VPS sizing baseline

**Status:** LOCKED

**Question:**  
What server size should be used for the first always-on KeraniClaw pilot?

**LOCKED initial pilot baseline:**

- Ubuntu LTS;
- 2 vCPU;
- 8 GB RAM;
- approximately 100 GB NVMe;
- Docker / Docker Compose;
- OpenClaw Gateway;
- thin Kerani adapter / business service;
- PostgreSQL for TEST/pilot where appropriate;
- no local LLM on this VPS;
- model inference provided by external model providers;
- Tailscale or SSH-based administrative access;
- Gateway kept loopback/private by default.

**Cost evidence checked 2026-09-30:**

- OpenClaw official VPS guidance states an absolute minimum around 1 vCPU / 1 GB RAM and recommends 2 GB+ RAM for headroom.
- Hostinger Malaysia OpenClaw KVM 2 is currently advertised at RM38.99/month promotional pricing, renewing at RM59.99/month, with 2 vCPU, 8 GB RAM, 100 GB NVMe and 8 TB bandwidth.
- Hostinger lists Malaysia and Singapore among available VPS locations.
- DigitalOcean Basic 2 vCPU / 4 GiB is USD24/month; 4 vCPU / 8 GiB is USD48/month.
- Hetzner Singapore cloud pricing is materially higher than its European regions after the 15 June 2026 adjustment.

**Interpretation:**  
A 2 vCPU / 8 GB VPS is a strong pilot starting point when LLM inference is external. 16 GB is a scale-up choice, not a starting requirement.

**Status note:**  
LOCKED as the initial pilot sizing baseline by explicit owner instruction on 2026-09-30. This does **not** lock a specific vendor or paid plan; vendor selection remains open. E-005 may justify a later change through normal ZASS change control.

---

## E-005 — VPS Capacity / Cost Test

**Question being tested:**  
Can a 2 vCPU / 8 GB VPS run the OpenClaw Gateway, Kerani adapter/business service, PostgreSQL and operational logging with adequate headroom for a real pilot?

**Method:**

1. Deploy the pilot stack using pre-built images where possible.
2. Record idle RAM/CPU.
3. Run normal Telegram/channel traffic.
4. Run concurrent worker/sub-agent tasks.
5. Measure peak RAM, CPU, disk growth and restart behaviour.
6. Repeat with browser automation disabled and enabled separately.

**Success criteria:**

- no sustained memory pressure / OOM;
- Gateway remains responsive;
- business service and database remain responsive;
- normal workload stays below a defined headroom threshold;
- upgrade to a larger VPS is based on measurements rather than assumption.

**Result:**  
PENDING.

**Affected decisions:**  
D-010.

---


# 26. OWNER LOCK — CONVERSATIONAL MEMORY & EXPLICIT WRITE GATE

## D-011 — Conversational Memory & Explicit Business Write Gate

**Status:** LOCKED

**Owner principle:**  
> **Sembang bebas. Rekod terkawal.**

**Decision:**  
KeraniClaw MUST separate natural human conversation and relationship memory from authoritative business writes.

Normal conversation may:

- converse naturally and casually;
- remember user communication preferences;
- adapt tone, response length and explanation style;
- learn stable working preferences and recurring patterns;
- retain non-authoritative conversational context;
- reason over authoritative business data;
- answer questions and generate reports from authoritative records;
- suggest actions;
- ask clarifying questions.

Normal conversation MUST NOT create or mutate an authoritative business record merely because a user mentioned a fact during chat.

Authoritative write intent MUST be explicit.

The initial universal write trigger is:

`/rekod`

A controlled equivalent keyword or explicit structured write action MAY be added later, but silent inference from ordinary conversation MUST NOT be sufficient to create an authoritative record.

### Write flow

```text
/rekod
   ↓
interpret business intent
   ↓
validate required fields
   ↓
identify / authorize actor
   ↓
clarify ambiguity if required
   ↓
confirm material interpretation when required
   ↓
idempotency / duplicate protection
   ↓
authoritative write
   ↓
business audit trail
```

### Read / report flow

Read-only questions and reports do not require `/rekod`.

Examples:

- "Berapa stok baja sekarang?"
- "Plot mana hasil jatuh minggu ini?"
- "Buat summary claim belum selesai."
- "Bandingkan penggunaan baja bulan ini dengan bulan lepas."

Kerani may interpret these naturally, query authoritative records and respond conversationally, subject to permissions.

### Memory boundary

KeraniClaw SHALL distinguish at least these four conceptual classes:

1. **User / Relationship Memory**
   - preferred tone;
   - preferred response style;
   - recurring work habits;
   - stable user preferences;
   - user goals relevant to the working relationship.

2. **Conversation / Session Memory**
   - temporary discussion context;
   - current task;
   - short-lived references and unresolved conversation state.

3. **Client Knowledge**
   - company terminology;
   - SOP;
   - organizational structure;
   - plot/site/product definitions;
   - non-transactional domain knowledge.

4. **Authoritative Business Record**
   - stock;
   - transactions;
   - harvest;
   - claims;
   - tasks;
   - approved operational records;
   - other business facts designated authoritative.

If memory conflicts with the authoritative business record, the authoritative business record wins.

### Preference learning rule

Kerani may infer preference candidates from repeated interaction, but a single casual statement SHOULD NOT automatically become a durable preference unless:

- the user explicitly asks Kerani to remember it; or
- repeated evidence makes the preference stable enough under the future memory policy.

Preference memory MUST NOT be treated as an authoritative business record.

### Human-facing design objective

Kerani SHOULD feel increasingly familiar and natural to each user over time, while preserving strict record integrity underneath.

The intended experience is:

```text
natural human conversation
        ↓
relationship / preference awareness
        ↓
reasoning / reporting / suggestions
        ↓
explicit write intent only
        ↓
deterministic Business Gate
        ↓
authoritative business record
```

**Reason:**  
OpsMate demonstrated strong record discipline but a rigid conversational surface. KeraniClaw should preserve the discipline behind the interface while allowing the human-facing layer to remain conversational, adaptive and relationship-aware.

**Related locked decisions:**  
D-003, D-004, D-006, D-007, D-009.

**Status note:**  
LOCKED by explicit owner instruction on 2026-09-30.

---

## E-006 — Conversational Freedom vs Record Integrity Test

**Question being tested:**  
Can Kerani hold natural, preference-aware conversations without accidentally creating authoritative business records, while `/rekod` reliably opens the controlled write path?

**Method:**

1. Run ordinary conversations containing numbers, estimates, corrections and uncertain statements.
2. Verify that no authoritative write occurs without explicit write intent.
3. Use `/rekod` with complete and incomplete business statements.
4. Verify validation, clarification, authorization, confirmation and idempotency behaviour.
5. Request read-only reports in natural language without commands.
6. Verify that relationship memory and business truth remain distinct.
7. Introduce a deliberate conflict between conversational memory and authoritative data and verify that authoritative data wins.

**Success criteria:**

- zero silent authoritative writes from ordinary chat;
- `/rekod` reliably invokes the controlled write path;
- incomplete writes are clarified or rejected safely;
- read-only reporting remains conversational;
- user preferences may affect presentation but not business truth;
- authoritative records override conflicting conversational memory.

**Result:**  
PENDING.

**Affected decisions:**  
D-003, D-004, D-006, D-007, D-011.

---

# 27. SOURCE NOTES

Project repositories:

- KeraniClaw: https://github.com/dzuddiyn/KeraniClaw-Core
- Kerani_Core_SuperBasic: https://github.com/dzuddiyn/Kerani_Core_SuperBasic
- ZASS baseline: https://github.com/dzuddiyn/ZASS-Zero-to-Architecture-Structured-Sprint

OpenClaw evidence for this track must preferentially use current official OpenClaw documentation and official source repository. Third-party summaries are secondary evidence only.

---

# END OF KERANICLAW ZASS
