# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

This is a new/empty project directory. As the codebase develops, update this file with:
- Build, test, and development commands
- Architecture and code organization patterns
- Project-specific conventions and workflows

## GitHub Workflows

This repository has comprehensive automated PR review and quality checks.

### 🤖 Claude Code Action (AI-Powered Reviews)

**Files:**
- `.github/workflows/claude-code-review.yml` - Automatic PR reviews
- `.github/workflows/claude-pr-assistant.yml` - Interactive @claude mentions
- `.github/CLAUDE_SETUP.md` - Complete setup guide
- `CLAUDE.md` - Project-level instructions for Claude (this file)

**Note:** Configuration is done through workflow files and this CLAUDE.md file. The v1.0 action does not use a separate `.github/claude-code.yml` configuration file.

**Features:**
- **Automatic PR Reviews**: Claude analyzes PRs for code quality, security, performance, and best practices
- **Interactive Assistant**: Mention `@claude` in PR comments to:
  - Review specific files: `@claude review src/auth.js`
  - Explain code: `@claude explain this function`
  - Get suggestions: `@claude suggest a better approach`
  - Security focus: `@claude security review`
  - Performance focus: `@claude performance review`

**Setup Required:**
1. Add `ANTHROPIC_API_KEY` to repository secrets
2. Go to: Settings → Secrets and variables → Actions
3. See `.github/CLAUDE_SETUP.md` for detailed instructions

**Configuration:**
- Model: `claude-sonnet-4-6` (configured via `claude_args: --model`)
- Max turns: 10 for automatic reviews, 15 for interactive mentions
- Excludes: lock files, minified files, generated files, dist/build folders, dependencies
- Focus areas: code quality, security, performance, tests, documentation
- Fix links: Enabled (clickable "Fix this" links in review comments)
- Branch prefix: `claude/` for automated fix branches

### ✅ Standard PR Review Checks

**File:** `.github/workflows/pr-review.yml`

**Checks:**
- **PR Title Validation**: Enforces conventional commit format
  - Format: `type(scope): description`
  - Examples: `feat(auth): add login`, `fix(api): resolve timeout`
- **Large File Detection**: Warns about files >5MB
- **Secret Scanning**: Basic pattern matching for API keys, passwords, tokens
- **Tech Stack Support**: Templates for Node.js, Python, Go, Rust (commented, customize as needed)

**Customization:**
Uncomment and configure sections for your tech stack in the workflow file.

### 📏 PR Size Checker

**File:** `.github/workflows/pr-size-checker.yml`

**Features:**
- Analyzes total lines changed (additions + deletions)
- Size categories: XS (<50), S (<200), M (<500), L (<1000), XL (>1000)
- Automatically comments on large PRs (>500 lines) with suggestions to split
- Visual indicators: 🟢 Small, 🟡 Medium, 🟠 Large, 🔴 Very Large

### 🏷️ Auto-Labeling

**Files:**
- `.github/labeler.yml` - Label configuration

**Auto-applied Labels:**
- `documentation` - Changes to `.md` files or `docs/` folder
- `dependencies` - Updates to package.json, requirements.txt, Cargo.toml, go.mod
- `ci/cd` - Changes to `.github/` workflows
- `configuration` - Changes to config files (yml, yaml, json, toml)

**Customization:**
Add project-specific labels in `labeler.yml` for frontend, backend, tests, etc.

## Workflow Triggers

All workflows trigger on:
- Pull requests targeting `main` branch
- PR events: opened, synchronize (new commits), reopened
- Comments: @claude mentions

## Development Workflow

1. **Create a branch** from `main`
2. **Make changes** and commit
3. **Open a PR** → Workflows run automatically:
   - Claude reviews your code
   - PR checks validate title, scan for secrets, check file sizes
   - PR size is analyzed
   - Labels are auto-applied
4. **Interact with Claude** by mentioning `@claude` in comments
5. **Address feedback** and push changes
6. **Merge** when approved and checks pass

## Commit Message Convention

Follow conventional commits format for PR titles:
- `feat(scope): description` - New feature
- `fix(scope): description` - Bug fix
- `docs(scope): description` - Documentation
- `style(scope): description` - Formatting, no code change
- `refactor(scope): description` - Code restructuring
- `test(scope): description` - Adding/updating tests
- `chore(scope): description` - Maintenance tasks
- `perf(scope): description` - Performance improvements

## Additional details