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
Main workflow for automatic PR reviews

### `.github/workflows/claude-pr-assistant.yml`
Workflow that responds to @claude mentions

### `.github/claude-code.yml`
Configuration for Claude's behavior:
- Review settings (level, max files, etc.)
- Focus areas (security, performance, etc.)
- Exclude patterns
- Comment behavior
- Custom instructions

## Customization

### Adjust Review Level

Edit `.github/claude-code.yml`:

```yaml
review:
  level: quick    # Options: quick, focused, full
```

### Exclude Files from Review

Edit `.github/claude-code.yml`:

```yaml
exclude:
  patterns:
    - "*.lock"
    - "dist/**"
    # Add your patterns
```

### Change Model

Edit the workflow files to use a different model:

```yaml
model: opus     # Options: opus, sonnet, haiku
```

- **opus**: Most capable, best for complex reviews
- **sonnet**: Balanced performance and cost (recommended)
- **haiku**: Fastest and most cost-effective

### Custom Instructions

Edit `.github/claude-code.yml` to customize Claude's behavior:

```yaml
instructions: |
  Your custom instructions here...
```

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

## Troubleshooting

### Claude isn't responding

1. Check that `ANTHROPIC_API_KEY` is set in repository secrets
2. Verify the workflow runs in Actions tab
3. Ensure you're mentioning `@claude` (lowercase) in comments
4. Check that the PR is in the correct branch (usually `main`)

### Reviews are too detailed

Adjust the review level to `quick` in `.github/claude-code.yml`

### Want to disable auto-reviews

Edit `.github/workflows/claude-code-review.yml`:
```yaml
auto_review: false
```

## Cost Considerations

- Each PR review costs API credits based on code size
- Mention responses are billed per interaction
- Consider using `haiku` model for cost savings
- Set `max_files` and `max_lines` limits to control costs

## Privacy & Security

- Claude processes your code through Anthropic's API
- See [Anthropic's Privacy Policy](https://www.anthropic.com/privacy)
- Sensitive files can be excluded via `.github/claude-code.yml`
- Consider security implications before enabling on private repos

## Support

- [Claude Code Documentation](https://github.com/anthropics/claude-code-action)
- [Anthropic API Docs](https://docs.anthropic.com/)
- Report issues in this repository's Issues tab
