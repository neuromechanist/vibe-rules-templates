# Dual Template System for Cursor and Claude

## Core Concept
Create separate template structures optimized for each AI tool while maintaining consistency.

## Proposed Structure
```
rule_templates/
├── cursor-templates/      # Cursor-optimized
│   ├── .context/         # Workflow docs
│   ├── .rules/           # MDC rule files
│   │   ├── testing.mdc
│   │   ├── ci_cd.mdc
│   │   └── ...
│   └── .cursorrules      # Main Cursor config
│
├── claude-templates/      # Claude-optimized  
│   ├── .context/         # Workflow docs
│   ├── .rules/           # Reference rules (markdown)
│   │   ├── testing.md
│   │   ├── ci_cd.md
│   │   └── ...
│   └── CLAUDE.md         # Main Claude instructions
│
└── shared/               # Common elements
    ├── config/           # Pre-commit, gitignore
    ├── github/           # Actions workflows
    └── .context-templates/ # Template context files

```

## Key Differences

### Cursor Templates
- Uses `.cursorrules` file pointing to `.rules/*.mdc`
- MDC format for modular rules
- Lighter main file, heavier on references

### Claude Templates  
- Uses `CLAUDE.md` with embedded critical rules
- References `.rules/*.md` for detailed guidance
- Heavier main file with core workflow inline
- Critical rules (NO MOCKS, atomic commits) in CLAUDE.md

## CLAUDE.md Structure
```markdown
# Project Instructions for Claude

## Core Principles (Always Apply)
- NO MOCK tests or data - real or nothing
- Atomic commits under 50 chars, no emojis
- Documentation-driven development via .context/

## Development Workflow
[Full workflow inline - from cursor_rules.mdc]

## Detailed Rules
See `.rules/` directory for:
- Testing standards: .rules/testing.md
- CI/CD setup: .rules/ci_cd.md
- Documentation: .rules/documentation.md

## Project-Specific
[Space for user customization]
```

## Migration Strategy

### Cursor → Claude
1. Convert `.cursorrules` + `.rules/*.mdc` → `CLAUDE.md`
2. Extract critical rules into CLAUDE.md body
3. Convert remaining MDC files to markdown in `.rules/`

### Claude → Cursor  
1. Extract workflow from CLAUDE.md → `.cursorrules`
2. Move detailed rules to `.rules/*.mdc`
3. Keep only references in main file

## Planning Approach Changes

### Default: Plan-based (Simple)
- Start with .context/ structure
- Basic plan.md approach
- Graduate to TaskMaster when needed

### Advanced: TaskMaster
- For complex multi-dependency projects
- AI-assisted task breakdown
- Full MCP integration

## Implementation Benefits
1. **Tool-optimized**: Each template fits its tool's strengths
2. **Flexible**: Easy to adapt and migrate
3. **Clear hierarchy**: Critical rules vs detailed guidance  
4. **Maintainable**: Shared components reduce duplication
5. **User-friendly**: Clear when to use which approach