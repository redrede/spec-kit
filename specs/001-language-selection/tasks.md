# Tasks: Multi-Language Support for Specifications

**Input**: Design documents from `/specs/001-language-selection/`
**Prerequisites**: plan.md (required), spec.md (required for user stories), research.md, data-model.md, contracts/

**Tests**: Not explicitly requested in the feature specification. Tests are omitted.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (e.g., US1, US2, US3)
- Include exact file paths in descriptions

## Path Conventions

- **CLI Source**: `src/specify_cli/__init__.py`
- **Templates**: `templates/` (source for release ZIP)
- **Deployed Templates**: `.specify/templates/` (in initialized projects)

---

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Prepare template directory structure for multi-language support

- [x] T001 Create directory structure `templates/en/` for English templates
- [x] T002 [P] Create directory structure `templates/pt-BR/` for Brazilian Portuguese templates
- [x] T003 [P] Create directory structure `templates/commands/en/` for English command prompts
- [x] T004 [P] Create directory structure `templates/commands/pt-BR/` for Portuguese command prompts
- [x] T005 [P] Create directory structure `templates/constitution/en/` for English constitution
- [x] T006 [P] Create directory structure `templates/constitution/pt-BR/` for Portuguese constitution

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Core CLI infrastructure that MUST be complete before ANY user story can be implemented

**⚠️ CRITICAL**: No user story work can begin until this phase is complete

- [x] T007 Add LANGUAGE_CHOICES constant to `src/specify_cli/__init__.py` (~line 230) with entries: `{"en": "English (Default)", "pt-BR": "Português (Brasil)"}`
- [x] T008 Add `validate_language_code(code: str) -> bool` function to `src/specify_cli/__init__.py` to check language codes against LANGUAGE_CHOICES
- [x] T009 Add `get_project_language(project_path: Path) -> str` function to `src/specify_cli/__init__.py` to read language from `.specify/config.json` with fallback to "en"

**Checkpoint**: Foundation ready - CLI has language constants and utility functions

---

## Phase 3: User Story 1 - Select Language During Project Setup (Priority: P1) 🎯 MVP

**Goal**: Enable language selection during `specify init` command

**Independent Test**: Run `specify init test-project --ai claude` and verify language selection prompt appears; verify `.specify/config.json` is created with selected language

### Implementation for User Story 1

- [x] T010 [US1] Add `--language` / `-l` option to `init()` function in `src/specify_cli/__init__.py` (~line 955) with type `str`, default `None`
- [x] T011 [US1] Add language validation in `init()` function in `src/specify_cli/__init__.py` to check `--language` flag value against LANGUAGE_CHOICES and show error for invalid codes
- [x] T012 [US1] Add language selection prompt using `select_with_arrows()` in `init()` function in `src/specify_cli/__init__.py` (~line 1060, after AI assistant selection) when `--language` flag not provided
- [x] T013 [US1] Skip language prompt in non-interactive mode (when stdin is not a TTY) and default to "en" in `src/specify_cli/__init__.py`
- [x] T014 [US1] Add config file writing after template extraction in `init()` function in `src/specify_cli/__init__.py` (~line 1130) to write `{"language": selected_language, "version": "1.0"}` to `.specify/config.json`
- [x] T015 [US1] Update success message in `init()` function in `src/specify_cli/__init__.py` to display selected language (e.g., "Language: English" or "Idioma: Português (Brasil)")

**Checkpoint**: At this point, `specify init` prompts for language and stores preference in config.json

---

## Phase 4: User Story 2 - Localized Templates (Priority: P2)

**Goal**: Provide all specification templates in the user's selected language

**Independent Test**: Run `specify init test-project --ai claude --language pt-BR` and verify that files in `.specify/templates/pt-BR/` contain Portuguese text

### Implementation for User Story 2

- [x] T016 [P] [US2] Move `templates/spec-template.md` to `templates/en/spec-template.md`
- [x] T017 [P] [US2] Move `templates/plan-template.md` to `templates/en/plan-template.md`
- [x] T018 [P] [US2] Move `templates/tasks-template.md` to `templates/en/tasks-template.md`
- [x] T019 [P] [US2] Move `templates/checklist-template.md` to `templates/en/checklist-template.md`
- [x] T020 [P] [US2] Move `templates/agent-file-template.md` to `templates/en/agent-file-template.md`
- [x] T021 [P] [US2] Create Portuguese translation `templates/pt-BR/spec-template.md` preserving all placeholders ([FEATURE NAME], $ARGUMENTS, [DATE], etc.)
- [x] T022 [P] [US2] Create Portuguese translation `templates/pt-BR/plan-template.md` preserving all placeholders and code blocks
- [x] T023 [P] [US2] Create Portuguese translation `templates/pt-BR/tasks-template.md` preserving all placeholders and task format
- [x] T024 [P] [US2] Create Portuguese translation `templates/pt-BR/checklist-template.md` preserving checkbox format
- [x] T025 [P] [US2] Create Portuguese translation `templates/pt-BR/agent-file-template.md` preserving all placeholders

**Checkpoint**: At this point, English and Portuguese template sets exist in separate directories

---

## Phase 5: User Story 3 - Localized Constitution (Priority: P3)

**Goal**: Provide constitution template in the user's selected language

**Independent Test**: Run `specify init test-project --ai claude --language pt-BR` and verify `.specify/memory/constitution.md` contains Portuguese text

### Implementation for User Story 3

- [x] T026 [P] [US3] Create English constitution template at `templates/constitution/en/constitution.md` (copy from existing `.specify/memory/constitution.md`)
- [x] T027 [P] [US3] Create Portuguese constitution template at `templates/constitution/pt-BR/constitution.md` with translated section headings and governance text

**Checkpoint**: At this point, constitution templates exist in both languages

---

## Phase 6: User Story 4 - AI-Generated Content in Selected Language (Priority: P4)

**Goal**: AI agents generate specification content in the user's selected language

**Independent Test**: Run `/speckit.specify` with Portuguese configured and verify generated spec.md content is in Portuguese

### Implementation for User Story 4

- [x] T028 [P] [US4] Move existing command files to `templates/commands/en/` directory (specify.md, plan.md, tasks.md, constitution.md, clarify.md, analyze.md, checklist.md, implement.md)
- [x] T029 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/specify.md` with language directive "Gere todo o conteúdo em Português do Brasil"
- [x] T030 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/plan.md` with language directive
- [x] T031 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/tasks.md` with language directive
- [x] T032 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/constitution.md` with language directive
- [x] T033 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/clarify.md` with language directive
- [x] T034 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/analyze.md` with language directive
- [x] T035 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/checklist.md` with language directive
- [x] T036 [P] [US4] Create Portuguese command prompt `templates/commands/pt-BR/implement.md` with language directive

**Checkpoint**: At this point, AI command prompts exist in both languages with proper language directives

---

## Phase 7: User Story 5 - Extensible Language Support (Priority: P5)

**Goal**: Design allows easy addition of new languages without code changes

**Independent Test**: Verify that adding a new language directory (e.g., `templates/es/`) with translated files makes Spanish available

### Implementation for User Story 5

- [x] T037 [US5] Add `get_template_path(template_name: str, language: str) -> Path` function to `src/specify_cli/__init__.py` with fallback logic: try language-specific path first, then fall back to English with warning
- [x] T038 [US5] Add `check_template_completeness(language: str) -> list[str]` function to `src/specify_cli/__init__.py` to return list of missing template files for a language
- [x] T039 [US5] Add fallback warning message using rich console: `"⚠ Template '{name}' not available in {language}, using English"` in `src/specify_cli/__init__.py`

**Checkpoint**: At this point, the system has extensible language infrastructure with proper fallback

---

## Phase 8: Polish & Cross-Cutting Concerns

**Purpose**: Final integration, validation, and documentation

- [x] T040 Update release build process to include language-specific template directories in ZIP assets
- [x] T041 Update `templates/` README or add CONTRIBUTING.md with instructions for adding new languages
- [x] T042 Run quickstart.md validation scenarios to verify all acceptance criteria
- [x] T043 Verify backward compatibility: test that existing projects without config.json default to English

---

## Dependencies & Execution Order

### Phase Dependencies

- **Setup (Phase 1)**: No dependencies - can start immediately
- **Foundational (Phase 2)**: Depends on Setup completion - BLOCKS all user stories
- **User Story 1 (Phase 3)**: Depends on Foundational - Core MVP, must complete first
- **User Story 2 (Phase 4)**: Depends on Foundational - Can run in parallel with US1 (different files)
- **User Story 3 (Phase 5)**: Depends on Foundational - Can run in parallel with US1/US2 (different files)
- **User Story 4 (Phase 6)**: Depends on Foundational - Can run in parallel with US1/US2/US3 (different files)
- **User Story 5 (Phase 7)**: Depends on User Story 1 completion (needs config infrastructure)
- **Polish (Phase 8)**: Depends on all user stories being complete

### User Story Dependencies

- **User Story 1 (P1)**: Can start after Foundational (Phase 2) - No dependencies on other stories
- **User Story 2 (P2)**: Can start after Foundational (Phase 2) - No dependencies on US1
- **User Story 3 (P3)**: Can start after Foundational (Phase 2) - No dependencies on US1/US2
- **User Story 4 (P4)**: Can start after Foundational (Phase 2) - No dependencies on US1/US2/US3
- **User Story 5 (P5)**: Depends on User Story 1 (needs LANGUAGE_CHOICES and config infrastructure)

### Within Each User Story

- Directory creation before file moves
- File moves before translations
- CLI code changes in logical order (constants → validation → prompts → output)
- Core implementation before integration

### Parallel Opportunities

- **Phase 1**: All directory creation tasks (T001-T006) can run in parallel
- **Phase 2**: Sequential (same file modifications)
- **Phase 3**: Sequential (same file modifications)
- **Phase 4**: All template moves (T016-T020) can run in parallel; all translations (T021-T025) can run in parallel
- **Phase 5**: Both constitution tasks (T026-T027) can run in parallel
- **Phase 6**: All command file tasks (T028-T036) can run in parallel
- **Phase 7**: Sequential (same file modifications)

---

## Parallel Example: Phase 4 (User Story 2)

```bash
# Launch all template moves together:
Task: "Move templates/spec-template.md to templates/en/spec-template.md"
Task: "Move templates/plan-template.md to templates/en/plan-template.md"
Task: "Move templates/tasks-template.md to templates/en/tasks-template.md"
Task: "Move templates/checklist-template.md to templates/en/checklist-template.md"
Task: "Move templates/agent-file-template.md to templates/en/agent-file-template.md"

# Then launch all translations together:
Task: "Create Portuguese translation templates/pt-BR/spec-template.md"
Task: "Create Portuguese translation templates/pt-BR/plan-template.md"
Task: "Create Portuguese translation templates/pt-BR/tasks-template.md"
Task: "Create Portuguese translation templates/pt-BR/checklist-template.md"
Task: "Create Portuguese translation templates/pt-BR/agent-file-template.md"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1: Setup
2. Complete Phase 2: Foundational (CRITICAL - blocks all stories)
3. Complete Phase 3: User Story 1
4. **STOP and VALIDATE**: Test `specify init` with `--language` flag
5. Language selection works, config persisted

### Incremental Delivery

1. Complete Setup + Foundational → Infrastructure ready
2. Add User Story 1 → Language selection working → MVP!
3. Add User Story 2 → Localized templates → Better UX for Portuguese users
4. Add User Story 3 → Localized constitution → Complete localized project setup
5. Add User Story 4 → AI content in Portuguese → Full native language experience
6. Add User Story 5 → Extensibility → Community can add languages

### Parallel Team Strategy

With multiple developers:

1. Team completes Setup + Foundational together
2. Once Foundational is done:
   - Developer A: User Story 1 (CLI changes)
   - Developer B: User Story 2 (Template translations)
   - Developer C: User Story 3 + 4 (Constitution + Command translations)
3. Developer A completes US5 after US1 is done
4. All stories integrate independently

---

## Notes

- [P] tasks = different files, no dependencies
- [Story] label maps task to specific user story for traceability
- Each user story should be independently completable and testable
- Commit after each task or logical group
- Stop at any checkpoint to validate story independently
- All translations must preserve placeholder tokens exactly
- Use native speaker review for Portuguese translations before release
