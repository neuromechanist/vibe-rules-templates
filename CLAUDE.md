# Rule Templates Repository Instructions

## Project Context
**Purpose:** Development and maintenance of AI-assisted coding templates for Cursor and Claude
**Tech Stack:** Markdown, MDC rules, Shell scripting
**Architecture:** Dual-template system with shared components

## Repository Structure
```
rule_templates/
├── claude-templates/       # Claude-specific templates
│   ├── CLAUDE.md           # Main instructions (<150 lines)
│   └── .rules/             # Detailed rules (markdown)
├── cursor-templates/       # Cursor-specific templates
│   └── planning/           # Planning approaches
├── shared/                 # Common components
│   └── .context-template/  # Template context files
├── commands/               # Slash commands (init-project)
└── plugins/                # Workflow plugins (epic-dev)
```

## Development Workflow
1. **Check context:** Review .context/plan.md for current tasks
2. **Understand deeply:** Check .context/ideas.md for design decisions
3. **Branch:** `gh issue develop <issue-number>`
4. **Implement:** Follow existing patterns in templates
5. **Test:** Verify templates work with both tools
6. **Commit:** Atomic, <50 chars, no emojis
7. **PR:** Run `/review-pr`, address ALL findings

## [CRITICAL] Core Principles

### NO MOCKS - Test Reality Only
- Templates must enforce real testing
- No mock examples in any template
**Details:** claude-templates/.rules/testing.md

### No Technical Debt Carried Forward
- Address ALL PR review findings
- Only skip genuine false positives or intended design choices
- Replace, don't deprecate
**Details:** claude-templates/.rules/code_review.md

### Tool Alignment
- **Python:** UV only (not pip/conda/venv)
- **JS/TS:** Bun only (not npm/npx)
- **Type checking:** ty for Python, tsc for TS
- **Linting:** ruff for Python, Biome for JS/TS
- All templates must reflect these tool choices consistently

### Template Quality
- Keep CLAUDE.md under 150 lines
- Use attention flags sparingly ([CRITICAL], [NEVER DO THIS])
- Maximum referencing to .rules/
- Include "Never Do This" anti-pattern sections
- Maintain both Cursor and Claude versions

## Think Like a Template Designer
- What will users need daily vs occasionally?
- How can we reduce cognitive load?
- Will this template scale from simple to complex?
- Anti-patterns (what NOT to do) are more effective than positive instructions

## Key Files to Maintain
- `claude-templates/CLAUDE.md` - Must stay <150 lines
- `claude-templates/.rules/*.md` - Must align with global CLAUDE.md tool choices
- `commands/init-project.md` - Track CLAUDE.md and .context/ in git by default
- `README.md` - User-facing documentation

## Context Files [ACTIVE USE]
- `.context/plan.md` - Current development tasks
- `.context/research.md` - Template design decisions
- `.context/ideas.md` - Feature concepts
- `.context/scratch_history.md` - What didn't work

## Quick Commands
```bash
# Check template line count
wc -l claude-templates/CLAUDE.md  # Must be <150

# Test claude templates
cp -r claude-templates/ test-project/
cd test-project && claude .
```

---
Remember: We're building templates that enforce best practices. NO MOCKS. No technical debt.
