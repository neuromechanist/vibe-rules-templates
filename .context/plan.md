# Implementation Plan: Dual Template System

## Phase 1: Structure Reorganization
- [ ] Create issue for dual template system
- [ ] Create feature branch
- [ ] Rename templates/ to cursor-templates/
- [ ] Create claude-templates/ directory
- [ ] Create shared/ directory for common elements
- [ ] Move common configs to shared/

## Phase 2: Cursor Templates
- [ ] Create .cursorrules main file
- [ ] Move rules to .rules/ directory as MDC files
- [ ] Update references and paths
- [ ] Test with Cursor

## Phase 3: Claude Templates  
- [ ] Create CLAUDE.md from cursor_rules.mdc
- [ ] Embed critical rules (NO MOCKS, commits, etc.)
- [ ] Convert MDC rules to markdown in .rules/
- [ ] Add references to detailed rules
- [ ] Add instructions for customization

## Phase 4: Planning Templates
- [ ] Make plan-based (simple) the default
- [ ] Move TaskMaster to advanced/ subdirectory
- [ ] Update READMEs to reflect hierarchy
- [ ] Add migration guide between approaches

## Phase 5: Documentation
- [ ] Create migration guide (Cursor ↔ Claude)
- [ ] Update main README with new structure
- [ ] Add setup instructions for each tool
- [ ] Create comparison table

## Phase 6: Testing & Refinement
- [ ] Test cursor-templates with Cursor
- [ ] Test claude-templates with Claude
- [ ] Gather feedback
- [ ] Refine based on usage

## Commit Strategy
Each task = one atomic commit:
- `feat: create dual template structure`
- `refactor: reorganize into cursor-templates`
- `feat: add claude-templates with CLAUDE.md`
- `feat: create shared components directory`
- `docs: add migration guides`
- etc.

## Notes
- Keep backwards compatibility where possible
- Document breaking changes clearly
- Ensure .context/ structure works in both