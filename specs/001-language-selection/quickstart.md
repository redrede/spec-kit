# Quickstart: Multi-Language Support Validation

**Feature Branch**: `001-language-selection`
**Date**: 2026-01-18

---

## Purpose

This document provides validation scenarios to verify that multi-language support is working correctly after implementation.

---

## Prerequisites

- Spec Kit CLI installed (`uv tool install specify-cli --from git+https://github.com/github/spec-kit.git`)
- Terminal with interactive input support
- Write access to create test directories

---

## Validation Scenario 1: Language Selection During Setup (P1)

### Test: Interactive Language Selection

```bash
# Create a temporary test directory
mkdir -p /tmp/speckit-test-1
cd /tmp/speckit-test-1

# Run specify init without --language flag
specify init test-project --ai claude
```

**Expected Behavior:**
1. CLI displays language selection prompt
2. Shows "English (Default)" and "Português (Brasil)" options
3. Arrow keys navigate between options
4. Enter confirms selection
5. Selection is stored in `.specify/config.json`

**Verification:**
```bash
cd test-project
cat .specify/config.json
# Should show: {"language": "en", "version": "1.0"} or {"language": "pt-BR", "version": "1.0"}
```

### Test: Non-Interactive Language Selection

```bash
# Create with Portuguese via flag
mkdir -p /tmp/speckit-test-2
cd /tmp/speckit-test-2

specify init test-project --ai claude --language pt-BR
```

**Expected Behavior:**
- No language prompt displayed
- Project created with Portuguese templates

**Verification:**
```bash
cd test-project
cat .specify/config.json
# Should show: {"language": "pt-BR", "version": "1.0"}
```

### Test: Default Language (No Selection)

```bash
# Pipe empty input to simulate non-interactive
mkdir -p /tmp/speckit-test-3
cd /tmp/speckit-test-3

echo "" | specify init test-project --ai claude
```

**Expected Behavior:**
- English is used as default

**Verification:**
```bash
cd test-project
cat .specify/config.json
# Should show: {"language": "en", "version": "1.0"}
```

---

## Validation Scenario 2: Localized Templates (P2)

### Test: Portuguese Templates

```bash
mkdir -p /tmp/speckit-test-4
cd /tmp/speckit-test-4

specify init test-project --ai claude --language pt-BR
cd test-project
```

**Verification - Check template content:**
```bash
# Check spec template has Portuguese headings
head -20 .specify/templates/pt-BR/spec-template.md
# Should contain: "# Especificação de Funcionalidade:" or similar

# Check plan template has Portuguese headings
head -20 .specify/templates/pt-BR/plan-template.md
# Should contain: "# Plano de Implementação:" or similar

# Check tasks template
head -20 .specify/templates/pt-BR/tasks-template.md
# Should contain: "# Tarefas:" or similar
```

### Test: English Templates

```bash
mkdir -p /tmp/speckit-test-5
cd /tmp/speckit-test-5

specify init test-project --ai claude --language en
cd test-project
```

**Verification:**
```bash
head -20 .specify/templates/en/spec-template.md
# Should contain: "# Feature Specification:"
```

---

## Validation Scenario 3: Localized Constitution (P3)

### Test: Portuguese Constitution

```bash
mkdir -p /tmp/speckit-test-6
cd /tmp/speckit-test-6

specify init test-project --ai claude --language pt-BR
cd test-project
```

**Verification:**
```bash
head -30 .specify/memory/constitution.md
# Should contain Portuguese text: "# Constituição do Projeto" or similar
```

### Test: English Constitution

```bash
mkdir -p /tmp/speckit-test-7
cd /tmp/speckit-test-7

specify init test-project --ai claude --language en
cd test-project
```

**Verification:**
```bash
head -30 .specify/memory/constitution.md
# Should contain: "# Project Constitution" or similar
```

---

## Validation Scenario 4: AI-Generated Content (P4)

### Test: Portuguese Content Generation

**Setup:**
```bash
mkdir -p /tmp/speckit-test-8
cd /tmp/speckit-test-8

specify init test-project --ai claude --language pt-BR
cd test-project
```

**Run in AI assistant:**
```
/speckit.specify Criar um sistema de login com autenticação por email
```

**Verification:**
- Generated `spec.md` should have Portuguese headings
- User stories written in Portuguese
- Requirements in Portuguese
- Success criteria in Portuguese

### Test: Portuguese Content Generation with English Input

**Setup:**
```bash
# Using same Portuguese project from above
```

**Run in AI assistant:**
```
/speckit.specify Build a user dashboard for analytics
```

**Verification:**
- Even with English input, output should be in Portuguese
- AI respects project language preference

---

## Validation Scenario 5: Fallback Behavior

### Test: Missing Translation Fallback

**Scenario:** If a specific template is missing in pt-BR directory

**Verification:**
```bash
# Temporarily remove a Portuguese template
mv .specify/templates/pt-BR/checklist-template.md /tmp/

# Use a command that requires checklist template
# Should see warning and use English fallback
# ⚠ Template 'checklist-template.md' not available in pt-BR, using English

# Restore
mv /tmp/checklist-template.md .specify/templates/pt-BR/
```

### Test: Corrupted Config Fallback

**Scenario:** If config.json is corrupted

**Verification:**
```bash
# Corrupt the config file
echo "not valid json" > .specify/config.json

# Run any command
# Should see warning and default to English
# ⚠ Invalid config file, using English
```

---

## Validation Scenario 6: Extensibility Check (P5)

### Test: Adding a New Language (Dry Run)

**Document verification only - no actual implementation:**

1. New language directory would be created: `.specify/templates/es/`
2. All template files would be copied from `en/`
3. Files would be translated
4. `LANGUAGE_CHOICES` dict would be updated in CLI
5. New language would appear in selection menu

**Verification questions:**
- [ ] Is the directory structure documented?
- [ ] Are placeholder tokens clearly identified?
- [ ] Is there a validation script for completeness?

---

## Error Case Validation

### Test: Invalid Language Code

```bash
specify init test-project --ai claude --language xyz
```

**Expected:**
```text
Error: Unsupported language: xyz. Available: en, pt-BR
```

### Test: Project Already Exists

```bash
mkdir -p /tmp/speckit-test-existing
cd /tmp/speckit-test-existing
mkdir test-project

specify init test-project --ai claude --language pt-BR
```

**Expected:**
```text
Error: Directory 'test-project' already exists. Use --force to overwrite.
```

---

## Cleanup

After validation, clean up test directories:

```bash
rm -rf /tmp/speckit-test-*
```

---

## Validation Checklist

| Scenario | Status | Notes |
|----------|--------|-------|
| Interactive language selection | ☐ | |
| Non-interactive with --language flag | ☐ | |
| Default to English | ☐ | |
| Portuguese templates installed | ☐ | |
| English templates installed | ☐ | |
| Portuguese constitution | ☐ | |
| English constitution | ☐ | |
| AI generates Portuguese content | ☐ | |
| AI respects language preference | ☐ | |
| Fallback for missing translation | ☐ | |
| Fallback for corrupted config | ☐ | |
| Invalid language code error | ☐ | |
