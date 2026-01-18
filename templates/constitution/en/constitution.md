# [PROJECT_NAME] Constitution

## Core Principles

### I. [PRINCIPLE_1_NAME]

[PRINCIPLE_1_DESCRIPTION]

**Non-negotiable rules:**

- [PRINCIPLE_1_RULE_1]
- [PRINCIPLE_1_RULE_2]
- [PRINCIPLE_1_RULE_3]

**Rationale:** [PRINCIPLE_1_RATIONALE]

### II. [PRINCIPLE_2_NAME]

[PRINCIPLE_2_DESCRIPTION]

**Non-negotiable rules:**

- [PRINCIPLE_2_RULE_1]
- [PRINCIPLE_2_RULE_2]
- [PRINCIPLE_2_RULE_3]

**Rationale:** [PRINCIPLE_2_RATIONALE]

### III. [PRINCIPLE_3_NAME]

[PRINCIPLE_3_DESCRIPTION]

**Non-negotiable rules:**

- [PRINCIPLE_3_RULE_1]
- [PRINCIPLE_3_RULE_2]
- [PRINCIPLE_3_RULE_3]

**Rationale:** [PRINCIPLE_3_RATIONALE]

<!--
  Add more principles as needed following the same pattern:
  ### IV. [PRINCIPLE_N_NAME]
  [PRINCIPLE_N_DESCRIPTION]
  **Non-negotiable rules:**
  - [RULE_1]
  **Rationale:** [RATIONALE]
-->

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

This constitution supersedes all other development practices within [PROJECT_NAME] projects. When conflicts arise, constitution principles take precedence.

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

**Version**: [CONSTITUTION_VERSION] | **Ratified**: [RATIFICATION_DATE] | **Last Amended**: [LAST_AMENDED_DATE]
