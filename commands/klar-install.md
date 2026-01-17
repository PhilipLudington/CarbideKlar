# /klar-install

Install the CarbideKlar Klar development framework into the current project.

## Usage

```
/klar-install
```

## What This Command Does

1. Clones CarbideKlar repository
2. Copies rules, commands, and documentation to `.claude/`
3. Sets up version tracking
4. Updates CLAUDE.md with framework reference

## Instructions for Claude

When the user runs `/klar-install`:

### 1. Clone CarbideKlar

Clone the repository into the project:

```bash
git clone https://github.com/PhilipLudington/CarbideKlar.git carbideklar
rm -rf carbideklar/.git
```

### 2. Copy Claude Code integration

Create directories and copy files:

```bash
# Create directories
mkdir -p .claude/commands .claude/rules .claude/docs/patterns .claude/docs/security

# Copy commands
cp carbideklar/commands/*.md .claude/commands/

# Copy rules
cp carbideklar/rules/*.md .claude/rules/

# Copy documentation
cp carbideklar/docs/patterns/*.md .claude/docs/patterns/
cp carbideklar/docs/security/*.md .claude/docs/security/
```

### 3. Set version tracking

Create version file:

```bash
echo "0.4.0" > .claude/carbideklar-version
```

### 4. Add CarbideKlar reference to CLAUDE.md

If `./CLAUDE.md` doesn't exist, create it. Add the following:

```markdown
## Klar Development

This project uses the CarbideKlar framework (v0.4.0) for Klar development standards.

See `carbideklar/CARBIDEKLAR.md` for coding guidelines and available commands.

### Key Syntax Requirements (Phase 4)
- **Explicit types**: `let x: i32 = 42`
- **Explicit return**: `return value` (not implicit last expression)
- **Statement-based control flow**: assign inside blocks
- **Closures with full types**: `|x: i32| -> i32 { return x * 2 }`
```

### 5. Verify installation

Check that all files were copied:

```bash
ls .claude/commands/
ls .claude/rules/
ls .claude/docs/patterns/
cat .claude/carbideklar-version
```

Expected files:
- `.claude/commands/`: klar-init.md, klar-install.md, klar-review.md, klar-safety.md, klar-check.md, klar-update.md
- `.claude/rules/`: api-design.md, comptime.md, concurrency.md, errors.md, logging.md, naming.md, ownership.md, portability.md, security.md, testing.md, traits.md
- `.claude/docs/patterns/`: api-design.md, errors.md, generics.md, ownership.md, resources.md
- `.claude/docs/security/`: injection.md, unsafe-blocks.md, validation.md

### 6. Report completion

```markdown
# CarbideKlar Installation Complete

**Version:** 0.4.0 (Phase 4 - Language Completion)

## Installed Components

### Commands (6)
- `/klar-init` - Create new CarbideKlar project
- `/klar-install` - Install CarbideKlar (this command)
- `/klar-review` - Review code against standards
- `/klar-safety` - Security-focused review
- `/klar-check` - Run validation tooling
- `/klar-update` - Update to latest version

### Rules (11)
- api-design, comptime, concurrency, errors, logging
- naming, ownership, portability, security, testing, traits

### Documentation
- Pattern guides: api-design, errors, generics, ownership, resources
- Security guides: injection, unsafe-blocks, validation

## Next Steps

1. Review `carbideklar/STANDARDS.md` for full coding standards
2. Use `/klar-review` to check existing code
3. Use `/klar-init` to create new compliant projects

## Phase 4 Features

- Generic functions, structs, enums with trait bounds
- Builtin traits: Eq, Ordered, Clone, Drop
- Async/await, spawn, channels, select
- Explicit types and return statements required
```

## After Installation

The following commands are now available:

| Command | Purpose |
|---------|---------|
| `/klar-review` | Review code against standards |
| `/klar-safety` | Security-focused review |
| `/klar-check` | Run validation tooling |
| `/klar-update` | Update to latest version |

## Updating

To update CarbideKlar to the latest version:

```
/klar-update
```
