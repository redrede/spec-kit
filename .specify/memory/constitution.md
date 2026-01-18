<!--
========================================================================
SYNC IMPACT REPORT
========================================================================
Version change: N/A (initial) → 1.0.0
Modified principles: None (initial population)
Added sections:
  - Core Principles (5 principles)
  - Development Workflow
  - Quality Standards
  - Governance
Removed sections: None
Templates requiring updates:
  - .specify/templates/plan-template.md ✅ (already references Constitution Check)
  - .specify/templates/spec-template.md ✅ (aligned with user story focus)
  - .specify/templates/tasks-template.md ✅ (aligned with test-first and phases)
  - .specify/templates/checklist-template.md ✅ (no constitution references needed)
Follow-up TODOs: None
========================================================================
-->

# Spec Kit Constitution

## Core Principles

### I. Specification-First Development

All development in Spec Kit MUST treat specifications as the primary artifact. Code serves specifications, not the other way around.

**Non-negotiable rules:**

- Every feature MUST begin with a specification (`spec.md`) before any implementation code is written
- Specifications MUST be precise, complete, and unambiguous enough to generate working systems
- Requirements MUST use clear language: "MUST" for mandatory, "SHOULD" for recommended, "MAY" for optional
- All ambiguities MUST be marked with `[NEEDS CLARIFICATION: specific question]` and resolved before planning
- Maintaining software means evolving specifications; code is the continuously regenerated output

**Rationale:** SDD eliminates the gap between intent and implementation by making specifications executable. When specifications generate code, there is no gap—only transformation.

### II. Intent-Driven Development

Development intent MUST be expressed in natural language focused on WHAT and WHY, not HOW.

**Non-negotiable rules:**

- Feature specifications MUST focus on user needs and business value
- Technical implementation details MUST be deferred to the planning phase (`plan.md`)
- User stories MUST be independently testable and deliverable as MVP increments
- Success criteria MUST be measurable and technology-agnostic

**Rationale:** Moving the lingua franca of development to a higher level allows teams to focus on creativity, experimentation, and critical thinking while code becomes the last-mile implementation.

### III. Research-Driven Context

Technical decisions MUST be informed by systematic research, not assumptions.

**Non-negotiable rules:**

- Research agents MUST investigate library compatibility, performance benchmarks, and security implications before planning
- Every technology choice MUST have documented rationale in `research.md`
- Organizational constraints (database standards, authentication requirements, deployment policies) MUST be discovered and applied automatically
- When information is uncertain, it MUST be marked for research rather than assumed

**Rationale:** Research-driven context ensures specifications integrate real-world constraints and best practices, preventing costly pivots during implementation.

### IV. Test-First Discipline

Testing MUST be integrated into the specification process, not added after implementation.

**Non-negotiable rules:**

- Acceptance scenarios MUST be defined in specifications using Given/When/Then format
- When tests are requested, test tasks MUST be written and confirmed to FAIL before implementation begins
- Contract tests MUST be defined before endpoint implementation
- Tests MUST use realistic environments: prefer real databases over mocks, actual services over stubs

**Rationale:** Test-first thinking ensures testability and contracts are considered before implementation, leading to more robust and verifiable specifications.

### V. Simplicity and Minimalism

Solutions MUST be as simple as possible while meeting requirements.

**Non-negotiable rules:**

- Start with the minimal viable solution; add complexity only when proven necessary
- Maximum 3 projects for initial implementation; additional projects require documented justification
- Use framework features directly rather than wrapping them in custom abstractions
- No speculative or "might need" features; all features MUST trace to concrete user stories
- YAGNI (You Aren't Gonna Need It) principle MUST guide all architectural decisions

**Rationale:** Over-engineering creates maintenance burden and obscures intent. Simplicity ensures specifications remain readable and implementations remain maintainable.

## Development Workflow

### Workflow Phases

Development MUST follow these ordered phases:

1. **Constitution** (`/speckit.constitution`): Establish or review project principles
2. **Specification** (`/speckit.specify`): Define WHAT to build and WHY
3. **Clarification** (`/speckit.clarify`): Resolve ambiguities before planning
4. **Planning** (`/speckit.plan`): Define HOW to build with technology choices
5. **Task Generation** (`/speckit.tasks`): Create actionable, dependency-ordered tasks
6. **Analysis** (`/speckit.analyze`): Validate cross-artifact consistency
7. **Implementation** (`/speckit.implement`): Execute tasks according to the plan

### Artifact Hierarchy

```text
.specify/
├── memory/
│   └── constitution.md          # This file - governing principles
├── specs/
│   └── [###-feature-name]/
│       ├── spec.md              # Feature specification (Phase 2)
│       ├── plan.md              # Implementation plan (Phase 4)
│       ├── research.md          # Technical research (Phase 4)
│       ├── data-model.md        # Data model design (Phase 4)
│       ├── quickstart.md        # Validation scenarios (Phase 4)
│       ├── contracts/           # API contracts (Phase 4)
│       └── tasks.md             # Executable tasks (Phase 5)
└── templates/                   # Templates for all artifacts
```

### Branching Strategy

- Each feature MUST be developed in its own branch
- Branch naming MUST follow the pattern: `[###-feature-name]` (e.g., `001-photo-albums`)
- Feature numbers MUST be sequential and auto-incremented

## Quality Standards

### Specification Quality

- [ ] No `[NEEDS CLARIFICATION]` markers remain unresolved
- [ ] All requirements are testable and unambiguous
- [ ] Success criteria are measurable
- [ ] User stories are prioritized and independently deliverable
- [ ] Edge cases are documented

### Plan Quality

- [ ] Constitution Check passes all gates
- [ ] Technical context is complete (language, dependencies, storage, testing, platform)
- [ ] Project structure is documented with concrete paths
- [ ] Complexity is justified in tracking table (if gates violated)

### Task Quality

- [ ] Tasks are organized by user story for independent delivery
- [ ] Dependencies are explicit and respected
- [ ] Parallel tasks are marked with `[P]`
- [ ] Each phase has clear checkpoints for validation

## Governance

### Authority

This constitution supersedes all other development practices within Spec Kit projects. When conflicts arise, constitution principles take precedence.

### Compliance

- All pull requests MUST verify compliance with constitution principles
- Complexity MUST be justified; default to simplicity
- Gate violations MUST be documented with rationale

### Amendment Process

Modifications to this constitution require:

1. Explicit documentation of the rationale for change
2. Version increment following semantic versioning:
   - MAJOR: Backward-incompatible governance/principle changes
   - MINOR: New principle/section added or materially expanded
   - PATCH: Clarifications, wording, non-semantic refinements
3. Update of `LAST_AMENDED_DATE` to reflect the change date
4. Propagation check across dependent templates

### Runtime Guidance

For detailed development guidance specific to AI agents, refer to the agent configuration files (e.g., `CLAUDE.md`, `.cursorrules`) which provide operational instructions aligned with this constitution.

**Version**: 1.0.0 | **Ratified**: 2026-01-18 | **Last Amended**: 2026-01-18
