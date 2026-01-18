# Research: Multi-Language Support for Specifications

**Feature Branch**: `001-language-selection`
**Research Date**: 2026-01-18
**Spec**: [spec.md](./spec.md)

## Executive Summary

This research documents the technical decisions for implementing multi-language support in Spec Kit. The approach prioritizes minimal changes to the existing architecture while enabling Brazilian Portuguese support and future language extensibility.

---

## Research Area 1: Configuration Storage Mechanism

### Question
How should the user's language preference be stored at the project level?

### Decision
Use a JSON configuration file at `.specify/config.json`

### Rationale
- Python's `json` module is built-in (no new dependencies)
- JSON is human-readable and editable
- Consistent with existing patterns in the codebase (e.g., `.vscode/settings.json` handling)
- Can be easily extended with additional project-level settings in the future
- Git-friendly (can be committed and shared across team)

### Alternatives Considered

| Alternative | Rejected Because |
|-------------|------------------|
| TOML file (`.specify/config.toml`) | While `tomllib` is built-in since Python 3.11, writing TOML requires `tomli-w` (new dependency) |
| YAML file | Requires `pyyaml` dependency |
| Metadata in `constitution.md` | Mixing config with content violates separation of concerns |
| User-level config (`~/.config/specify-cli/`) | Language is project-specific, not user-specific |

### Configuration Schema
```json
{
  "language": "pt-BR",
  "version": "1.0"
}
```

---

## Research Area 2: Template Organization for Multiple Languages

### Question
How should translated templates be organized in the file system?

### Decision
Use language code subdirectories within `.specify/templates/` with English (`en/`) as the base and fallback.

### Rationale
- Clear separation of concerns
- Easy to add new languages (just add a new directory)
- Allows contributors to translate without touching code
- Fallback mechanism is straightforward: if `pt-BR/spec-template.md` doesn't exist, use `en/spec-template.md`

### Directory Structure
```text
.specify/templates/
├── en/                           # English (base/fallback)
│   ├── spec-template.md
│   ├── plan-template.md
│   ├── tasks-template.md
│   ├── checklist-template.md
│   └── agent-file-template.md
├── pt-BR/                        # Brazilian Portuguese
│   ├── spec-template.md
│   ├── plan-template.md
│   ├── tasks-template.md
│   ├── checklist-template.md
│   └── agent-file-template.md
└── commands/                     # Command instructions (language-aware)
    ├── en/
    │   ├── specify.md
    │   ├── plan.md
    │   ├── tasks.md
    │   └── ...
    └── pt-BR/
        ├── specify.md
        ├── plan.md
        ├── tasks.md
        └── ...
```

### Alternatives Considered

| Alternative | Rejected Because |
|-------------|------------------|
| Suffix naming (`spec-template-pt-BR.md`) | Harder to manage, doesn't scale well |
| Single file with language sections | Complex parsing, poor maintainability |
| Key-value translation files (gettext style) | Over-engineering for markdown templates |

---

## Research Area 3: Language Selection in CLI

### Question
How should language selection be presented to users during `specify init`?

### Decision
Reuse the existing `select_with_arrows()` function with a new language choices dictionary.

### Rationale
- Consistent user experience with existing AI assistant and script type selection
- No new code patterns to maintain
- Rich UI already provides excellent cross-platform support
- Can be skipped in non-interactive mode (similar to script type)

### Implementation Pattern
```python
LANGUAGE_CHOICES = {
    "en": "English (Default)",
    "pt-BR": "Português (Brasil)",
}

# In init() function, after ai_assistant selection:
selected_language = select_with_arrows(
    LANGUAGE_CHOICES,
    "Choose your language:",
    "en"
)
```

### CLI Flag Addition
Add `--language` / `-l` flag to allow non-interactive specification:
```bash
specify init my-project --ai claude --language pt-BR
```

---

## Research Area 4: Template Loading and Fallback

### Question
How should the system load language-specific templates and handle missing translations?

### Decision
Implement a simple path-based fallback mechanism in template loading functions.

### Rationale
- No new dependencies required
- Clear precedence: selected language → English fallback
- Warning message when fallback is used (transparency requirement from spec)

### Pseudocode
```python
def get_template_path(template_name: str, language: str) -> Path:
    """Get template path with fallback to English."""
    templates_dir = Path(".specify/templates")

    # Try language-specific template first
    lang_template = templates_dir / language / template_name
    if lang_template.exists():
        return lang_template

    # Fallback to English
    en_template = templates_dir / "en" / template_name
    if en_template.exists():
        console.print(f"[yellow]⚠ Template '{template_name}' not available in {language}, using English[/yellow]")
        return en_template

    raise FileNotFoundError(f"Template not found: {template_name}")
```

---

## Research Area 5: AI Instructions for Language Generation

### Question
How should the AI assistant be instructed to generate content in the user's selected language?

### Decision
Add language directive to command prompt files (in `.specify/templates/commands/`).

### Rationale
- AI assistants (Claude, Copilot, etc.) naturally support multiple languages
- Adding a directive at the beginning of command prompts is simple and effective
- No code changes required—just template content changes

### Implementation
Each command file (e.g., `specify.md`, `plan.md`) will include:

**English version:**
```markdown
## Language Directive
Generate all content in English. Use technical terminology appropriate for software development.
```

**Portuguese version:**
```markdown
## Diretiva de Idioma
Gere todo o conteúdo em Português do Brasil. Use terminologia técnica apropriada para desenvolvimento de software.
```

---

## Research Area 6: Constitution Localization

### Question
How should the constitution template be localized?

### Decision
Create language variants of the constitution template at `.specify/memory/constitution-{lang}.md` during project initialization.

### Rationale
- Constitution is a special file that exists in a different location than templates
- Users may customize the constitution, so we provide a localized starting point
- The `/speckit.constitution` command should respect the project language setting

### Implementation
- During `specify init`, copy the appropriate constitution template based on language
- The constitution template should be stored in the release ZIP under `templates/constitution/en/constitution.md` and `templates/constitution/pt-BR/constitution.md`
- The destination remains `.specify/memory/constitution.md` (single file, in the selected language)

---

## Research Area 7: Extensibility for Future Languages

### Question
How easy is it for contributors to add new languages?

### Decision
Document a simple process: create a new language directory and translate all template files.

### Steps to Add a New Language
1. Create directory: `.specify/templates/{lang-code}/`
2. Copy all files from `.specify/templates/en/` to the new directory
3. Translate all markdown content while preserving:
   - Placeholder tokens (e.g., `[FEATURE NAME]`, `$ARGUMENTS`)
   - Markdown structure (headings, lists, code blocks)
   - Section identifiers referenced by scripts
4. Create corresponding `commands/{lang-code}/` directory
5. Translate command prompt files
6. Add entry to `LANGUAGE_CHOICES` dictionary in CLI code
7. Create release asset: `spec-kit-template-{ai}-{script}-{lang}.zip`

### Validation Script (Future Enhancement)
A validation script could be added to verify translation completeness:
```bash
# Future: .specify/scripts/validate-translations.sh
# Checks that all files in en/ have corresponding files in target language
```

---

## Research Area 8: Release Asset Naming

### Question
How should multi-language template releases be named and distributed?

### Decision
Extend the current asset naming pattern to include language code.

### Current Pattern
```
spec-kit-template-{ai_assistant}-{script_type}.zip
# Example: spec-kit-template-claude-sh.zip
```

### New Pattern
```
spec-kit-template-{ai_assistant}-{script_type}-{language}.zip
# Example: spec-kit-template-claude-sh-pt-BR.zip
```

### Backward Compatibility
- Assets without language code default to English
- Existing assets remain valid

---

## Research Area 9: Existing Codebase Integration Points

### Analysis
Based on codebase exploration, these are the specific files and functions to modify:

| File | Function/Location | Change Required |
|------|-------------------|-----------------|
| `src/specify_cli/__init__.py` | Line ~125 | Add `LANGUAGE_CHOICES` constant |
| `src/specify_cli/__init__.py` | `init()` function (~line 945) | Add `--language` option |
| `src/specify_cli/__init__.py` | `init()` function (~line 1054) | Add language selection prompt |
| `src/specify_cli/__init__.py` | `download_and_extract_template()` | Pass language parameter |
| `src/specify_cli/__init__.py` | After extraction (~line 1127) | Write `.specify/config.json` |
| `.specify/templates/` | All template files | Reorganize into `en/` and `pt-BR/` subdirectories |
| `.specify/templates/commands/` | All command files | Reorganize into language subdirectories |
| `.specify/memory/` | `constitution.md` | Create `en/` and `pt-BR/` variants |

---

## Decisions Summary

| Area | Decision |
|------|----------|
| Config Storage | `.specify/config.json` |
| Template Organization | Language subdirectories (`en/`, `pt-BR/`) |
| CLI Selection | Reuse `select_with_arrows()` + `--language` flag |
| Fallback Mechanism | Path-based with English fallback |
| AI Language Instruction | Language directive in command prompts |
| Constitution | Single file in selected language |
| Release Naming | `{ai}-{script}-{language}.zip` |
| Extensibility | Directory-based, documented process |

---

## Next Steps

1. **Phase 1**: Implement language selection in `specify init`
2. **Phase 2**: Reorganize template directories
3. **Phase 3**: Create Brazilian Portuguese translations
4. **Phase 4**: Update AI command prompts with language directives
5. **Phase 5**: Update release build process for multi-language assets
6. **Phase 6**: Documentation for contributors

---

## References

- Spec Kit CLI source: `/src/specify_cli/__init__.py`
- Current templates: `/templates/` and `/.specify/templates/`
- Feature specification: `/specs/001-language-selection/spec.md`
- Constitution: `/.specify/memory/constitution.md`
