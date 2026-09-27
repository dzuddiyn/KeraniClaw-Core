# ZASS — KeraniClaw Core

> **Project:** KeraniClaw — OpenClaw-Native Architecture Experiment  
> **Engineering source of truth:** `dzuddiyn/KeraniClaw-Core`  
> **Architecture discipline:** RAW → CANDIDATE → TESTING → DECIDED → LOCKED  
> Terminal/exception states: REJECTED · DEFERRED · SUPERSEDED

---

## 0. Status

| Field | Current value |
|---|---|
| Repository | `dzuddiyn/KeraniClaw-Core` |
| Track | KeraniClaw — OpenClaw-native architecture experiment |
| Repository role | Engineering source of truth for this track |
| Current phase | Architecture reconnaissance |
| Implementation status | No KeraniClaw implementation yet |
| Coding posture | UNDERSTAND → MAP → IDENTIFY GAP → DESIGN MINIMUM ADAPTATION → TEST → IMPLEMENT |
| Architecture status | Initial findings recorded; no implementation architecture LOCKED |

This repository is a separate architecture experiment from the main Kerani Core track.

Decisions from Kerani Core do not automatically become KeraniClaw decisions.  
KeraniClaw decisions do not modify or supersede Kerani Core decisions.

---

## 1. Experiment purpose

KeraniClaw tests an alternative architecture to Kerani Core:

```text
OpenClaw
    ↓
host / runtime / primary agent infrastructure
    ↓
Kerani business-specific layer
```

The experiment asks:

> **What is the minimum change required for the behavioural responsibilities of Kerani_Core_SuperBasic to operate natively on top of OpenClaw?**

The experiment must not rebuild generic agent infrastructure that OpenClaw already provides unless evidence shows a real gap.

---

## 2. Primary hypothesis — RAW

> If OpenClaw already solves most generic agent-infrastructure concerns — Gateway, channels, sessions, agent runtime, tools, plugins, model providers, queueing, scheduler and operational state — Kerani may only need to build the business-specific capabilities that remain unique.

Questions to answer with evidence:

1. What has OpenClaw already solved?
2. What does Kerani still need to own?
3. What can be reused directly?
4. What needs an adapter?
5. What belongs in a plugin, tool, skill or hook?
6. What belongs in an external business service/storage layer?
7. What can be removed from the SuperBasic responsibility set?
8. What remains unknown?

---

## 3. Architecture authority for this track

For OpenClaw facts:

1. Prefer official OpenClaw documentation.
2. Inspect the official OpenClaw repository when implementation detail matters.
3. Prefer primary sources over third-party articles.
4. Distinguish:
   - documented OpenClaw behaviour;
   - inference;
   - KeraniClaw proposal.
5. Use the currently verified OpenClaw implementation/version when architecture details can change.

For KeraniClaw:

> The current repository is authoritative once a decision, experiment or implementation is recorded here.

Chat is a thinking surface, not engineering authority.

---

## 4. Migration source

`Kerani_Core_SuperBasic` is the behavioural migration source.

Do not transplant the architecture of the main Kerani Core project into this repository.

For every SuperBasic responsibility, classify it as:

| Code | Meaning |
|---|---|
| A | OpenClaw already provides this function |
| B | Still required as Kerani business logic |
| C | Should become an OpenClaw plugin/tool/skill/hook |
| D | Should become an external business service/storage |
| E | No longer required |
| F | Unknown / evidence insufficient |

Current repository observation:

> `Kerani_Core_SuperBasic` is presently an extraction specification / landing zone and does not yet contain a completed generic runtime to transplant.

Therefore the first mapping is responsibility/behaviour based, not file-to-file migration.

---

## 5. Kerani business-truth principle

KeraniClaw must preserve the distinction:

```text
AI memory
    ≠
Client Knowledge Base
    ≠
Authoritative Business Record
```

Starting behavioural model:

```text
raw inbound event
      ↓
interpretation
      ↓
clarification / validation
      ↓
reporter confirmation
      ↓
authority / permission check
      ↓
authoritative business write
```

An AI interpretation, memory entry, model agreement or conversational statement must not silently become authoritative business truth.

This principle must be tested against OpenClaw extension mechanisms rather than assumed to be solved by OpenClaw.

---

## 6. STEP 1 — OpenClaw architecture reconnaissance

### 6.1 Observed infrastructure coverage

Initial reconnaissance indicates that OpenClaw provides substantial generic infrastructure in these areas:

- Gateway / control plane;
- channel integrations and adapters;
- deterministic channel routing;
- agent runtime and agent loop;
- sessions and conversation history;
- per-session/global queueing and concurrency controls;
- tools;
- plugins;
- skills;
- lifecycle hooks;
- model/provider abstraction;
- authentication and runtime access mechanisms;
- scheduler / automation;
- provider/runtime retry and failover;
- runtime state and recovery mechanisms;
- multi-agent support;
- operational logging and observability;
- runtime backup mechanisms;
- deployment/runtime infrastructure.

These are infrastructure capabilities, not proof that OpenClaw supplies Kerani business semantics.

### 6.2 Important boundaries found

#### OpenClaw session state ≠ Kerani business state

OpenClaw session/context infrastructure is appropriate for conversational continuity and runtime state.

It is not automatically suitable as the authoritative store for business facts, transactions, approvals or domain history.

#### OpenClaw memory ≠ Client Knowledge Base ≠ Authoritative Business Record

OpenClaw memory may support recall/context.

Kerani still needs evidence for business-specific provenance, authority, revisions, verification state and record integrity.

#### OpenClaw queue ≠ business transaction safety

OpenClaw can serialize agent/session work and manage runtime concurrency.

Kerani business writes may still need:

- idempotency keys;
- transaction semantics;
- duplicate-write protection;
- authority checks;
- explicit confirmation state;
- business-level retries/reconciliation.

#### OpenClaw operational audit ≠ Kerani business audit

Operational logs/telemetry can be reused.

Kerani may still need domain audit semantics such as:

- who reported;
- what was interpreted;
- what clarification occurred;
- who confirmed;
- what exact authoritative record changed;
- before/after values;
- source evidence/provenance.

#### OpenClaw multi-agent ≠ hostile multi-tenant isolation

Multi-agent/workspace/session separation must not be assumed to provide complete SaaS tenant isolation.

Tenant/trust-boundary architecture remains an unresolved area requiring explicit evidence and experiments.

---

## 7. Initial SuperBasic → OpenClaw mapping — CANDIDATE

| SuperBasic responsibility | OpenClaw equivalent / likely host | Candidate disposition |
|---|---|---|
| Telegram adapter | Channel architecture | REUSE |
| Telegram receiving | Gateway/channel pipeline | REUSE |
| Message routing | Channel routing/bindings | REUSE |
| Basic message normalisation | Channel/runtime context | REUSE / ADAPT |
| Apps Script runtime | OpenClaw Gateway + agent runtime | REMOVE candidate |
| Gemini-specific API adapter | Model/provider subsystem | REMOVE candidate |
| Provider authentication | Model auth/provider layer | REUSE |
| Provider fallback | Provider/model failover | REUSE |
| Generic agent loop | OpenClaw agent runtime | REMOVE candidate |
| Generic queue | OpenClaw queue/concurrency | REMOVE candidate |
| Generic worker | OpenClaw runtime/background execution | REMOVE / ADAPT |
| Concurrency control | Session/global lanes | REUSE |
| Scheduler | OpenClaw automation/scheduler | REUSE |
| Generic retry | Runtime/provider automation retry mechanisms | REUSE / ADAPT |
| Session state | OpenClaw sessions | REUSE for conversation/runtime only |
| Conversation history | OpenClaw session transcript/history | REUSE |
| Prompt assembly | OpenClaw runtime | REUSE |
| Tool invocation | OpenClaw tools | REUSE |
| Generic extension contract | OpenClaw plugin SDK | REUSE |
| Runtime lifecycle interception | Hooks | REUSE |
| Workflow instructions | Skills | ADAPT |
| AI memory | OpenClaw memory | REUSE selectively |
| Clarification | Agent + Kerani workflow | KEEP |
| Business validation | Kerani-specific | KEEP |
| Reporter confirmation | Kerani-specific | KEEP |
| Human review | Runtime primitives + Kerani semantics | KEEP / ADAPT |
| Authoritative business write | Kerani-specific | KEEP |
| Business schema | Kerani-specific | KEEP |
| Business rules | Kerani-specific | KEEP |
| Client Knowledge Base | Separate Kerani concern | KEEP |
| Business permissions | OpenClaw policy + Kerani domain rules | KEEP / ADAPT |
| Business identity | OpenClaw identity + Kerani domain identity | ADAPT |
| Business audit semantics | Separate Kerani semantics | KEEP |
| Business idempotency | Separate Kerani semantics | KEEP |
| Business data storage | External/domain storage | KEEP / D |
| Deterministic reply mechanics | OpenClaw reply/channel pipeline | REUSE / ADAPT |
| Safe fallback replies | Runtime/provider handling | REUSE / ADAPT |
| Operational logs | OpenClaw logging | REUSE |
| Operational telemetry | OpenClaw diagnostics/telemetry | REUSE |
| TEST/runtime isolation | OpenClaw config/workspace/sandbox capabilities | ADAPT |
| Business TEST/PROD isolation | Kerani operational discipline | KEEP |
| Regression tests | Kerani behaviour tests | KEEP |
| Multi-agent infrastructure | OpenClaw agents | REUSE |
| SaaS tenant isolation | Not yet demonstrated | UNKNOWN / GAP |
| Runtime backup | OpenClaw backup mechanisms | REUSE |
| Business-data backup | Business-storage responsibility | KEEP |

This table is a hypothesis to be tested. It is not an implementation plan.

---

## 8. Candidate architecture hypothesis — NOT LOCKED

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

Useful candidate boundary:

> **OpenClaw owns how an agent runs. Kerani owns when a business statement becomes authoritative truth.**

Status: **CANDIDATE**

---

## 9. Preferred extension direction — CANDIDATE

Avoid modifying OpenClaw core unless evidence proves the public extension surfaces are insufficient.

Preference order:

```text
configuration
    ↓
skill
    ↓
tool
    ↓
hook
    ↓
plugin
    ↓
external service / adapter
    ↓
OpenClaw core modification only if necessary
```

Current candidate packaging hypothesis:

```text
OpenClaw upstream
      +
KeraniClaw plugin
      +
Kerani skills
      +
Kerani business tools/hooks
      +
external authoritative business service/storage
```

No decision has yet been made that all of these layers are required.

---

## 10. Candidate components likely removable from Kerani-owned infrastructure

Initial reconnaissance suggests that Kerani may not need to maintain its own generic implementation of:

- Telegram transport;
- generic Gateway;
- generic session manager;
- generic queue/concurrency manager;
- generic scheduler;
- Gemini-specific runtime/provider adapter;
- generic provider failover;
- generic agent loop;
- generic plugin system;
- generic tool dispatcher;
- generic channel reply routing;
- generic operational logging;
- generic runtime backup machinery.

Status for every item: **CANDIDATE**, pending practical proof.

---

## 11. Candidate Kerani-specific responsibilities

Initial reconnaissance suggests the likely durable Kerani-specific surface is:

```text
Raw Business Event
       ↓
Interpretation
       ↓
Schema Validation
       ↓
Business Validation
       ↓
Clarification
       ↓
Reporter Confirmation
       ↓
Permission / Authority Check
       ↓
Idempotent Business Command
       ↓
Authoritative Record
       ↓
Business Audit Provenance
```

Potential responsibilities:

- verified business truth;
- business rules;
- validation;
- clarification;
- reporter approval/confirmation;
- authoritative records;
- Client Knowledge Base;
- business modules;
- domain workflows;
- audit semantics;
- business-specific permissions;
- domain identity;
- business idempotency;
- authoritative data storage.

Status: **CANDIDATE**

---

## 12. Initial architecture risks

| ID | Finding | State |
|---|---|---|
| KR-001 | OpenClaw memory could be mistaken for business truth | RAW |
| KR-002 | OpenClaw session state could be mistaken for business state | RAW |
| KR-003 | OpenClaw queue does not by itself prove business-write idempotency | RAW |
| KR-004 | Skills are guidance/instruction, not sufficient integrity enforcement | RAW |
| KR-005 | Multi-agent isolation does not automatically prove hostile multi-tenant isolation | RAW |
| KR-006 | Dependence on OpenClaw internal source APIs would increase upgrade/maintenance risk | RAW |
| KR-007 | Plugin/hook/tool surfaces appear promising but require prototype evidence | CANDIDATE TEST |
| KR-008 | Operational audit does not automatically replace Kerani business audit | RAW |
| KR-009 | Runtime retries may duplicate external side effects if Kerani tools are not idempotent | RAW |

---

## 13. Candidate decision ledger

| ID | Candidate decision | State | Evidence / reason |
|---|---|---|---|
| KC-001 | Use OpenClaw as the host/runtime architecture for this experiment | CANDIDATE | Core experiment hypothesis; needs practical proof |
| KC-002 | Prefer OpenClaw extension mechanisms over modifying OpenClaw core | CANDIDATE | Lower maintenance and upgrade coupling |
| KC-003 | Treat Kerani primarily as a business-specific layer on top of OpenClaw | CANDIDATE | Reconnaissance shows substantial generic infrastructure already exists upstream |
| KC-004 | Keep authoritative business storage logically separate from OpenClaw runtime/session state | CANDIDATE | Runtime/session semantics differ from authoritative business-record semantics |
| KC-005 | Do not treat OpenClaw memory as authoritative business truth | CANDIDATE | Memory is contextual/agent state, not domain approval/provenance |
| KC-006 | Do not treat an OpenClaw session as a business transaction | CANDIDATE | Session serialization does not establish business transaction semantics |
| KC-007 | Reuse OpenClaw queue/concurrency infrastructure rather than rebuilding generic SuperBasic queueing | CANDIDATE | OpenClaw already supplies runtime queue/concurrency primitives |
| KC-008 | Remove Apps Script as a required runtime if OpenClaw can host the required behaviour natively | CANDIDATE | Apps Script was an initial SuperBasic runtime boundary, not a business requirement |
| KC-009 | Remove Gemini-specific adapter ownership if OpenClaw provider abstraction satisfies Kerani requirements | CANDIDATE | Provider abstraction already exists upstream |
| KC-010 | Package KeraniClaw primarily through plugin/tool/hook/skill/external-service boundaries | CANDIDATE | Public extension surfaces appear sufficient but need prototype evidence |
| KC-011 | Treat multi-tenant/trust-boundary architecture as unresolved | RAW / UNKNOWN | Multi-agent capability alone is insufficient evidence |
| KC-012 | Maintain Kerani-specific regression tests even when OpenClaw provides runtime infrastructure | CANDIDATE | Upstream tests do not prove Kerani business behaviour |

---

## 14. Owner-approved project boundary

The following project-management facts are explicitly established by the owner:

| ID | Decision | State |
|---|---|---|
| KD-001 | `dzuddiyn/KeraniClaw-Core` is the official KeraniClaw repository and engineering source of truth for this track | LOCKED |
| KD-002 | KeraniClaw is a separate architecture experiment from the main Kerani Core track | LOCKED |
| KD-003 | Decisions do not propagate automatically between KeraniClaw and Kerani Core | LOCKED |
| KD-004 | Do not edit the main Kerani Core repository from this track unless the owner explicitly requests it | LOCKED |
| KD-005 | Avoid premature coding; architecture reconnaissance and mapping precede implementation | LOCKED |

These are project-boundary decisions, not approval of the candidate implementation architecture above.

---

## 15. Next evidence stage — not started

STEP 1 is complete enough to record candidates.

Before implementation, the next stage should test concrete extension boundaries against a minimal Kerani behavioural slice.

Potential experiment questions:

1. Can a Kerani plugin expose a structured business-validation tool without changing OpenClaw core?
2. Can a hook prevent an authoritative write when confirmation/authority evidence is missing?
3. Can clarification state survive normal session/runtime behaviour without becoming business truth?
4. Can an external authoritative business store be called idempotently through OpenClaw tools?
5. Can TEST and production business writes remain explicitly separated?
6. What tenant boundary is required for multiple mutually untrusted clients?
7. Which SuperBasic regression behaviours can be expressed unchanged over the OpenClaw runtime?

No experiment is approved merely by being listed here.

---

## 16. Change control

Significant ideas and architecture changes follow:

```text
RAW
 ↓
CANDIDATE
 ↓
TESTING
 ↓
DECIDED
 ↓
LOCKED
```

Alternative states:

```text
REJECTED
DEFERRED
SUPERSEDED
```

Rules:

- AI may propose, challenge, compare and design experiments.
- AI may not silently promote a candidate to a decision.
- Multi-model agreement is not evidence.
- Disagreement becomes a question, trade-off, experiment or candidate.
- The owner makes final architecture decisions.
- Repository evidence outranks chat memory when the repository can be inspected.
- OpenClaw documentation/source must be rechecked before important conclusions if upstream behaviour may have changed.

---

## 17. STEP 1 checkpoint

Current checkpoint:

```text
UNDERSTAND  ✓
MAP         ✓ initial
IDENTIFY GAP ✓ initial

DESIGN MINIMUM ADAPTATION  not started
TEST                       not started
IMPLEMENT                  not started
```

**Do not begin implementation from this document alone. Discuss and approve the next experiment boundary first.**
