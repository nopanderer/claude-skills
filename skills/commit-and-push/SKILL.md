---
name: commit-and-push
description: Automatically analyze git changes, generate Conventional Commits compliant commit messages, commit changes, and push to remote in a single automated workflow. Use when the user invokes /commit-and-push or explicitly requests to automatically commit and push changes following Conventional Commits format (feat:, fix:, docs:, etc.). This skill provides a fully automated git workflow without manual intervention.
---

# Commit and Push

## Overview

Automate the entire git workflow: analyze changes → generate commit message → commit → push to remote. All commit messages follow the Conventional Commits specification for consistency and tooling compatibility.

## Automated Workflow

Execute these steps sequentially and automatically without user confirmation:

### 1. Analyze Current State

Run in parallel:
- `git status` - See all untracked and modified files
- `git diff` - See both staged and unstaged changes
- `git diff --cached` - See already staged changes
- `git log -5 --oneline` - Review recent commit message style

### 2. Generate Commit Message

Based on the analysis, create a commit message following Conventional Commits format:

**Structure:**
```
<type>[optional scope]: <description>

[optional body]

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Type Selection:**
- `feat`: New feature or functionality
- `fix`: Bug fix
- `docs`: Documentation changes only
- `style`: Code style/formatting (no logic change)
- `refactor`: Code restructuring (no feature/bug change)
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `build`: Build system or dependencies
- `ci`: CI/CD configuration changes
- `chore`: Other changes (tooling, config, etc.)

**Guidelines:**
- Description: Present tense, lowercase, no period at end
- Keep description under 72 characters
- Use scope when changes are focused on specific module/component
- Use `!` after type/scope or `BREAKING CHANGE:` footer for breaking changes
- Body: Optional, explain "why" not "what", wrap at 72 characters

**Examples:**
- `feat(auth): add OAuth2 login support`
- `fix: resolve memory leak in image processing`
- `docs: update API usage examples`
- `refactor(api)!: change response format to JSON`

### 3. Stage and Commit

**CRITICAL - Commit Safety:**
- Stage relevant files explicitly by name (avoid `git add -A` or `git add .`)
- NEVER stage sensitive files (.env, credentials, secrets, etc.)
- If sensitive files detected, warn user and skip them
- Create NEW commit (never use `--amend` unless explicitly requested)
- If pre-commit hook fails, fix issue and create NEW commit (not amend)

Execute:
```bash
# Stage specific files
git add [file1] [file2] ...

# Commit with message using HEREDOC for proper formatting
git commit -m "$(cat <<'EOF'
<type>[scope]: <description>

[optional body]

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
EOF
)"
```

### 4. Push to Remote

After successful commit:
```bash
# Push to remote (use -u if first push on new branch)
git push

# Or if new branch
git push -u origin <branch-name>
```

### 5. Verify

Run `git status` to confirm everything is clean and pushed.

## Error Handling

If any step fails:
- **Commit fails due to hook**: Fix the issue, re-stage, create NEW commit (not amend)
- **Push fails (no upstream)**: Use `git push -u origin <branch-name>`
- **Push fails (diverged)**: STOP and inform user - do NOT force push
- **Sensitive files detected**: STOP, warn user, exclude files

## Reference

For complete Conventional Commits specification, see [references/conventional-commits.md](references/conventional-commits.md).
