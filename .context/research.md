# Research: AI Coding Assistant Rule Files - Best Practices

**Date:** 2026-03-07
**Scope:** CLAUDE.md, .cursorrules/.mdc, Windsurf rules, and cross-tool patterns

---

## 1. CLAUDE.md Best Practices

### What to Include (Essential Sections)

1. **Project context** - One-liner orientation ("This is a Next.js e-commerce app with Stripe integration")
2. **Tech stack and tools** - Languages, frameworks, package managers, preferred tooling
3. **Commands** - Exact build, test, lint, deploy commands Claude should use
4. **Architecture map** - Directory structure with purpose of each directory (critical for monorepos)
5. **Code style** - Formatting preferences, import style, naming conventions
6. **Important gotchas** - Project-specific warnings and constraints
7. **Custom tools** - Document MCP tools, custom commands with usage examples

### Sizing and Organization

- **Target: under 150-200 lines per file** (Anthropic recommends ~500 tokens ideally)
- For each line, ask: "Would removing this cause Claude to make mistakes?" If not, cut it
- Bloated files cause Claude to ignore instructions; if Claude keeps breaking a rule despite having it, the file is probably too long
- Use bullet points over paragraphs; one concept per line

### Hierarchy (loads bottom-up, more specific overrides broader)

1. **Enterprise Policy** - Organization-level (Claude for Teams)
2. **User Memory** - `~/.claude/CLAUDE.md` for personal global preferences
3. **Project Memory** - `CLAUDE.md` in repo root
4. **Project Rules** - Files in `.claude/rules/` directory (auto-loaded, same priority as CLAUDE.md)
5. **Subdirectory** - CLAUDE.md in nested directories (most specific, highest priority)

### Import Syntax

- Use `@path/to/file` to reference other files (e.g., `See @docs/api-patterns.md`)
- Imported files expand into context at launch
- Recursive imports supported up to 5 levels deep
- `.claude/rules/` directory: all .md files auto-loaded, no imports needed

### Key Insight: Prefer Pointers to Copies

- Avoid pasting code snippets; use `file:line` references instead
- Keeps rules short and prevents staleness as code changes
- Example: "See src/utils/auth.ts:45 for the auth pattern" instead of copying the code

### What Actually Makes a Difference

- **Build/test commands** prevent Claude from guessing or scanning the codebase each time
- **Architecture map** is the single most impactful section for monorepos
- **Anti-pattern gallery** (explicit "never do X") is more reliable than "always do Y"
- **Check it into git** so the file compounds in value as team contributes

---

## 2. Cursor Rules Best Practices

### File Formats

- **Legacy:** `.cursorrules` file in project root (single file, still works)
- **Modern:** `.cursor/rules/` directory with `.mdc` (Markdown Component) files

### .mdc Frontmatter Format

```yaml
---
description: [When/why this rule applies]
globs: ["*.py", "src/**/*.ts"]
alwaysApply: true/false
---

Rule content in markdown...
```

### Rule Types

| Type | `alwaysApply` | `globs` | `description` | Behavior |
|------|-----------|-------|-------------|----------|
| Always | true | - | optional | Included in every conversation |
| Auto Attached | - | set | optional | Attached when matching files are referenced |
| Agent Requested | - | - | set | AI reads description, decides if relevant |
| Manual | false | - | - | Only when explicitly @-mentioned |

### What Works in Practice

- **Specific and actionable** rules: "Use camelCase for variables", "Limit functions to 20 lines", "Include type annotations for all parameters"
- **Vague rules fail**: "Write clean code" does nothing
- **Fail fast pattern**: Handle errors at function start, early returns for error conditions, happy path last
- **Organize by concern**: separate files for testing, style, error-handling, security
- **Glob patterns**: auto-attach language-specific rules only when those files are edited

### Community Resources

- [cursor.directory](https://cursor.directory/) - Community rule examples
- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) - Curated collection by framework/language

---

## 3. Cross-Tool Patterns (What Works Everywhere)

### Universal Rule Structure

Based on [steipete/agent-rules](https://github.com/steipete/agent-rules), [Trail of Bits claude-code-config](https://github.com/trailofbits/claude-code-config), [jsonallen/example-ai-coding-rules](https://github.com/jsonallen/example-ai-coding-rules), and [rulebook-ai](https://github.com/botingw/rulebook-ai):

1. **Project context** - What is this, what stack, what's the goal
2. **Tooling preferences** - Specific tools over vague categories (e.g., "uv not pip", "ruff not black")
3. **Code patterns** - What patterns to follow with examples
4. **Anti-patterns** - What to never do (more effective than positive instructions)
5. **Testing philosophy** - Real tests, coverage expectations, test commands
6. **Git workflow** - Commit format, branching strategy, PR process

### Trail of Bits Opinionated Defaults (High-Quality Example)

Their rules enforce:
- **Replace, don't deprecate** - No backward-compatible shims, no dual config formats, no migration paths
- **Prefer clarity over cleverness** - Readable code over clever abstractions
- **Justify new dependencies** - Don't add deps without justification
- **No speculative features** - Only build what users actively need
- **Avoid premature abstraction** - Concrete first, abstract only when pattern repeats
- Python: uv + ruff + ty (not pip/poetry + black/pylint + mypy)
- JS/TS: oxlint + oxfmt (not eslint + prettier)

### The "Anti-Pattern Gallery" Technique

Explicitly forbidding common AI mistakes is more reliable than describing desired behavior:
```markdown
## NEVER DO
- Never use `any` type in TypeScript
- Never catch errors silently
- Never use deprecated APIs (list specific ones)
- Never commit .env files
- Never use mocks in tests
```

This works because LLMs are better at recognizing "don't do X" patterns than inferring desired behavior from positive examples.

### Universal File Organization

| Tool | Global | Project | Per-Directory |
|------|--------|---------|---------------|
| Claude Code | `~/.claude/CLAUDE.md` | `CLAUDE.md` + `.claude/rules/*.md` | Nested `CLAUDE.md` |
| Cursor | Settings > Rules | `.cursor/rules/*.mdc` | Nested `.cursor/rules/` |
| Windsurf | `global_rules.md` | `.windsurfrules` | - |
| Codex | `~/.codex/instructions.md` | `AGENTS.md` | Nested `AGENTS.md` |
| GitHub Copilot | - | `.github/copilot-instructions.md` | - |

---

## 4. What Experienced Developers Include That Makes a Real Difference

### High-Impact (Include Always)

1. **Exact commands** - `uv run pytest tests/ -x` not "run the tests"
2. **Architecture map with purpose** - Not just directory names but what each does
3. **Explicit tool choices** - "Use Bun, not npm" prevents wrong defaults
4. **Error handling pattern** - One clear example the AI copies everywhere
5. **Anti-pattern gallery** - Explicit "never do" list
6. **File size limits** - "Keep files under 300 lines" prevents sprawl
7. **Import order** - Saves constant reformatting

### Medium-Impact (Include If Relevant)

1. **Naming conventions** - camelCase vs snake_case per context
2. **Testing philosophy** - No mocks, real DB, coverage thresholds
3. **PR/commit format** - Conventional commits, message length
4. **Security constraints** - Never log secrets, always validate input
5. **Performance guardrails** - No N+1 queries, use pagination

### Low-Impact (Often Noise)

1. **Generic advice** - "Write clean code", "Follow best practices"
2. **Lengthy philosophical sections** - AI doesn't need motivation
3. **Duplicated documentation** - Reference it, don't copy it
4. **Obvious patterns** - Things the AI already does well

### The "Start Minimal, Add on Failure" Approach

From Cursor's official blog and multiple practitioners:
- Start with 5-10 lines
- Add a rule ONLY when the AI makes the same mistake twice
- Add a command ONLY after you have a workflow you want to repeat
- Review and prune rules quarterly

---

## 5. Anti-Patterns to Avoid

### Anti-Pattern 1: Bloated Rule Files
- **Problem:** 500+ line rule files where important instructions get lost
- **Fix:** Keep under 150-200 lines; use imports/references for detail
- **Why it matters:** Tokens spent on instructions reduce context for actual code

### Anti-Pattern 2: Vague Instructions
- **Problem:** "Follow best practices", "Write clean code"
- **Fix:** Specific, actionable rules with examples
- **Example:** Instead of "Handle errors properly" use "Use early returns for error conditions; wrap external calls in try/catch with specific error types"

### Anti-Pattern 3: Copying Code Into Rules
- **Problem:** Pasting code snippets that become stale
- **Fix:** Reference files by path and line number
- **Example:** "See @src/middleware/auth.ts for the authentication pattern"

### Anti-Pattern 4: Over-Optimizing Before Understanding
- **Problem:** Writing elaborate rules before using the tool enough
- **Fix:** Use the AI for a week first; note where it fails; then write targeted rules

### Anti-Pattern 5: No Version Control
- **Problem:** Rules only on one developer's machine
- **Fix:** Check into git; the file compounds in value as the team refines it

### Anti-Pattern 6: Contradictory Rules
- **Problem:** Global rules conflict with project rules
- **Fix:** Use the hierarchy intentionally; project-specific overrides global

### Anti-Pattern 7: Rules Without Enforcement
- **Problem:** Instructions the AI may ignore under pressure
- **Fix:** Use hooks (Claude Code) or pre-commit checks for zero-exception rules
- Claude Code hooks are deterministic, unlike advisory CLAUDE.md instructions

### Anti-Pattern 8: Mock-Friendly Testing Rules
- **Problem:** Allowing or encouraging mock-based testing
- **Fix:** Explicit "no mocks" policy with real alternatives (Docker test DBs, fixtures, sample data)

---

## 6. Actionable Recommendations for This Repository

### Template CLAUDE.md Should Include

1. One-line project description placeholder
2. Tech stack with specific tool preferences
3. Exact setup and run commands
4. Architecture map section (directory -> purpose)
5. Anti-pattern gallery section (never-do list)
6. Pointer-based rule references (`@.rules/testing.md`)
7. Context file references (`.context/plan.md`, etc.)

### Template .cursorrules/.mdc Should Include

1. `always` rule for core coding standards
2. Language-specific rules with glob patterns (auto-attach)
3. Agent-requested rules with descriptions for situational guidance
4. Separate .mdc files by concern (testing, style, git, security)

### Key Structural Decisions

- **CLAUDE.md:** Under 150 lines, maximum referencing to `.claude/rules/` or `.rules/`
- **Cursor:** One `.mdc` per concern, use frontmatter metadata for smart activation
- **Shared:** Anti-pattern gallery, testing philosophy, commit standards work identically across tools
- **Skills:** Use `.claude/skills/` for reusable workflows (PR review, deployment, etc.)

---

## Sources

- [Anthropic: Best Practices for Claude Code](https://code.claude.com/docs/en/best-practices)
- [Anthropic: Using CLAUDE.md Files](https://claude.com/blog/using-claude-md-files)
- [Anthropic: How Claude Remembers Your Project](https://code.claude.com/docs/en/memory)
- [HumanLayer: Writing a Good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [Builder.io: How to Write a Good CLAUDE.md](https://www.builder.io/blog/claude-md-guide)
- [Builder.io: How I Use Claude Code](https://www.builder.io/blog/claude-code)
- [Trail of Bits: claude-code-config](https://github.com/trailofbits/claude-code-config)
- [steipete/agent-rules](https://github.com/steipete/agent-rules)
- [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice)
- [Cursor: Official Rules Docs](https://docs.cursor.com/context/rules)
- [Cursor: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
- [PromptHub: Top Cursor Rules for Coding Agents](https://www.prompthub.us/blog/top-cursor-rules-for-coding-agents)
- [Elementor: Cursor Rules Best Practices](https://medium.com/elementor-engineers/cursor-rules-best-practices-for-developers-16a438a4935c)
- [Trigger.dev: How to Write Great Cursor Rules](https://trigger.dev/blog/cursor-rules)
- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)
- [rulebook-ai](https://github.com/botingw/rulebook-ai)
- [jsonallen/example-ai-coding-rules](https://github.com/jsonallen/example-ai-coding-rules)
- [Arun Iyer: Instruction Files for AI Coding Assistants](https://aruniyer.github.io/blog/agents-md-instruction-files.html)
- [PostHog: Avoid These AI Coding Mistakes](https://newsletter.posthog.com/p/avoid-these-ai-coding-mistakes)
- [DEV: AI Coding Anti-Patterns](https://dev.to/lingodotdev/ai-coding-anti-patterns-6-things-to-avoid-for-better-ai-coding-f3e)
- [Gend.co: Claude Skills and CLAUDE.md Guide](https://www.gend.co/blog/claude-skills-claude-md-guide)
- [codewithmukesh: CLAUDE.md for .NET Developers](https://codewithmukesh.com/blog/claude-md-mastery-dotnet/)
