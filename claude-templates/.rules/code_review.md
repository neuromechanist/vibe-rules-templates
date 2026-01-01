# Code Review Standards

## PR Review Toolkit
When the `pr-review-toolkit` plugin is available, use it after creating PRs to catch issues before merge.

### Available Agents
- `code-reviewer` - Review for style, best practices, project guidelines
- `silent-failure-hunter` - Find inadequate error handling, silent failures
- `code-simplifier` - Simplify code while preserving functionality
- `comment-analyzer` - Check comment accuracy and maintainability
- `pr-test-analyzer` - Review test coverage quality
- `type-design-analyzer` - Analyze type design and invariants

### Workflow
1. Create PR with `gh pr create`
2. Run code review agent on the changes
3. Address critical findings before requesting human review
4. Document any intentionally skipped suggestions

## Manual Code Review Checklist

### Before Committing
- [ ] Code compiles without warnings
- [ ] Tests pass
- [ ] No debug code left (print statements, TODO hacks)
- [ ] No sensitive data in code or logs

### Logic & Safety
- [ ] Error cases handled appropriately
- [ ] No silent failures (empty catch blocks)
- [ ] Thread safety for shared state
- [ ] Resource cleanup (files, connections, memory)

### Code Quality
- [ ] Functions do one thing
- [ ] Clear naming (no abbreviations)
- [ ] No magic numbers (use constants)
- [ ] Comments explain "why", not "what"

### Swift Specific
- [ ] Use `#if DEBUG` for debug-only code
- [ ] Prefer `let` over `var`
- [ ] Use `guard` for early returns
- [ ] Handle optionals safely (no force unwrap in production)

## Review Comments
When leaving review comments:
- Be specific about the issue
- Suggest a fix when possible
- Distinguish blocking vs. non-blocking issues
- Reference documentation or examples

---
*Review early, review often, catch issues before they ship.*
