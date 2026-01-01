# {{PROJECT_NAME}} Instructions

## Project Context
**Purpose:** [Brief description of what you're building]
**Tech Stack:** Python 3.11+, [frameworks]
**Architecture:** [Key design decisions]

## Environment Setup
```bash
conda activate {{ENV_NAME}}  # or source .venv/bin/activate
pip install -e .
pytest  # Real tests only - NO MOCKS
```

## Development Workflow
1. **Check context:** Review .context/plan.md for current tasks
2. **Understand deeply:** Check .context/ideas.md for design decisions
3. **Research if needed:** Update .context/research.md with findings
4. **Branch:** `git checkout -b feature/short-description`
5. **Code:** Follow patterns (see .rules/python.md for standards)
6. **Test:** Real data only (see .rules/testing.md for details)
7. **Document failures:** Log in .context/scratch_history.md immediately
8. **Commit:** Atomic, <50 chars, no emojis
9. **PR:** Reference context and issue
10. **Code review:** Run pr-review-toolkit after creating PR (see .rules/code_review.md)

## [CRITICAL] Core Principles - Never Compromise

### [FUNDAMENTAL] NO MOCKS - Test Reality Only
- Use real data or skip tests entirely
- Docker for test databases
- Ask user for sample data if needed
**Details:** .rules/testing.md

### Commits & Git
- Atomic commits, focused changes
- Messages <50 chars, no emojis
- Feature branches for multi-step work
**Details:** .rules/git.md

### Documentation
- Examples > explanations
- README gets someone running in <5 minutes
**Details:** .rules/documentation.md

## Think Like a Senior Developer
- Keep the big picture in mind
- Ask: "Will this prevent a 3am wake-up call?"
- Document learnings → .context/scratch_history.md
- Extract patterns (3+ uses) → Create rules
**See:** .rules/self_improve.md for learning process

## [REFERENCE] Rules Directory

### Core Standards
- `.rules/testing.md` - Complete NO MOCK policy
- `.rules/self_improve.md` - Learning from projects
- `.rules/documentation.md` - MkDocs setup
- `.rules/code_review.md` - PR review toolkit and checklist

### Language & Tools
- `.rules/python.md` - Style, linting, type hints
- `.rules/javascript.md` - ES6+, TypeScript
- `.rules/ci_cd.md` - GitHub Actions setup
- `.rules/{{framework}}.md` - Framework patterns

### MCP Tools (When Available)
- `.rules/serena_mcp.md` - Code intelligence with Serena MCP

## Context Files
- `.context/plan.md` - Current tasks and phases
- `.context/research.md` - Technical explorations
- `.context/ideas.md` - Design concepts
- `.context/scratch_history.md` - Failed attempts

## Quick Commands
```bash
# Run tests (real data only)
pytest tests/ --cov

# Format code
ruff check --fix . && ruff format .

# Build docs
mkdocs serve
```

## Project-Specific Guidelines
[Add custom requirements here]
[Team conventions]
[Special considerations]

---
Remember: You're building maintainable systems, not just writing code.
Check .rules/ for detailed guidance on any topic.