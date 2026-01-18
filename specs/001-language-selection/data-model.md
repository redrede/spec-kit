# Data Model: Multi-Language Support for Specifications

**Feature Branch**: `001-language-selection`
**Date**: 2026-01-18
**Spec**: [spec.md](./spec.md)

---

## Overview

This feature introduces minimal data structures for language configuration. Since Spec Kit is a CLI tool that operates on files (not a database-backed application), the data model focuses on configuration files and directory structures.

---

## Entity: Project Configuration

**File**: `.specify/config.json`
**Purpose**: Stores project-level settings including language preference

### Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Spec Kit Project Configuration",
  "type": "object",
  "properties": {
    "language": {
      "type": "string",
      "description": "ISO language code for the project",
      "enum": ["en", "pt-BR"],
      "default": "en"
    },
    "version": {
      "type": "string",
      "description": "Configuration schema version",
      "pattern": "^\\d+\\.\\d+$",
      "default": "1.0"
    }
  },
  "required": ["language", "version"],
  "additionalProperties": false
}
```

### Example Instance

```json
{
  "language": "pt-BR",
  "version": "1.0"
}
```

### Validation Rules

| Field | Rule | Error Behavior |
|-------|------|----------------|
| `language` | Must be a supported language code | Fall back to `"en"` with warning |
| `version` | Must match semantic versioning pattern | Ignore and process as current version |

### State Transitions

```text
[No config file] → (specify init with language) → [Config with language]
[Config with language] → (manual edit) → [Config with new language]
[Corrupted config] → (any speckit command) → [Warning + fallback to en]
```

---

## Entity: Language Choices

**Location**: In-memory constant in `src/specify_cli/__init__.py`
**Purpose**: Defines available languages for selection

### Structure

```python
LANGUAGE_CHOICES: Dict[str, str] = {
    "en": "English (Default)",
    "pt-BR": "Português (Brasil)",
}
```

### Extension Pattern

To add a new language, add an entry to this dictionary:
```python
LANGUAGE_CHOICES: Dict[str, str] = {
    "en": "English (Default)",
    "pt-BR": "Português (Brasil)",
    "es": "Español",  # Future addition
}
```

---

## Entity: Template Set

**Location**: `.specify/templates/{language}/`
**Purpose**: Contains all markdown templates for a specific language

### Directory Structure

```text
.specify/templates/
├── en/
│   ├── spec-template.md
│   ├── plan-template.md
│   ├── tasks-template.md
│   ├── checklist-template.md
│   └── agent-file-template.md
└── pt-BR/
    ├── spec-template.md
    ├── plan-template.md
    ├── tasks-template.md
    ├── checklist-template.md
    └── agent-file-template.md
```

### Template File Requirements

Each template file MUST:
- Preserve all placeholder tokens (e.g., `[FEATURE NAME]`, `$ARGUMENTS`, `[DATE]`)
- Maintain the same markdown structure (heading levels, sections)
- Preserve any code blocks and their content unchanged
- Translate only human-readable text

### Completeness Check

A template set is considered complete when all files from `en/` have corresponding translations.

| Template File | Required | Fallback Behavior |
|---------------|----------|-------------------|
| `spec-template.md` | Yes | Use English with warning |
| `plan-template.md` | Yes | Use English with warning |
| `tasks-template.md` | Yes | Use English with warning |
| `checklist-template.md` | Yes | Use English with warning |
| `agent-file-template.md` | Yes | Use English with warning |

---

## Entity: Command Instructions

**Location**: `.specify/templates/commands/{language}/`
**Purpose**: Contains AI command prompts for each language

### Directory Structure

```text
.specify/templates/commands/
├── en/
│   ├── specify.md
│   ├── plan.md
│   ├── tasks.md
│   ├── constitution.md
│   ├── clarify.md
│   ├── analyze.md
│   ├── checklist.md
│   └── implement.md
└── pt-BR/
    ├── specify.md
    ├── plan.md
    ├── tasks.md
    ├── constitution.md
    ├── clarify.md
    ├── analyze.md
    ├── checklist.md
    └── implement.md
```

### Command File Requirements

Each command file MUST:
- Include the language directive section at the top
- Preserve all technical instructions for AI agents
- Translate user-facing output descriptions
- Maintain placeholder syntax

---

## Entity: Constitution Template

**Location**: `.specify/memory/constitution.md` (deployed)
**Source**: `templates/constitution/{language}/constitution.md` (in release)
**Purpose**: Project governance document in selected language

### File Placement

During `specify init`:
1. Read language preference from user selection
2. Copy `constitution/{language}/constitution.md` to `.specify/memory/constitution.md`
3. The deployed file is a single file in the selected language

### Versioning

Constitution files maintain their own version:
```text
**Version**: 1.0.0 | **Ratified**: 2026-01-18 | **Last Amended**: 2026-01-18
```

Translation does not change the constitution version; only content amendments do.

---

## Relationships

```text
┌─────────────────────────────────────────────────────────────────┐
│                     Project Configuration                        │
│                    (.specify/config.json)                        │
│                                                                  │
│  ┌──────────────┐                                               │
│  │  language    │────────────────────────────────────┐          │
│  │  "pt-BR"     │                                    │          │
│  └──────────────┘                                    │          │
└──────────────────────────────────────────────────────│──────────┘
                                                       │
                                                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                     Template Selection                           │
│                                                                  │
│   Language Code ──────► .specify/templates/{lang}/               │
│                         .specify/templates/commands/{lang}/      │
│                                                                  │
│   Fallback ──────────► .specify/templates/en/                   │
│                         .specify/templates/commands/en/          │
└─────────────────────────────────────────────────────────────────┘
```

---

## Data Flow

### Project Initialization

```text
User                    CLI                    File System
  │                      │                         │
  │ specify init         │                         │
  │ ──────────────────►  │                         │
  │                      │                         │
  │ ◄──────────────────  │                         │
  │ Language prompt      │                         │
  │                      │                         │
  │ Select "pt-BR"       │                         │
  │ ──────────────────►  │                         │
  │                      │                         │
  │                      │ Download template ZIP   │
  │                      │ (with pt-BR templates)  │
  │                      │ ─────────────────────►  │
  │                      │                         │
  │                      │ Extract templates       │
  │                      │ ─────────────────────►  │
  │                      │                         │
  │                      │ Write config.json       │
  │                      │ {"language": "pt-BR"}   │
  │                      │ ─────────────────────►  │
  │                      │                         │
  │ ◄──────────────────  │                         │
  │ Success message      │                         │
```

### Template Loading

```text
AI Agent                 Scripts               File System
  │                        │                       │
  │ /speckit.specify       │                       │
  │ ─────────────────►     │                       │
  │                        │                       │
  │                        │ Read config.json      │
  │                        │ ─────────────────►    │
  │                        │ ◄─────────────────    │
  │                        │ {language: "pt-BR"}   │
  │                        │                       │
  │                        │ Load template         │
  │                        │ templates/pt-BR/      │
  │                        │ spec-template.md      │
  │                        │ ─────────────────►    │
  │                        │ ◄─────────────────    │
  │                        │ Template content      │
  │                        │                       │
  │ ◄─────────────────     │                       │
  │ Template in Portuguese │                       │
```

---

## Migration & Backward Compatibility

### Existing Projects

Projects created before this feature will not have:
- `.specify/config.json` file
- Language subdirectories in templates

**Behavior**: System treats missing config as `language: "en"` (English default).

### Config File Absence

```python
def get_project_language(project_path: Path) -> str:
    config_file = project_path / ".specify" / "config.json"
    if not config_file.exists():
        return "en"  # Default fallback

    try:
        config = json.loads(config_file.read_text())
        return config.get("language", "en")
    except (json.JSONDecodeError, KeyError):
        console.print("[yellow]⚠ Invalid config file, using English[/yellow]")
        return "en"
```

---

## Validation Functions

### Config Validation

```python
def validate_config(config: dict) -> bool:
    """Validate project configuration."""
    required_fields = {"language", "version"}
    supported_languages = {"en", "pt-BR"}

    if not required_fields.issubset(config.keys()):
        return False

    if config["language"] not in supported_languages:
        return False

    return True
```

### Template Completeness Check

```python
def check_template_completeness(language: str) -> list[str]:
    """Return list of missing template files for a language."""
    required_templates = [
        "spec-template.md",
        "plan-template.md",
        "tasks-template.md",
        "checklist-template.md",
        "agent-file-template.md",
    ]

    missing = []
    templates_dir = Path(".specify/templates") / language

    for template in required_templates:
        if not (templates_dir / template).exists():
            missing.append(template)

    return missing
```
