# Rule Templates - Failed Attempts & Lessons

## Failed Attempts Log

### Attempt: Single template for both tools
**Date:** Early development
**Goal:** One set of rules for both Cursor and Claude

#### Issue Encountered:
- Cursor works best with modular .mdc files
- Claude needs concise inline instructions
- Different reference styles (.mdc vs .md)

#### Lesson Learned:
Tools have different strengths - optimize for each rather than compromise

#### Alternative Solution:
Dual-template system with shared components

---

### Attempt: Very verbose CLAUDE.md (>300 lines)
**Date:** Initial Claude template design
**Goal:** Include all details inline

#### Issue Encountered:
- Too long to scan quickly
- Diluted critical information
- Hard to find essential rules

#### Lesson Learned:
Conciseness is crucial for Claude - <150 lines with references

#### Alternative Solution:
Brief inline + detailed .rules/ directory

---

### Attempt: Using .project directory name
**Date:** During .context planning
**Goal:** Consolidate workflow docs in .project/

#### Issue Encountered:
- Conflicts with Eclipse IDE's .project file
- Would cause confusion for Eclipse users

#### Lesson Learned:
Always check for existing tool conventions

#### Alternative Solution:
Used .context/ instead - clear purpose, no conflicts

---

## Common Pitfalls

### Pitfall: Forgetting NO MOCK principle
**Symptoms:** Examples with mock objects in templates
**Solution:** Review all examples for real data usage

### Pitfall: Inconsistent rules between versions
**Symptoms:** Cursor and Claude templates diverge
**Solution:** Maintain shared principles, tool-specific implementation

## Lessons Summary

### Key Learnings
1. Tool optimization > universal compromise
2. Conciseness critical for Claude (<150 lines)
3. Check for naming conflicts with existing tools
4. NO MOCKS must be enforced everywhere
5. Attention flags should be sparse and meaningful

### Things to Check First Next Time
- [ ] Existing tool conventions for names
- [ ] Line count for Claude templates
- [ ] Mock examples in any code
- [ ] Consistency between template versions