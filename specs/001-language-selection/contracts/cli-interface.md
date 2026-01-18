# CLI Interface Contract: Multi-Language Support

**Feature Branch**: `001-language-selection`
**Date**: 2026-01-18

---

## Overview

This document defines the CLI interface contracts for the multi-language support feature. Since Spec Kit is a CLI tool (not a REST API), contracts are defined as command signatures, flags, and expected behaviors.

---

## Command: `specify init`

### Updated Signature

```bash
specify init [PROJECT_NAME] [OPTIONS]
```

### New Option: `--language` / `-l`

| Flag | Type | Default | Description |
|------|------|---------|-------------|
| `--language`, `-l` | `string` | `"en"` | Set the language for templates and AI-generated content |

### Valid Values

| Code | Display Name | Status |
|------|--------------|--------|
| `en` | English (Default) | Supported |
| `pt-BR` | Português (Brasil) | Supported |

### Usage Examples

```bash
# Interactive mode (prompted for language)
specify init my-project --ai claude

# Non-interactive mode (language specified via flag)
specify init my-project --ai claude --language pt-BR

# Short form
specify init my-project --ai claude -l pt-BR

# Current directory with language
specify init . --ai claude --language pt-BR

# Using --here flag
specify init --here --ai claude --language pt-BR
```

### Interaction Flow

When `--language` is NOT provided and stdin is interactive:

```text
┌────────────────────────────────────────────────┐
│  Choose your language:                         │
│                                                │
│  ▶ English (Default)                           │
│    Português (Brasil)                          │
│                                                │
│  ↑↓ to navigate, Enter to select, Esc to exit │
└────────────────────────────────────────────────┘
```

When `--language` IS provided:
- Skip language selection prompt
- Use provided language value directly
- Validate language code against supported values

### Error Cases

| Scenario | Behavior |
|----------|----------|
| Invalid language code (`--language xyz`) | Error: "Unsupported language: xyz. Available: en, pt-BR" |
| Non-interactive + no `--language` flag | Use default (`en`) |
| Invalid combination with other flags | Standard Typer validation |

### Output Changes

Success message now includes language:

**English:**
```text
✓ Project created: my-project
  Language: English
  AI Assistant: Claude Code
  Script Type: POSIX Shell

Next steps:
  cd my-project
  claude
```

**Portuguese:**
```text
✓ Projeto criado: my-project
  Idioma: Português (Brasil)
  Assistente IA: Claude Code
  Tipo de Script: POSIX Shell

Próximos passos:
  cd my-project
  claude
```

---

## File Contracts

### Config File: `.specify/config.json`

**Location**: `<project_root>/.specify/config.json`
**Created By**: `specify init`
**Modified By**: Manual user edit (future: `specify config` command)

#### Schema

```json
{
  "type": "object",
  "properties": {
    "language": {
      "type": "string",
      "enum": ["en", "pt-BR"]
    },
    "version": {
      "type": "string",
      "pattern": "^\\d+\\.\\d+$"
    }
  },
  "required": ["language", "version"]
}
```

#### Example

```json
{
  "language": "pt-BR",
  "version": "1.0"
}
```

---

## Template Loading Contract

### Interface

```python
def get_template_path(template_name: str, language: str = None) -> Path:
    """
    Get the path to a language-specific template.

    Args:
        template_name: Name of the template file (e.g., "spec-template.md")
        language: ISO language code. If None, reads from project config.

    Returns:
        Path to the template file

    Raises:
        FileNotFoundError: If template doesn't exist in any language

    Behavior:
        1. Try .specify/templates/{language}/{template_name}
        2. If not found, fallback to .specify/templates/en/{template_name}
        3. Print warning when falling back
    """
```

### Fallback Behavior

```text
Request: get_template_path("spec-template.md", "pt-BR")

Step 1: Check .specify/templates/pt-BR/spec-template.md
        ├── Exists? → Return path
        └── Not found? → Step 2

Step 2: Check .specify/templates/en/spec-template.md
        ├── Exists? → Return path + Warning
        └── Not found? → FileNotFoundError
```

### Warning Output

When fallback occurs:
```text
⚠ Template 'spec-template.md' not available in pt-BR, using English
```

---

## Environment Variable Contract

### `SPECIFY_LANGUAGE`

**Purpose**: Override project language for a single command
**Scope**: Command execution only (does not modify config file)

```bash
# Use Portuguese for this command only
SPECIFY_LANGUAGE=pt-BR specify check

# Or inline
env SPECIFY_LANGUAGE=pt-BR specify check
```

### Priority Order

1. `SPECIFY_LANGUAGE` environment variable (highest)
2. Command-line flag (`--language`)
3. Project config (`.specify/config.json`)
4. Default (`en`)

---

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | General error |
| 2 | Invalid language code |
| 3 | Missing required files |

---

## Compatibility Notes

### Backward Compatibility

- Existing projects without `.specify/config.json` default to English
- Existing `specify init` commands without `--language` continue to work
- All existing flags (`--ai`, `--script`, `--here`, etc.) work unchanged

### Forward Compatibility

- Config file `version` field allows for future schema changes
- Additional languages can be added without breaking changes
- Template directory structure supports arbitrary language codes
