# Klar Install

Install the CarbideKlar Klar development framework into the current project.

## Instructions

1. **Clone CarbideKlar** into the project:
   ```bash
   git clone https://github.com/PhilipLudington/CarbideKlar.git carbideklar
   rm -rf carbideklar/.git
   ```

2. **Copy Claude Code integration**:
   ```bash
   mkdir -p .claude/commands .claude/rules
   cp carbideklar/commands/*.md .claude/commands/
   cp carbideklar/rules/*.md .claude/rules/
   ```

3. **Add CarbideKlar reference to CLAUDE.md**:

   If `./CLAUDE.md` doesn't exist, create it. Add the following:
   ```markdown
   ## Klar Development

   This project uses the CarbideKlar framework for Klar development standards.
   See `carbideklar/CARBIDEKLAR.md` for coding guidelines and available commands.
   ```

4. **Verify installation**:
   - Confirm `.claude/commands/` contains klar-*.md files
   - Confirm `.claude/rules/` contains the rule files
   - Confirm `CLAUDE.md` references the CarbideKlar framework

## After Installation

The following commands are now available:
- `/klar-review` - Review code against standards
- `/klar-safety` - Security-focused review
- `/klar-check` - Run validation tooling
