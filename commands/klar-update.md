# /klar-update

Update CarbideKlar to the latest version.

## Usage

```
/klar-update
```

## What This Command Does

1. Fetches latest CarbideKlar from repository
2. Updates rules, commands, and documentation
3. Preserves any local customizations (optional backup)

## Instructions for Claude

When the user runs `/klar-update`:

### 1. Check current installation

Verify CarbideKlar is installed:

```bash
ls .claude/rules/ .claude/commands/
```

If directories don't exist, suggest `/klar-install` instead:
```
CarbideKlar is not installed. Run /klar-install first.
```

### 2. Offer backup (ask user)

```
Found existing CarbideKlar installation.
Do you want to backup current rules before updating? [Y/n]
```

If yes, create backups:
```bash
cp -r .claude/rules .claude/rules.backup.$(date +%Y%m%d)
cp -r .claude/commands .claude/commands.backup.$(date +%Y%m%d)
```

### 3. Fetch latest CarbideKlar

Remove old version and clone fresh:

```bash
rm -rf carbideklar
git clone https://github.com/PhilipLudington/CarbideKlar.git carbideklar
rm -rf carbideklar/.git
```

### 4. Copy updated files

Copy all CarbideKlar integration files:

```bash
# Commands
cp carbideklar/commands/*.md .claude/commands/

# Rules
cp carbideklar/rules/*.md .claude/rules/

# Documentation (create dirs if needed)
mkdir -p .claude/docs/patterns .claude/docs/security
cp -r carbideklar/docs/patterns/*.md .claude/docs/patterns/
cp -r carbideklar/docs/security/*.md .claude/docs/security/
```

### 5. Update version tracking

Create/update version file:

```bash
echo "0.4.0" > .claude/carbideklar-version
```

### 6. Show update report

```markdown
# CarbideKlar Update Complete

**Updated to:** v0.4.0 (Phase 4 - Language Completion)

## Updated Files

### Rules
- api-design.md: Updated (generics, trait implementation)
- concurrency.md: Updated (async/await, channels, select)
- errors.md: Updated (try blocks, error conversion)
- ownership.md: Updated (Drop trait, smart pointers)
- traits.md: NEW (trait patterns and builtin traits)
- naming.md, security.md, testing.md, comptime.md, logging.md, portability.md

### Commands
- klar-init.md: Updated (Phase 4 syntax)
- klar-install.md, klar-review.md, klar-safety.md, klar-check.md, klar-update.md

### Documentation
- docs/patterns/generics.md: NEW (generic programming patterns)
- docs/patterns/ownership.md, api-design.md, errors.md, resources.md
- docs/security/unsafe-blocks.md, injection.md, validation.md

## What's New in v0.4.0

- **Generics**: Generic functions, structs, enums with trait bounds
- **Traits**: Builtin traits (Eq, Ordered, Clone, Drop), custom implementations
- **Async/Await**: Full async patterns, spawn, channels, select
- **Syntax**: Explicit types, explicit return, statement-based control flow
- **Phase 4 Alignment**: Full native compilation support via LLVM
```

### 7. Verify installation

```bash
ls .claude/rules/
ls .claude/commands/
ls .claude/docs/patterns/
cat .claude/carbideklar-version
```

Confirm all files present and version is correct.

## Rollback

If update causes issues:

```
/klar-update --rollback
```

Instructions for Claude:

```bash
# Find most recent backup
BACKUP=$(ls -d .claude/rules.backup.* 2>/dev/null | tail -1)

if [ -n "$BACKUP" ]; then
    rm -rf .claude/rules
    mv "$BACKUP" .claude/rules

    COMMANDS_BACKUP=$(ls -d .claude/commands.backup.* 2>/dev/null | tail -1)
    if [ -n "$COMMANDS_BACKUP" ]; then
        rm -rf .claude/commands
        mv "$COMMANDS_BACKUP" .claude/commands
    fi

    echo "Rolled back to previous version"
else
    echo "No backup found. Cannot rollback."
fi
```

## Version History

| Version | Klar Phase | Key Features |
|---------|------------|--------------|
| 0.4.0 | Phase 4 | Generics, traits, async/await, explicit syntax |
| 0.2.0 | Phase 1 | Initial release, ownership, errors, security |
