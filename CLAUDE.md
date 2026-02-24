# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Status

This is a new/empty project directory. As the codebase develops, update this file with:
- Build, test, and development commands
- Architecture and code organization patterns
- Project-specific conventions and workflows

## GitHub Workflows

This repository includes automated PR review workflows:

### Claude Code Action
- **Automatic PR reviews**: Claude reviews all PRs automatically
- **Interactive assistance**: Mention `@claude` in PR comments to ask questions
- **Setup required**: Add `ANTHROPIC_API_KEY` to repository secrets
- See `.github/CLAUDE_SETUP.md` for complete setup instructions

### PR Review Checks
- PR title validation (conventional commits)
- Large file detection
- Secret scanning
- Auto-labeling based on changed files
- PR size checking

## Additional details