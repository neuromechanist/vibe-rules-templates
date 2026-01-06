# AI Development Templates for Cursor & Claude

Optimized templates for AI-assisted development with separate configurations for Cursor and Claude.

## 🎯 Quick Start

### For Cursor Users
```bash
cp -r cursor-templates/.cursorrules your-project/
cp -r cursor-templates/core_rules your-project/
cp -r shared/.context-template your-project/.context
```

Then, tell the agent to edit the context and rule documents to match your project. It is always better to have your main idea as a paragraph or a separate document first and then initialize the project based on that.

### For Claude Users  
```bash
cp claude-templates/CLAUDE.md your-project/
cp -r claude-templates/.rules your-project/
cp -r shared/.context-template your-project/.context
```

**For Claude Code:** Copy the `commands/init-project.md` to `~/.claude/commands/`. Then you will be able to do `/init-project` in a Claude Code session with your project idea. It again works best on an empty project. But, even if the project is fully operational, Claude will search through the directory and populate the documents based on the project design.

## 📁 New Dual-Template Structure

```
rule_templates/
├── cursor-templates/       # Cursor-optimized templates
│   ├── .cursorrules       # Main configuration file
│   ├── core_rules/        # Modular .mdc rule files
│   └── planning/          # Workflow approaches
│       ├── default/       # Plan-based (recommended)
│       └── advanced-taskmaster/  # For complex projects
│
├── claude-templates/       # Claude-optimized templates
│   ├── CLAUDE.md          # Concise instructions (<150 lines)
│   └── .rules/            # Detailed rule references (.md)
│       ├── testing.md     # NO MOCK policy details
│       ├── python.md      # Language standards
│       └── ...
│
└── shared/                # Common components
    ├── .context-template/ # Workflow documentation
    │   ├── plan.md       
    │   ├── research.md   
    │   ├── ideas.md      
    │   └── scratch_history.md
    ├── config/           # Pre-commit, gitignore, pyproject
    └── github/           # GitHub Actions workflows
```

## 🔑 Core Principles

### [CRITICAL] NO MOCK Testing Policy
- **Never** use mocks, stubs, or fake data
- Real tests with real data only
- If real testing isn't possible, don't write tests
- Ask user for sample data or test environment
- See `.rules/testing.md` for complete policy

### [FUNDAMENTAL] Documentation-Driven Development
All projects use `.context/` directory:
- **plan.md** - Task tracking and phases
- **research.md** - Technical explorations
- **ideas.md** - Design concepts  
- **scratch_history.md** - Failed attempts and lessons

### Commit Standards
- Messages <50 characters, no emojis
- Atomic commits with single purpose
- Feature branches: `feature/short-description`
- See `.rules/git.md` for details

## 📋 Cursor vs Claude Comparison

| Feature | Cursor | Claude |
|---------|--------|--------|
| **Main File** | `.cursorrules` | `CLAUDE.md` |
| **Rule Format** | `.mdc` files (modular) | `.md` references |
| **Structure** | Distributed rules | Inline critical + references |
| **File Size** | Multiple small files | <150 lines total |
| **Planning** | MDC workflows | Embedded instructions |
| **Flexibility** | High modularity | Quick scanning |
| **Best For** | Cursor IDE users | Claude Code/API users |

## 🚀 Setup Instructions

### Step 1: Choose Your Tool

#### Cursor Setup
```bash
# Copy core files
cp cursor-templates/.cursorrules my-project/
cp -r cursor-templates/core_rules my-project/

# Copy planning (default: plan-based)
cp cursor-templates/planning/default/dev_workflow.mdc my-project/

# Copy shared components
cp -r shared/.context-template my-project/.context
cp shared/config/gitignore-template my-project/.gitignore
```

#### Claude Setup
```bash
# Copy main instruction file
cp claude-templates/CLAUDE.md my-project/

# Copy rule references
cp -r claude-templates/.rules my-project/

# Copy shared components
cp -r shared/.context-template my-project/.context
cp shared/config/gitignore-template my-project/.gitignore
```

### Step 2: Customize Templates

Replace placeholders in all files:
- `{{PROJECT_NAME}}` - Your project name
- `{{ENV_NAME}}` - Python environment name
- `{{TECH_STACK}}` - Your technology stack

### Step 3: Set Up Pre-commit (Python Projects)
```bash
cp shared/config/pre-commit .git/hooks/
chmod +x .git/hooks/pre-commit
# Automatically runs ruff format and check on commits
```

### Step 4: Initialize Context
```bash
cd my-project
# Context files are already in .context/
# Just start using them!
```

## 🔄 Migration Between Tools

### Cursor → Claude
1. Combine `.cursorrules` + `.mdc` rules into `CLAUDE.md`
2. Extract critical rules (NO MOCKS, commits) inline
3. Convert remaining `.mdc` files to `.md` in `.rules/`
4. Keep CLAUDE.md under 150 lines

### Claude → Cursor
1. Split `CLAUDE.md` sections into `.mdc` files
2. Create `.cursorrules` pointing to rule files
3. Move detailed content to appropriate `.mdc` files
4. Maintain modular structure

## 📚 Planning Approaches

### Default: Plan-Based Development
- Simple `.context/plan.md` with checkboxes
- Best for most projects
- Clear task progression
- No external dependencies

### Advanced: TaskMaster
- AI-powered task breakdown
- Dependency management
- For complex multi-phase projects
- Requires TaskMaster CLI installation

## 🛠️ Included Components

### Shared Configuration Files
- **gitignore-template** - Comprehensive Python/.context ignores
- **pre-commit** - Python formatting hook (ruff)
- **pyproject.toml** - Python project configuration
- **mkdocs.yml** - Documentation site config

### GitHub Actions Workflows
- **test.yml** - CI testing pipeline
- **docs.yml** - Documentation deployment
- **release.yml** - Package release automation

## 📖 Rule Categories

### Always Referenced
- **testing.md** - NO MOCK policy and real testing
- **self_improve.md** - Learning from projects
- **documentation.md** - MkDocs and writing standards

### Language/Tool Specific
- **python.md** - Python style, linting, types
- **git.md** - Version control standards
- **ci_cd.md** - GitHub Actions setup

## 🤝 Contributing

1. Create issue for proposed changes
2. Branch: `feature/description`
3. Make atomic commits (<50 chars)
4. Test with both Cursor and Claude
5. PR with clear description
6. Update both template versions

## 📝 Key Differences from Original

### What's New
- **Dual-template system** for tool-specific optimization
- **CLAUDE.md** format for Claude users (<150 lines)
- **Shared components** reduce duplication
- **Plan-based as default** (TaskMaster now advanced option)
- **.context/** directory standard for all templates

### What's Preserved
- NO MOCK testing philosophy
- Atomic commit standards
- Documentation-driven development
- Pre-commit hooks
- GitHub Actions workflows

## 📜 License

MIT - See LICENSE file

---

*Remember: Real tests only. No mocks. Document everything in .context/.*
