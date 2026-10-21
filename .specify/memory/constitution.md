<!--
Sync Impact Report
==================
Version change: 1.3.0 (proposed, unratified) → 2.0.0 (proposed, unratified)
Bump rationale: MAJOR. The project identity is redefined: the canonical name changes from
  "Servio AI" to "Servio", and the permitted identifier forms change incompatibly (`ServioAI` →
  `Servio`, `servio-ai` → `servio`, `SERVIO_AI_` → `SERVIO_`). Every artifact that uses these
  forms must be updated. The Baseline rule on unsupported framework majors is also redefined to
  allow only an approved, time-boxed security exception. No core principle is removed.

Amendments in 2.0.0 (owner decisions D45–D51, 2026-10-21; applied to the proposed text only):
  B1 Project identity: canonical name `Servio`; descriptor kept as tagline; machine forms
     `Servio`, `servio`, `SERVIO_`; `servio_ai` kept for the Postgres schema, database roles, and
     the Python import package; "Servio AI" added to the historical names
  B2 Technology Baseline, Frontend: Next.js 14.2.35 with React 18.3.1 (owner preference,
     Unsupported upstream) under time-boxed security exception X2 (ADR-017), limited to
     development and private demos; public deployment blocked until gate G-WEB and an explicit
     owner decision; unsupported majors permitted only under an approved, time-boxed exception
     recorded in an ADR
  B3 Technology Baseline, Backend runtime: Python 3.11.x (owner override 2026-10-21 of the
     earlier 3.12.15 wording); dependency resolution verified by a read-only dry run; runtime
     compatibility and exact patch unverified (ADR-022)
  B4 Technology Baseline, Model providers: DeepSeek (`deepseek-flash`) is the selected primary
     provider; OpenAI, other approved providers, local runtimes, and the mock stay configurable;
     DeepSeek adapter disabled until activation is separately approved; proposed USD 5 aggregate
     paid-testing cap; no paid key in CI, nightly evaluations, or demos (synthetic data only)
  B5 Open decisions updated; Governance records the 2026-10-21 owner decisions
Revision before ratification (2026-10-21, same version 2.0.0, still unratified): B3 changed from
  Python 3.12.15 to 3.11.x; B4 cap wording changed from "approved" to "proposed, subject to
  activation approval"; B2 scope narrowed to development and private demos.
Approval-record correction: the 1.3.0 report said amendments A1–A6 were "owner-approved for
  application". Accurate wording: A1–A6 were approved for application to the proposed text on
  2026-10-20; they were not ratified. The same applies to B1–B5.

Modified principles (titles unchanged): II (name only); no rule in any principle changed
Unchanged principles: I, III, IV, V, VI, VII, VIII, IX, X, XI, XII
Modified sections: preamble and Project identity; Technology Baseline (Frontend, Backend runtime,
  Model providers rows, unsupported-major rule, open decisions); Governance (name, recorded owner
  decisions, amendment history); version footer
Added sections: none
Removed sections: none
Unchanged: PostgreSQL remains the relational system of record and default temporal graph store;
  Neo4j remains optional behind G-NEO, an approved ADR, and explicit owner approval.
Resolved items: Python minor version (3.11.x; exact patch still open); primary LLM provider
  (embedding model still open).

Dependent templates:
  ✅ .specify/templates/plan-template.md: no edit required; its "Constitution Check" gate and
     Technical Context are derived from this file at runtime
  ✅ .specify/templates/spec-template.md: no edit required
  ✅ .specify/templates/tasks-template.md: no edit required
  ✅ .specify/templates/checklist-template.md: no edit required

Deferred items / follow-up TODOs:
  TODO(RATIFICATION_DATE): Set only when the project owner explicitly approves a final version.
    Until then the status of this document is "Proposed".
  Open decisions to be settled by ADR during master planning (see Technology Baseline).
  Exception X2 (Next.js 14.2.35) covers development and private demos only. Any public
    deployment is blocked until gate G-WEB (ADR-017) and an explicit owner decision. X2 is also
    reviewed before any real-data pilot, or when a new Critical/High advisory affecting 14.x has
    no mitigation. Mitigations are not patches.
  DeepSeek activation and the proposed USD 5 cap require explicit owner approval before any
    paid call (Principle VIII).
  Python 3.11.x runtime compatibility and the exact patch are verified in Phase 1 (ADR-022).
  Master Plan v0.4.0 (ADR-007, ADR-017, ADR-018, ADR-022) reflects B1–B4; it is Proposed.
-->

<!-- Owner brief → constitution mapping (v1.0.0 brief A–S):
  A, B, Q → I | C → II | D → III | E → IV | F → V (+ data-authority parts of J) | G, H → VI
  I → VII | J, K → VIII | L → IX | M → X | N → XI | O → XII
  P → Technology Baseline and Architecture Decisions | R → Governance
  S → Delivery Workflow and Quality Gates / Definition of Done
-->


# Servio Constitution

Servio (Verified Enterprise Reasoning & Intelligent Task Automation System) is a multi-tenant
enterprise AI platform. It combines evidence-grounded reasoning, Hybrid RAG, a temporal knowledge
graph, multi-agent orchestration, safe business automation, Market Intelligence and Idea
Intelligence, all delivered through an Arabic-first, bilingual (Arabic and English) interface.

**Project identity.** The single canonical project name is **Servio**, with the descriptor
(tagline) *Verified Enterprise Reasoning & Intelligent Task Automation System* and the Arabic
tagline *منصة الذكاء المؤسسي للاستدلال الموثق وأتمتة المهام الذكية*. The taglines are not alternative
product names. All active
documentation, configuration, metadata, prompts, and UI branding MUST use "Servio" with exactly
this spelling and capitalization. Only the forms below MAY be used, each only in its stated
context:

| Form | Context | Usage rules |
|------|---------|-------------|
| `Servio` | Human-readable name and mixed-case identifiers (e.g., code identifiers) | Required in all prose, UI branding, metadata, and prompts; also used where a mixed-case identifier is needed |
| `servio` | Identifiers that require lowercase (e.g., package, image, or project names) | Lowercase, exactly as shown. A component-specific identifier MUST begin with `servio-`; its full form is defined in the Master Plan |
| `SERVIO_` | Prefix for project-defined environment variables | Full names follow `SERVIO_<NAME>`, where `<NAME>` uses uppercase letters, digits, and underscores. Variables whose names are dictated by an external framework, library, or provider keep the name that tool requires. Frontend variables intentionally exposed to browsers use `NEXT_PUBLIC_SERVIO_<NAME>` (ADR-018). A `NEXT_PUBLIC_` variable MUST NOT carry a secret |
| `servio_ai` | Internal database and Python identifiers: the PostgreSQL schema, database roles, and the Python import package | Owner-approved internal identifier (2026-10-20, retained 2026-10-21). It is not a product name and MUST NOT appear in prose or UI branding. Derived names are defined in ADR-018 |

A context that permits none of these forms MUST use a form approved by the owner and recorded in
an ADR before first use. The approved Master Plan MUST record where each identifier form is
actually used. The former names "Servio AI" (canonical until 2026-10-21), "VERITAS AI", "AION",
and "ResolveFlow" are historical only. They MUST NOT be used as active project names, and any
retained reference to them MUST be labeled as historical.

**Normative language.** **MUST** / **MUST NOT** are mandatory requirements. A requirement can be
waived only through the exception process in Governance. **SHOULD** / **SHOULD NOT** are strong
recommendations. Any deviation from one MUST be justified in the relevant plan or ADR. **MAY**
marks a permitted option. The Core Principles are mandatory. The Technology Baseline records
choices that are changed through Architecture Decision Records (ADRs), not by amending this
constitution, unless the change would weaken a principle.

## Core Principles

### I. Specification-First, Phase-Governed Delivery

- All implementation work MUST trace to an approved specification, plan, and task list produced
  through the installed Spec Kit workflow. Work with no approved task MUST NOT be implemented.
- Each specification MUST define scope, requirements, testable acceptance criteria,
  dependencies, risks, testing strategy, and exit criteria.
- An owner-approved Master Plan (architecture, roadmap, shared contracts) MUST exist before any
  execution phase begins. The environment audit and master planning stage precedes Phase 1.
- Delivery MUST follow the roadmap in *Delivery Workflow and Quality Gates*: Phase 0 followed by
  nine execution phases. Each phase MUST have its own reviewable specification, technical plan, task breakdown, acceptance
  criteria, and tests. A phase MUST NOT begin until its specification and plan are approved.
- Phase work MUST stay consistent with the Master Plan's dependencies and shared architectural
  contracts.
- A material deviation from an approved specification or plan MUST be documented, reviewed, and
  authorized by the owner before it is implemented. If a deviation is discovered during
  implementation, the affected work MUST stop until it is resolved.
- Delivery MUST be incremental. Every required MVP module (as defined in the Master Plan) MUST
  deliver at least one complete, demonstrable, testable end-to-end workflow. Advanced
  capabilities MAY be staged. A required core module MUST NOT be removed from the MVP without
  explicit owner approval.

**Compliance:** Each plan's Constitution Check confirms that approved inputs exist. Tasks
reference the requirements they satisfy. Reviewers reject work that has no approved task.

### II. Clean-Start Modular Architecture and Explicit Boundaries

- Servio MUST be built from scratch on a clean, modular application architecture. Existing
  code, repositories, schemas, and UI components MAY be inspected only to understand the
  environment and extract ideas. They MUST NOT be assumed to be retained. Reusing any existing
  code requires explicit owner approval.
- Documentation and planning work MUST NOT delete, replace, or restructure existing application
  code.
- The initial implementation MUST be a modular monolith unless an approved ADR justifies a
  different architecture.
- These concerns MUST have explicit module boundaries and documented interfaces:
  frontend/UX; backend APIs and domain services; database and storage; AI orchestration and model
  integrations; evidence retrieval and knowledge processing; connectors and external
  integrations; security, authorization, and auditing; testing, evaluation, and infrastructure.
- A module MUST NOT reach into another module's internal storage or implementation. It MUST
  interact only through that module's documented interface.
- The frontend MUST communicate with the backend only through documented APIs. It MUST NOT
  access application databases directly, including through hosted-database client SDKs. Any
  frontend use of Supabase Auth, Supabase Storage, or another provider SDK MUST be defined in
  an approved ADR and MUST NOT bypass backend authorization.
- Business rules MUST be enforced in backend domain services. UI components and prompts MAY
  reflect rules but MUST NOT be their only enforcement point.
- Circular dependencies and duplicated business logic MUST NOT be introduced. New services,
  microservices, or infrastructure components MUST be justified by a documented need in an ADR.

**Compliance:** Plans identify the owning module for each change and its dependency direction.
New infrastructure without an approved ADR is a constitution violation.

### III. Tenant Isolation and Least Privilege

- Real multi-tenancy is mandatory from the first release. Single-tenant shortcuts intended for
  later retrofit MUST NOT be used.
- Every tenant-owned resource and operation MUST have a defined owning tenant and explicit
  authorization rules.
- Tenant isolation MUST cover PostgreSQL data, pgvector embeddings, Neo4j nodes and
  relationships (if Neo4j is adopted), files and object storage, connectors and their credentials, background jobs,
  caches, agent state and checkpoints, evaluation data, audit records, logs and traces, and usage
  and cost data.
- Tenant isolation MUST be enforced at both the application layer and the database layer.
  Application checks alone are not sufficient. The implementation mechanism is subject to an
  approved ADR.
- The server MUST derive tenant context and validate it against the authenticated identity and
  that identity's authorized membership. A client-supplied tenant identifier MUST NOT be treated
  as sufficient authorization.
- Platform administration MUST be a separate trust domain from tenant administration, with its
  own identities, roles, permissions, and audit records. A platform role MUST NOT grant access to
  tenant content.
- Platform administration MUST require multi-factor authentication.
- Platform staff MAY access tenant content only through a support grant that is requested with a
  stated reason, approved by the tenant owner or an authorized tenant administrator, read-only by
  default, limited to at most four hours, revocable, and fully recorded in the tenant's audit
  records. A scope broader than read-only requires an approved ADR and the tenant's explicit
  approval for that grant. Access under a grant MUST use a validated tenant context and the
  ordinary tenant authorization path.
- No emergency, break-glass, or other path MAY bypass tenant approval of support access. Adding
  such a path requires a constitution amendment.
- Retrieval, vector, and graph queries MUST be tenant-scoped when executed. Filtering
  cross-tenant results after retrieval MUST NOT be the only control.
- Authorization MUST be role-based. Users, service accounts, connectors, agents, and tools MUST
  receive least-privilege permissions.
- Automated negative tests MUST cover cross-tenant access attempts for every tenant-owned
  resource type and every API or tool that touches one.

**Compliance:** Each specification lists the tenant-owned resources it introduces. Missing
isolation tests block completion. Platform negative tests cover every tenant-content route with
no grant, an expired grant, and a revoked grant.

### IV. Evidence-Grounded Reasoning

- Material factual claims and business recommendations MUST be supported by cited, relevant
  evidence wherever evidence applies.
- Evidence MUST preserve provenance: source system, record identifier, passage or span reference,
  relevant timestamps, freshness, and access restrictions.
- Outputs MUST distinguish verified facts, inferences, assumptions, recommendations, unknown
  information, conflicting evidence, and confirmed execution results.
- Citations MUST NOT be fabricated. An output MUST NOT claim that evidence supports a conclusion
  the evidence does not support. Citations MUST resolve only to evidence the requesting user is
  authorized to access.
- When evidence is insufficient, the system MUST request clarification, gather further evidence,
  or escalate. It MUST NOT fill the gap with unsupported assertions.

**Compliance:** Citation correctness and unsupported-claim rate are mandatory AI evaluation
metrics (Principle X).

### V. Conflict-Aware Temporal Knowledge

- The knowledge architecture MUST represent conflicting sources, data freshness, historical
  records, and temporal relationships. Conflicting or superseded facts MUST NOT be silently
  overwritten.
- Event time MUST be distinguished from record-creation time and ingestion time wherever the
  difference affects reasoning.
- Conflicts MUST be resolved by documented source-authority rules, business policies,
  verification status, temporal context, and evidence quality. Unresolved conflicts MUST be
  surfaced, not hidden.
- PostgreSQL is the relational system of record and the default store for the temporal knowledge
  graph unless an approved ADR establishes an exception.
- pgvector embeddings, and Neo4j graph data if Neo4j is adopted, are derived data. Each derived
  store MUST have documented synchronization, freshness, and reconciliation rules. Each SHOULD be
  rebuildable from the system of record.

**Compliance:** Specifications that touch knowledge data state their source-authority and
reconciliation rules. Conflict detection is an evaluated metric.

### VI. Safe Actions, Human Approval, and Verified Outcomes

- Business actions MUST execute only through validated, permission-checked backend services or
  tools. A model MUST NOT grant itself permissions or bypass application authorization.
  Permissions come from the authenticated principal and from policy, never from model output.
- Every action type MUST have a policy-assigned risk classification. Only low-risk actions
  explicitly permitted by policy MAY execute automatically. Sensitive, restricted,
  consequential, or irreversible actions MUST obtain the applicable human approval. Unclassified
  actions MUST be treated as requiring approval.
- An approval record MUST be bound to the tenant, the requesting actor, the approver, the exact
  action parameters, the supporting evidence, and the applicable policy. Any change to the
  parameters invalidates the approval, and an approval MUST NOT be reused for a different action.
- Before execution, the system MUST verify permissions, evidence, policy compliance, parameters,
  and approval state.
- Mutating operations MUST address idempotency and duplicate execution.
- Outcomes MUST be verified through an authoritative response, a read-after-write check, or
  another documented mechanism. An agent's statement that an action succeeded is not proof of
  success.
- Designs MUST handle timeouts, retries, duplicate requests, partial failures, unknown outcomes,
  and cancellation. They MUST provide recovery or compensation where feasible. Unknown outcomes
  MUST be recorded as unknown and reconciled, not blindly retried.
- An unverified or failed operation MUST NOT be reported as a confirmed success. Action states
  MUST distinguish at least: pending approval, executed but unverified, verified success,
  failed, and unknown.

**Compliance:** Every action-capable feature specifies its risk class, approval policy,
idempotency strategy, verification mechanism, and failure handling, and tests each of them.

### VII. Bounded Multi-Agent Orchestration

- LangGraph is the primary orchestration framework. LangChain MAY be used where it provides
  practical value.
- Every agent or workflow node MUST have a single clear responsibility, schema-validated inputs
  and outputs, explicitly granted tool and data permissions, and measurable evaluation criteria.
- Agent state MUST be validated. State persistence (including checkpoints) MUST be tenant-aware.
- Every workflow MUST define bounded retries, timeouts, maximum steps or iterations, token and
  cost budgets, cancellation, and failure handling. Unbounded loops are prohibited.
- Side-effecting tool calls MUST go through the Principle VI controls. They MUST NOT be repeated
  unintentionally on retry, replay, or resume.
- Agents MUST access data and execute actions only through controlled interfaces. Agents MUST
  NOT have unrestricted production database access or direct access to credentials.
- Agent runs MUST be traceable end to end (Principle XI).

**Compliance:** Plans list each agent and node with its responsibility, permissions, limits,
and evaluation criteria.

### VIII. Replaceable Providers, Local-First Development, and Free-Tier-First Cost Governance

- Model access MUST go through a provider abstraction with configurable providers: DeepSeek,
  OpenAI, other cloud providers approved by the owner, applicable local runtimes, and a
  deterministic mock provider. Abstractions MUST be proportional to real requirements, not
  speculative.
- Every configured provider MUST declare its data-handling properties, including whether inputs
  may be used for training or service improvement, data retention, and processing location. An
  undeclared property MUST be treated as its least favorable value.
- Every tenant MUST carry a data classification. Provider routing MUST refuse any provider whose
  declared data handling is incompatible with the tenant's classification. Non-synthetic data
  MUST NOT be sent to a provider whose terms the owner has not approved.
- The deterministic mock provider MUST support every required workflow with no external LLM, so
  the system remains demonstrable when external providers are unavailable, exhausted, or not
  approved.
- Paid model usage MUST NOT begin without explicit owner approval that states its scope and
  spending cap.
- Unnecessary vendor-specific coupling SHOULD be avoided. Supabase is the default for
  authentication and storage, and MAY provide hosted PostgreSQL where appropriate. Adopting any
  provider-specific feature that would block replacement MUST be recorded in an ADR.
- Delivery MUST be local-first. Docker-based local development comes first, followed by a
  cloud-hosted demo, with a production-ready deployment path defined by ADR.
- Required workflows MUST run without a GPU, and hardware and model choices MUST support
  CPU-only development. GPU acceleration is optional and MAY be used when available.
- Local development and the initial hosted demo MUST NOT require paid services without explicit
  owner approval. Open-source software, local services, synthetic datasets, mock connectors, and
  verified free tiers SHOULD be preferred.
- Every significant external dependency MUST document its costs, limits and quotas, required
  credentials, local alternative, failure behavior, and replacement strategy. Free-tier terms
  MUST be verified when the dependency is adopted and MUST NOT be assumed unlimited or permanent.
- Usage and estimated cost SHOULD be tracked. Provider failures and exhausted quotas MUST
  degrade gracefully and visibly.
- Cost reduction MUST NOT override security, tenant isolation, evidence integrity, or action
  safety.

**Compliance:** Dependency documentation exists before a dependency is merged. Significant cost
commitments follow Governance approval.

### IX. Security, Privacy, and Secret Management

- Specifications and plans MUST address the threats relevant to their scope: authentication,
  authorization, prompt injection, malicious retrieved content, insecure file access, data
  leakage, unauthorized tool execution, approval tampering, and resource exhaustion.
- All untrusted input MUST be validated. Retrieved documents, connector data, external
  responses, and model outputs MUST be treated as untrusted data. Untrusted content MUST NOT be
  executed as instructions or alter permissions, policy, or approval state.
- Secrets MUST NOT be hardcoded, committed, exposed in frontend bundles, or written to logs.
  Local development MUST load secrets from environment variables. Deployed environments MUST use
  secure secret management. Example configuration MUST use placeholders only.
- Sensitive data MUST be minimized and redacted in logs, traces, prompts, and evaluation
  artifacts.
- Access control, retention, deletion, redaction, and tenant-offboarding practices MUST be
  defined. Offboarding MUST cover derived stores (vectors, graph, caches, files).
- Security exceptions require explicit owner approval.

**Compliance:** Each phase completes a threat review for its scope before exit, and its
findings are tracked to resolution.

### X. Testability and Measurable AI Quality

- Every significant feature MUST have testable acceptance criteria.
- Tests MUST include, as applicable: unit, integration, end-to-end, security, tenant-isolation,
  failure-recovery, and AI regression tests. Plans MUST state which test types apply and why.
- AI evaluation MUST measure, where relevant: retrieval quality, citation correctness,
  unsupported claims, conflict detection, policy compliance, action correctness, approval
  compliance, verification success, resolution success, latency, and cost. Thresholds are set in
  the phase specification or plan and approved with it.
- AI regression evaluation MUST run when prompts, models, retrieval configuration, or agent
  graphs change.
- Core development and evaluation MUST use reproducible, versioned synthetic datasets and
  fixtures by default. Real customer data MUST NOT be used without explicit owner approval and
  appropriate controls.
- Code that merely executes is not complete (see Definition of Done).

**Compliance:** Evaluation results are recorded with the dataset and model versions used.

### XI. Observability and Auditability

- Significant workflows MUST be traceable end to end with correlation identifiers. A trace
  spans the request, evidence retrieval, model calls, tool calls, approvals, actions, and
  verification results.
- The system MUST emit structured logs, metrics, and traces attributed to the relevant tenant.
- Approvals, actions, permission and policy changes, and security-relevant events MUST produce
  tenant-scoped, tamper-evident audit records.
- Telemetry MUST NOT expose secrets or unnecessary sensitive data (Principle IX).
- Operational limitations, migration requirements, recovery behavior, and reproducibility
  expectations MUST be documented.

**Compliance:** Reviews of action-capable or AI workflows verify trace and audit coverage.

### XII. Arabic-First, Bilingual, Right-to-Left Experience

- Arabic MUST be the primary and default language, with correct, consistent right-to-left
  layout across navigation, layouts, forms, tables, charts, and workflow views.
- English MUST be supported alongside Arabic in the first release. The Master Plan MUST account
  for this bilingual first-release scope.
- User-facing strings MUST be externalized. Layouts MUST use direction-aware styling so Arabic
  renders right-to-left and English renders left-to-right without restructuring.
- The interface MUST clearly communicate missing evidence, uncertainty, conflicting evidence,
  pending approvals, failures, unverified outcomes, and degraded service states. It MUST reflect
  the distinctions required by Principles IV and VI.
- Acceptance tests for user-facing features MUST verify RTL rendering and mixed Arabic and
  English content, covering text, numbers, dates, identifiers, forms, tables, charts, and
  navigation.

**Compliance:** UI specifications for first-release features include Arabic and English
content, RTL acceptance criteria for Arabic, and LTR acceptance criteria for English.

## Technology Baseline and Architecture Decisions

### Baseline

These are owner-agreed architectural defaults. They do not authorize implementation, which
still requires the approvals in Principle I. A default MUST NOT be silently substituted. Any
change requires an approved ADR.

| Area | Default | Status |
|------|---------|--------|
| Specification framework | Spec Kit (installed v1.0.1) | Mandatory (Principle I) |
| Architecture style | Clean-start modular monolith | Default; deviation by approved ADR |
| Frontend | Next.js **14.2.35** with React 18.3.1 and TypeScript | Owner preference (2026-10-21); the preference does not make the version supported or secure. Next.js 14.x is Unsupported upstream and has known advisories with no 14.x patch. It is permitted only under the time-boxed security exception X2 recorded in ADR-017, **for development and private demos only**, with that ADR's mandatory mitigations, which are not patches. **Public deployment MUST NOT occur** until the security feasibility gate G-WEB (ADR-017) is reported and the owner decides explicitly. Exact versions are pinned in the lockfile (ADR-022). The version MUST NOT change silently; another version or major MUST NOT be adopted without explicit owner approval. Support status and advisories are re-checked at each phase exit |
| Backend | Python with FastAPI | Agreed default |
| Backend runtime | Python 3.11.x | Owner decision (2026-10-21; supersedes the same-day 3.12.15 wording). A read-only dependency-resolution dry run for 3.11 succeeded on 2026-10-21 (ADR-022). Runtime compatibility is **unverified**, and the exact patch is selected only after a Phase 1 smoke test. Security-only upstream, with end of support in 2027-10 |
| Background jobs | PostgreSQL-backed job queue inside the system-of-record database; no separate message broker | Agreed default; queue library chosen by ADR |
| Database migrations | Supabase CLI plain-SQL migrations, version-controlled, with database tests | Agreed default |
| Authentication | Supabase Auth | Agreed default, subject to security review and ADR |
| Relational system of record | PostgreSQL | Agreed default; exception by approved ADR |
| Vector search | pgvector, where appropriate | Agreed default |
| File storage | Supabase Storage, where appropriate | Agreed default |
| Temporal knowledge graph | Modeled in PostgreSQL, the authoritative system of record | Agreed default |
| Neo4j | Optional derived graph store | Adopted only after the G-NEO comparison, an approved ADR, and explicit owner approval. No MVP capability, test, or exit criterion may depend on it |
| Agent orchestration | LangGraph (LangChain where useful) | Agreed default |
| Model providers | Provider abstraction: DeepSeek, OpenAI, other owner-approved cloud providers, local runtimes, deterministic mock | **DeepSeek (`deepseek-flash`) is the selected primary provider** (owner decision, 2026-10-21). Its adapter stays disabled until billing, data handling, processing location, and privacy requirements are reviewed and the owner separately authorizes activation. The proposed aggregate paid-testing cap is USD 5, with no auto-recharge, synthetic data only, and paid use only in authorized local development and evaluation runs; CI, nightly evaluations, and demos use the mock or permitted free tiers (ADR-007). Other providers stay configurable; adding or switching a primary needs an ADR and owner approval |
| Observability | OpenTelemetry traces with correlation identifiers; structured, redacted logs | Agreed default |
| Secrets | Environment variables locally; secure secret management when deployed | Mandatory (Principle IX) |
| Local development and deployment | Docker-based local development; production-ready deployment path | Agreed default; deployment target by ADR |

A framework or runtime major that is unsupported upstream MUST NOT be used, except under an
approved, time-boxed security exception recorded in an ADR that states its mitigations and
review triggers (Principle IX).

Every schema change MUST be a reviewed, version-controlled migration. Manual schema changes to
shared or hosted databases MUST NOT be made. Migrations MUST preserve tenant-isolation controls
and MUST NOT contain secrets or real customer data.

**Open decisions.** The following are NOT decided by this constitution. They MUST be resolved
through ADRs approved during master planning (or in the phase that first needs them): approval
of the pinned framework and library versions (ADR-022), the application- and database-layer
tenant-isolation mechanism, the job-queue library, cloud demo and production hosting targets,
deployed secret manager, embedding model, local model runtime, the G-NEO outcome (Neo4j
adoption, edition, and licence), and the ADR storage location.

### Architecture Decision Records

- Every material architectural decision MUST be recorded in an ADR. An ADR states context,
  decision, alternatives considered, rationale, costs, trade-offs, consequences, reconsideration
  triggers, and approval status.
- ADRs MUST be stored in a single location defined by the approved Master Plan.
- Artifacts MUST remain consistent in this order of precedence: Constitution → Master Plan →
  ADRs → phase specifications → plans → tasks → implementation. When artifacts conflict, the
  lower artifact MUST be corrected or a documented exception MUST be approved.
- Architectural changes MUST follow documented review and owner approval before implementation.

## Delivery Workflow and Quality Gates

### Roadmap

The pre-execution stage, **Phase 0: Environment Audit and Master Planning**, produces the
owner-approved Master Plan, shared contracts, and initial ADRs. Delivery then follows nine
execution phases. Each phase is a vertical slice that delivers the bilingual user interface for
the capabilities it builds:

1. Secure Multi-Tenant Foundation, Platform Identity, and Bilingual Shell
2. Connectors, Data Foundation, and Customer 360
3. Evidence, Hybrid RAG, and Temporal Knowledge Graph
4. Case Intelligence and Agent Workflow
5. Safe Actions and Approvals
6. Supervisor Console and Platform Admin Console
7. Market Intelligence
8. Idea Intelligence
9. Evaluation Lab, Security Hardening, and Hosted Demo

- User-facing capabilities MUST be delivered in the phase that builds them. Frontend work MUST
  NOT be deferred to a single late phase.
- The first release is bilingual (Principle XII). Every phase that delivers user-facing work MUST
  include Arabic (RTL) and English (LTR) acceptance criteria.
- The Master Plan defines phase dependencies, the per-module MVP minimum, and the approved MVP
  deferral lists. Reordering, merging, or splitting phases, or moving any item between MVP and
  deferred, is a Master Plan change that requires owner approval.
- A phase MUST pass its exit gate before any phase that depends on it begins.

### Phase Gates

1. **Entry:** The phase specification and plan are owner-approved, and prerequisite phases have
   exited or have approved partial dependencies.
2. **Specification:** It defines scope, dependencies, acceptance criteria, testing strategy,
   risks, and exit criteria.
3. **Plan:** It passes the Constitution Check against every principle before research and again
   after design. Each violation is either fixed or listed in Complexity Tracking with an
   approved exception.
4. **Tasks:** Every task traces to a requirement. Cross-artifact consistency is verified
   (e.g., `/speckit-analyze`) before implementation.
5. **Exit:** The Definition of Done is met and the owner accepts the phase.

### Definition of Done

A feature or phase is complete only when all of the following hold:

- Applicable requirements and acceptance criteria are satisfied.
- Required tests pass, including tenant-isolation and failure-recovery tests where applicable.
- Security checks for the scope are completed (Principle IX).
- Relevant AI evaluation has been run and its results recorded against approved thresholds.
- Documentation, ADRs, and operational notes are updated.
- Known critical regressions are resolved or explicitly accepted through the exception process.
- For a phase: owner acceptance is recorded.

## Governance

**Authority.** This constitution governs all specifications, plans, tasks, ADRs, implementation,
testing, security reviews, and deployment activities for Servio. It supersedes conflicting
practices and documents. The project owner retains authority over product scope, major
architectural choices, security exceptions, significant cost commitments, constitution changes,
and phase acceptance.

**Approval.** Approval MUST be explicit and recorded. Silence, elapsed time, or the existence of
a plan, draft, or pull request does not constitute approval. AI assistants MUST NOT grant
approval on the owner's behalf. Material changes to requirements, authorization policies, shared
contracts, or architecture MUST be reviewed, documented, and approved before implementation.

**Compliance review.** Every specification, plan, and task list MUST be checked for conflicts
with this constitution. Violations MUST be identified and resolved, either by correcting the work
or through an approved exception. They MUST NOT be silently ignored. Code and design reviews MUST
verify compliance for the principles in scope.

**Exceptions.** An exception MUST document the affected principle(s), scope, duration or revisit
trigger, impact, risk, mitigation, and explicit owner approval. It MUST be recorded in the
relevant plan's Complexity Tracking table or in an ADR.

**Amendments.** Amendments MUST be made through the Spec Kit constitution workflow
(`/speckit-constitution`). Each amendment MUST state its rationale, affected principles or
sections, compatibility and migration implications for existing artifacts, and approval status.
It MUST also update the Sync Impact Report and the amendment history below. In-flight
specifications and plans MUST be re-checked against an amended constitution.

**Versioning.** The constitution uses semantic versioning. A MAJOR increment marks a
backward-incompatible removal or redefinition of a principle or of governance. A MINOR increment
marks a new principle or section, or materially expanded guidance. A PATCH increment marks a
clarification or wording fix that does not change meaning.

**Recorded owner decisions.** These explicit owner decisions are incorporated in this proposed
version. Recording them does not ratify the constitution.

| Date | Decision | Where applied |
|------|----------|---------------|
| 2026-10-19 | Canonical project name is `Servio AI` (**superseded 2026-10-21**) | Project identity |
| 2026-10-19 | `servio-ai` for lowercase machine identifiers; `SERVIO_AI_` as the environment-variable prefix (**superseded 2026-10-21**) | Project identity |
| 2026-10-19 | English supported in the first release alongside Arabic; Arabic remains default with correct RTL; Master Plan accounts for bilingual first-release scope | Principle XII; Roadmap |
| 2026-10-19 | Tenant isolation enforced at application and database layers; mechanism by approved ADR | Principle III |
| 2026-10-19 | Required workflows run without a GPU; GPU acceleration optional | Principle VIII |
| 2026-10-20 | Canonical name remains `Servio AI`; "VERITAS AI" remains historical (**name superseded 2026-10-21**) | Project identity |
| 2026-10-20 | Amendments A1–A6 approved for application to the proposed text with the reviewed wording; not ratified (A4 superseded 2026-10-21) | Roadmap; Principles I, III, V, VIII; Technology Baseline |
| 2026-10-20 | No emergency or break-glass path may bypass tenant approval of support access; adding one requires a constitution amendment | Principle III |
| 2026-10-20 | Support grants: read-only by default, at most four hours, approved by the tenant owner or an authorized tenant administrator, audited; MFA for platform administration | Principle III |
| 2026-10-21 | Canonical name `Servio`; machine forms `Servio`, `servio`, `SERVIO_`; `servio_ai` kept for the Postgres schema, database roles, and the Python import package; descriptor kept as the tagline; "Servio AI" is historical | Project identity |
| 2026-10-21 | Next.js 14.2.35 with React 18.3.1 kept through development under time-boxed security exception X2 (ADR-017), development and private demos only; public deployment only after gate G-WEB and an explicit owner decision; no other version or major without explicit owner approval | Technology Baseline |
| 2026-10-21 | Python 3.11.x (supersedes the same-day 3.12.15 wording); dependency resolution dry run succeeded; runtime and exact patch unverified (ADR-022) | Technology Baseline |
| 2026-10-21 | DeepSeek (`deepseek-flash`) is the selected primary LLM provider; adapter disabled until activation is separately approved; proposed USD 5 aggregate paid-testing cap, no auto-recharge, synthetic data only; no paid key in CI, nightly evaluations, or demos (ADR-007) | Technology Baseline; Principle VIII |
| 2026-10-21 | Browser-exposed frontend variables use `NEXT_PUBLIC_SERVIO_<NAME>` and never carry secrets; the platform database role is `servio_ai_platform_app`, distinct from the `servio_ai_platform` schema (derived names Proposed in ADR-018) | Project identity |
| 2026-10-21 | PostgreSQL remains the primary system of record; Neo4j remains optional behind G-NEO, an approved ADR, and explicit owner approval (reaffirmed, unchanged) | Principle V; Technology Baseline |

**Amendment history.**

| Version | Date | Summary | Approval status |
|---------|------|---------|-----------------|
| 1.0.0 | 2026-10-19 | Initial constitution created from the owner brief (principles A–S) | Proposed; superseded by 1.1.0 before ratification |
| 1.1.0 | 2026-10-19 | Canonical name set to Servio AI; clean-start rule; agreed technology defaults; local-first, CPU-only, bilingual, RBAC, and database-layer isolation rules | Proposed; superseded by 1.2.0 before ratification |
| 1.2.0 | 2026-10-19 | Recorded owner decisions: exact identifier forms (`servio-ai`, `SERVIO_AI_`); English required in the first release; confirmed name, two-layer tenant isolation, and no-GPU requirement | Proposed; superseded by 1.3.0 before ratification |
| 1.3.0 | 2026-10-20 | Amendments A1–A6: vertical-slice roadmap (Phase 0 + 9 phases); provider data controls and mock fallback; Baseline rows for Python 3.12, job queue, Supabase CLI migrations, OpenTelemetry; supported Next.js LTS; PostgreSQL-first temporal graph with optional Neo4j; platform trust boundary, MFA, support grants, no break-glass | Proposed; superseded by 2.0.0 before ratification |
| 2.0.0 | 2026-10-21 | B1–B5: canonical name Servio and new identifier forms (`servio_ai` retained); Next.js 14.2.35 + React 18.3.1 under exception X2 (development and private demos; public deployment gated by G-WEB); Python 3.11.x (revised before ratification from 3.12.15); DeepSeek primary with a proposed USD 5 cap subject to activation approval; open decisions updated; approval-record wording corrected | Proposed; awaiting owner ratification |

**Status**: Proposed. Not ratified; awaiting explicit owner approval.

**Version**: 2.0.0 | **Ratified**: TODO(RATIFICATION_DATE): set on explicit owner approval | **Last Amended**: 2026-10-21
