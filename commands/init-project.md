---
description: Initialize a new project with vibe-rules templates for Claude
argument-hint: <project-description>
allowed-tools: Bash(echo:*), Bash(pwd:*), Bash(ls:*), Bash(rm:*), Bash(git:*), Bash(cp:*), Bash(mkdir .context), Bash(mkdir .rules), Bash(chmod:*), Bash(grep -q:*), Bash(cat:*), Bash(test:*), Bash([:*), Write, Edit, Read
---

# Project Initialization with Vibe Rules

I'll initialize this project following the vibe-rules-templates structure from https://github.com/neuromechanist/vibe-rules-templates.

## Setup Process:

### 1. Analyze Current Project
!echo "Project description: $ARGUMENTS"
!pwd
!ls -la

### 2. Download Templates from GitHub
!rm -rf /tmp/vibe-rules-templates 2>/dev/null || true
!git clone https://github.com/neuromechanist/vibe-rules-templates.git /tmp/vibe-rules-templates

### 3. Copy Claude Templates (with safety checks)

#### CLAUDE.md (if doesn't exist)
!if [ ! -f "CLAUDE.md" ]; then cp /tmp/vibe-rules-templates/claude-templates/CLAUDE.md ./CLAUDE.md && echo "✓ Created CLAUDE.md"; else echo "⚠ CLAUDE.md already exists, skipping"; fi

#### .rules directory (if doesn't exist)
!if [ ! -d ".rules" ]; then cp -r /tmp/vibe-rules-templates/claude-templates/.rules ./.rules && echo "✓ Created .rules directory"; else echo "⚠ .rules directory already exists, skipping"; fi

#### .context directory (if doesn't exist)
!if [ ! -d ".context" ]; then cp -r /tmp/vibe-rules-templates/shared/.context-template ./.context && echo "✓ Created .context directory"; else echo "⚠ .context directory already exists, skipping"; fi

### 4. Update .gitignore (append if doesn't contain Claude-specific files)
!grep -q "CLAUDE.md" .gitignore 2>/dev/null || echo "\n# Claude specific files\nCLAUDE.md" >> .gitignore
!grep -q ".rules" .gitignore 2>/dev/null || echo ".rules/" >> .gitignore

### 5. Python-specific setup (if Python project detected)
!if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ] || [ -f "setup.py" ]; then \
  if [ ! -f ".git/hooks/pre-commit" ]; then \
    cp /tmp/vibe-rules-templates/shared/config/pre-commit .git/hooks/pre-commit && \
    chmod +x .git/hooks/pre-commit && \
    echo "✓ Installed pre-commit hooks with ruff"; \
  else \
    echo "⚠ Pre-commit hook already exists"; \
  fi; \
fi

### 6. Customize CLAUDE.md based on project context
Now I'll analyze your project and customize the CLAUDE.md file to:
- Replace template placeholders ({{PROJECT_NAME}}, {{ENV_NAME}}, {{TECH_STACK}})
- Add project-specific instructions
- Document what's in the .context and .rules directories
- Reference any existing planning documents if you have them
- Update the contents of .context/ and /.rules with the project requirements.
- If some rules are not needed, remove them.
- If some contexts are not yet used, keep minimal instructions how to use the specific context file.
- Re-read CLAUDE.md to ensure only relevant context and rules are being referenced.

!echo "\n=== Analyzing project structure ===\n"
!ls -la
!if [ -f "package.json" ]; then echo "Detected: Node.js/JavaScript project"; fi
!if [ -f "requirements.txt" ] || [ -f "pyproject.toml" ] || [ -f "setup.py" ]; then echo "Detected: Python project"; fi
!if [ -f "Cargo.toml" ]; then echo "Detected: Rust project"; fi
!if [ -f "go.mod" ]; then echo "Detected: Go project"; fi

### 7. Verify initialization
!echo "\n=== Initialization Complete ===\n"
!echo "Claude structure created:"
!ls -la CLAUDE.md .rules/ .context/ 2>/dev/null || true

### 8. Clean up
!rm -rf /tmp/vibe-rules-templates

Let me now help you customize the CLAUDE.md file to properly document:
- Project-specific goals and instructions
- What's contained in the .context directory (documentation, specs, examples)
- What rules are defined in the .rules directory
- References to any planning documents you maintain separately
