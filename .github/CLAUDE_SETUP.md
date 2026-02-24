# Claude Code Action Setup Guide

This repository is configured to use Claude AI for automatic PR reviews and responding to mentions in pull requests.

## Prerequisites

You need to set up the following secrets in your GitHub repository:

### 1. Anthropic API Key

1. Get an API key from [Anthropic Console](https://console.anthropic.com/)
2. Go to your GitHub repository → Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `ANTHROPIC_API_KEY`
5. Value: Your Anthropic API key
6. Click "Add secret"

### 2. GitHub Token

The `GITHUB_TOKEN` is automatically provided by GitHub Actions, so no setup needed!

## Features

### 🤖 Automatic PR Reviews

Claude will automatically review your pull requests when:
- A new PR is opened
- New commits are pushed to an existing PR
- A PR is reopened

**What Claude reviews:**
- Code quality and best practices
- Potential bugs or security issues
- Performance concerns
- Maintainability and readability
- Test coverage

**Excluded from review:**
- Lock files (package-lock.json, yarn.lock, etc.)
- Minified files (*.min.js, *.min.css)
- Build artifacts (dist/, build/, .next/, coverage/)
- Dependencies (node_modules/, __pycache__/)
- Generated files (*.generated.*)

### 💬 Mention Claude in Comments

You can interact with Claude by mentioning `@claude` in PR comments:

**Examples:**

```
@claude review this file
```
Review specific files or sections

```
@claude explain how this authentication works
```
Get explanations of code or logic

```
@claude suggest a better approach for this function
```
Ask for alternative implementations

```
@claude security review
```
Focus on security concerns

```
@claude performance review
```
Focus on performance optimization

```
@claude what are the potential issues here?
```
General questions about the code

## Configuration Files

### `.github/workflows/claude-code-review.yml`
Main workflow for automatic PR reviews (triggered on PR open/update)

### `.github/workflows/claude-pr-assistant.yml`
Workflow that responds to @claude mentions in comments

### `CLAUDE.md`
Project-level instructions and context for Claude. Claude reads this file to understand your:
- Project structure and conventions
- Build and test commands
- Preferred patterns and approaches
- Any special considerations

**Note:** Configuration is done through workflow files and `CLAUDE.md`. The v1.0 action does not use a separate `.github/claude-code.yml` configuration file.

## Customization

### Change Model

Edit the workflow files (`.github/workflows/claude-code-review.yml` or `claude-pr-assistant.yml`):

```yaml
claude_args: |
  --model claude-opus-4-6
```

**Available models (February 2026):**
- `claude-opus-4-6` - Most capable, best for complex reviews (higher cost)
- `claude-sonnet-4-6` - Balanced performance and cost (recommended, default)
- `claude-haiku-4-0` - Fastest and most cost-effective

**Important:** Use full model IDs, not short names like "sonnet" or "opus".

### Adjust Max Conversation Turns

Edit the `claude_args` in workflow files:

```yaml
claude_args: |
  --model claude-sonnet-4-6
  --max-turns 20
```

- **Automatic reviews**: 10 turns is usually sufficient
- **Interactive mentions**: 15-20 turns allows more back-and-forth

### Customize Instructions

Edit the `prompt` parameter in workflow files:

```yaml
prompt: |
  You are reviewing code in a pull request.

  Focus areas:
  - Security vulnerabilities
  - Performance issues
  - Code style consistency

  Our coding standards:
  - Use TypeScript for all new code
  - Write tests for all new features
  - Follow existing naming conventions
```

### Exclude Additional File Patterns

Add exclusion patterns to the `prompt` parameter:

```yaml
prompt: |
  [Your instructions here]

  EXCLUDE these file patterns from review:
  - *.lock
  - dist/**
  - *.test.js (if you don't want test files reviewed)
  - docs/** (if you don't want docs reviewed)
```

### Enable/Disable Fix Links

Edit workflow files:

```yaml
include_fix_links: true   # Enable "Fix this" links (default)
include_fix_links: false  # Disable "Fix this" links
```

When enabled, Claude's comments include clickable "Fix this" links that:
- Open Claude Code CLI with the full PR context
- Pre-load the specific issue that needs fixing
- Let you apply fixes locally with one command

### Custom Branch Prefix for Fixes

Edit workflow files:

```yaml
branch_prefix: "ai-fix/"  # Fix branches will be ai-fix/xxx
branch_prefix: "claude/"  # Fix branches will be claude/xxx (default)
```

### Control Available Tools

Restrict which tools Claude can use:

```yaml
claude_args: |
  --model claude-sonnet-4-6
  --allowedTools "Edit,Read,Grep,Glob"
```

## Advanced Configuration

### Separate Models for Reviews vs Mentions

Use different models for automatic reviews and interactive mentions:

**In claude-code-review.yml:**
```yaml
claude_args: |
  --model claude-sonnet-4-6
  --max-turns 10
```

**In claude-pr-assistant.yml:**
```yaml
claude_args: |
  --model claude-opus-4-6
  --max-turns 20
```

### Custom Trigger Phrase

Change the mention trigger from `@claude` to something else:

**In claude-pr-assistant.yml:**
```yaml
trigger_phrase: "@ai"
trigger_phrase: "hey claude"
```

### Integration with Project Documentation

Add comprehensive instructions to `CLAUDE.md` in your repository root:

```markdown
# CLAUDE.md

## Project Overview
This is a React + TypeScript web application...

## Code Review Focus
When reviewing PRs, pay special attention to:
- API security (we use JWT authentication)
- React hooks usage (follow our custom hooks pattern)
- TypeScript strict mode compliance

## Testing Requirements
All new features must include:
- Unit tests (Jest)
- Integration tests for API endpoints
- E2E tests for critical user flows (Playwright)
```

Claude will automatically read and follow these instructions.

## Migration from Beta Versions

If you're upgrading from a beta version of claude-code-action, note these breaking changes:

### Removed Parameters
These parameters no longer exist in v1.0:
- ❌ `github_token` (automatically provided)
- ❌ `auto_review` (auto-detected from workflow triggers)
- ❌ `respond_to_mentions` (auto-detected from workflow triggers)
- ❌ `model` (use `claude_args: --model` instead)
- ❌ `instructions` (renamed to `prompt`)
- ❌ `exclude_patterns` (include in `prompt` text)
- ❌ `max_files` (removed)
- ❌ `review_level` (removed)

### New v1.0 Parameters
- ✅ `prompt` - Custom instructions (replaces `instructions`)
- ✅ `claude_args` - CLI arguments like `--model`, `--max-turns`
- ✅ `trigger_phrase` - Custom mention trigger (default: "@claude")
- ✅ `include_fix_links` - Enable "Fix this" links (default: true)
- ✅ `branch_prefix` - Prefix for fix branches (default: "claude/")

### Configuration File Changes
- ❌ `.github/claude-code.yml` is **not used** in v1.0 (delete it)
- ✅ Use workflow YAML files for configuration
- ✅ Use `CLAUDE.md` for project-level instructions

## Usage Examples

### Example 1: Basic PR Review
1. Create a PR
2. Claude automatically posts a review with feedback
3. Address the feedback and push changes
4. Claude reviews the updates

### Example 2: Ask Questions
In a PR comment:
```
@claude can you explain why we need this mutex here?
```

### Example 3: Request Specific Review
In a PR comment:
```
@claude please review src/api/auth.js for security issues
```

### Example 4: Get Suggestions
In a PR comment:
```
@claude this function is slow, any suggestions to optimize it?
```

### Example 5: Multi-turn Conversation
```
You: @claude what does this regex do?
Claude: [Explains the regex]
You: @claude can you suggest a simpler alternative?
Claude: [Provides alternative]
You: @claude which approach is more performant?
Claude: [Compares performance]
```

## Troubleshooting

### Claude isn't responding

1. **Check API key:** Verify `ANTHROPIC_API_KEY` is set in repository secrets
2. **Check workflow runs:** Go to Actions tab and see if workflows are running
3. **Check trigger:** Ensure you're mentioning `@claude` (lowercase) in comments
4. **Check branch:** Verify PR is targeting the correct branch (usually `main`)
5. **Check logs:** Click on failed workflow runs to see error messages

### "Parameter not recognized" errors

You're likely using beta parameters that don't exist in v1.0. See "Migration from Beta Versions" above.

### "Claude Code process exited with code 1"

Common causes:
- Invalid model name (use full IDs like `claude-sonnet-4-6`, not "sonnet")
- Invalid `claude_args` syntax
- Missing required permissions in workflow file
- API key issues

Check the workflow logs for specific error messages.

### Reviews are too verbose

Use a more concise prompt:

```yaml
prompt: |
  Review this PR. Be concise. Only comment on significant issues.
  Focus on: bugs, security, performance.
  Skip: minor style issues, nitpicks.
```

Or switch to a faster model:

```yaml
claude_args: |
  --model claude-haiku-4-0
```

### Want to disable automatic reviews

Comment out or remove the `claude-code-review.yml` workflow file. Keep only `claude-pr-assistant.yml` for @claude mentions.

### Permission denied errors

Ensure workflow has correct permissions:

```yaml
permissions:
  contents: write
  pull-requests: write
  issues: write
  actions: read
  id-token: write
```

## Cost Considerations

- Each PR review costs API credits based on code size and model used
- Mention responses are billed per interaction
- Costs scale with `--max-turns` (more turns = more API calls)
- Consider using `claude-haiku-4-0` for cost savings on simple reviews
- Monitor usage in [Anthropic Console](https://console.anthropic.com/)

**Estimated costs (varies by PR size):**
- Small PR (~100 lines): $0.01-0.05
- Medium PR (~500 lines): $0.05-0.25
- Large PR (~1000 lines): $0.25-0.50

Using Opus instead of Sonnet increases costs by ~3-5x.

## Privacy & Security

- Claude processes your code through Anthropic's API
- Code is not used to train models (per Anthropic's policy)
- See [Anthropic's Privacy Policy](https://www.anthropic.com/privacy)
- Sensitive files can be excluded via `prompt` exclusion patterns
- Consider security implications before enabling on repos with secrets
- Never commit API keys to the repository - use GitHub secrets

## Support & Resources

- [Claude Code Action Repository](https://github.com/anthropics/claude-code-action)
- [Claude Code Documentation](https://code.claude.com/docs/en/github-actions)
- [Anthropic API Documentation](https://docs.anthropic.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- Report issues in this repository's Issues tab

## Tips for Better Reviews

1. **Keep PRs focused:** Smaller PRs get better, more focused reviews
2. **Write good descriptions:** PR descriptions help Claude understand context
3. **Use CLAUDE.md:** Add project context to help Claude give better feedback
4. **Be specific with mentions:** "@claude review auth.js for SQL injection" is better than "@claude review this"
5. **Iterate:** Claude learns from the conversation - ask follow-up questions
6. **Combine with human review:** Claude augments, doesn't replace, human reviewers
