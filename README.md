# AI Experiments

This repository demonstrates comprehensive GitHub Actions workflows for automated code review and quality checks, powered by Claude AI.

## Features

### 🤖 AI-Powered Code Reviews
- **Automatic PR Reviews**: Claude automatically analyzes pull requests for code quality, security, performance, and best practices
- **Interactive Assistant**: Mention `@claude` in PR comments to get explanations, suggestions, and focused reviews
- **Smart Context**: Claude understands your codebase through `CLAUDE.md` instructions

### ✅ Automated Quality Checks
- **PR Title Validation**: Enforces conventional commit format
- **Secret Scanning**: Detects accidentally committed API keys, passwords, and tokens
- **Large File Detection**: Warns about files over 5MB
- **PR Size Analysis**: Categorizes PRs by size and suggests splitting large ones
- **Auto-Labeling**: Automatically applies labels based on changed files

## Quick Start

### Prerequisites
1. GitHub repository with Actions enabled
2. Anthropic API key ([Get one here](https://console.anthropic.com/))

### Setup

1. **Add your Anthropic API key:**
   - Go to: Settings → Secrets and variables → Actions
   - Add secret: `ANTHROPIC_API_KEY`
   - Value: Your Anthropic API key

2. **That's it!** The workflows are already configured and ready to use.

### Using Claude for Code Reviews

**Automatic reviews** happen when you:
- Open a new pull request
- Push new commits to an existing PR
- Reopen a PR

**Interactive assistance** with `@claude` mentions:
```
@claude review this file
@claude explain how this authentication works
@claude suggest a better approach for this function
@claude security review
@claude performance review
```

## Workflows

### Claude Code Action
- **File**: `.github/workflows/claude-code-review.yml`
- **Triggers**: PR opened, synchronized, reopened
- **Model**: claude-sonnet-4-6
- **Features**: Automatic code reviews with fix links

### Claude PR Assistant
- **File**: `.github/workflows/claude-pr-assistant.yml`
- **Triggers**: Comments containing `@claude`
- **Model**: claude-sonnet-4-6
- **Features**: Interactive Q&A and focused reviews

### PR Review Checks
- **File**: `.github/workflows/pr-review.yml`
- **Features**: Title validation, secret scanning, large file detection

### PR Size Checker
- **File**: `.github/workflows/pr-size-checker.yml`
- **Features**: Analyzes PR size and suggests splitting large PRs

### Auto-Labeler
- **Config**: `.github/labeler.yml`
- **Features**: Automatically labels PRs based on file changes

## Configuration

### Customize Claude's Behavior

Edit `CLAUDE.md` to provide project-specific context and conventions:
```markdown
## Project Overview
This is a [description of your project]...

## Code Review Focus
When reviewing PRs, pay special attention to:
- [Your specific concerns]
```

### Change AI Model

Edit workflow files to use a different model:
```yaml
claude_args: |
  --model claude-opus-4-6    # Most capable
  --model claude-sonnet-4-6  # Balanced (default)
  --model claude-haiku-4-0   # Fastest
```

### Adjust Review Depth

Modify the `--max-turns` parameter:
```yaml
claude_args: |
  --model claude-sonnet-4-6
  --max-turns 20  # More thorough reviews
```

## Project Structure

```
.
├── .github/
│   ├── workflows/
│   │   ├── claude-code-review.yml    # Automatic PR reviews
│   │   ├── claude-pr-assistant.yml   # @claude mentions
│   │   ├── pr-review.yml             # Quality checks
│   │   ├── pr-size-checker.yml       # PR size analysis
│   │   └── labeler.yml               # Auto-labeling config
│   ├── labeler.yml                   # Label configuration
│   └── CLAUDE_SETUP.md               # Detailed setup guide
├── CLAUDE.md                         # Project instructions for Claude
└── README.md                         # This file
```

## Development Workflow

1. **Create a branch** from `main`
2. **Make your changes** and commit
3. **Open a pull request** → Workflows run automatically:
   - Claude reviews your code
   - PR checks validate title and scan for issues
   - PR size is analyzed
   - Labels are auto-applied
4. **Interact with Claude** by mentioning `@claude` in comments
5. **Address feedback** and push changes
6. **Merge** when approved and checks pass

## Commit Convention

Follow [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types:**
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code style/formatting (no logic change)
- `refactor` - Code restructuring
- `test` - Adding or updating tests
- `chore` - Maintenance tasks
- `perf` - Performance improvements

**Examples:**
```
feat(auth): add OAuth2 login flow
fix(api): resolve timeout issue in user endpoint
docs(readme): update installation instructions
```

## Cost Considerations

Claude API usage is billed based on:
- **Model used**: Opus > Sonnet > Haiku
- **Code size**: Larger PRs cost more
- **Conversation length**: More turns = higher cost

**Estimated costs per PR:**
- Small (~100 lines): $0.01-0.05
- Medium (~500 lines): $0.05-0.25
- Large (~1000 lines): $0.25-0.50

Monitor usage at [Anthropic Console](https://console.anthropic.com/)

## Documentation

- **[CLAUDE_SETUP.md](.github/CLAUDE_SETUP.md)** - Complete setup and configuration guide
- **[CLAUDE.md](CLAUDE.md)** - Project-specific instructions for Claude
- **[Workflow Documentation](CLAUDE.md#github-workflows)** - Detailed workflow information

## Resources

- [Claude Code Action](https://github.com/anthropics/claude-code-action)
- [Claude Code Documentation](https://code.claude.com/docs/en/github-actions)
- [Anthropic API Documentation](https://docs.anthropic.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

## Tips for Better Reviews

1. **Keep PRs focused** - Smaller PRs get better reviews
2. **Write descriptive PR descriptions** - Help Claude understand context
3. **Use CLAUDE.md** - Add project context for better feedback
4. **Be specific with @claude** - "Review auth.js for SQL injection" > "Review this"
5. **Iterate** - Ask follow-up questions
6. **Combine with human review** - Claude augments, doesn't replace reviewers

## Troubleshooting

### Claude isn't responding
1. Check `ANTHROPIC_API_KEY` is set in repository secrets
2. Verify workflows are enabled in Actions tab
3. Ensure you're using `@claude` (lowercase)
4. Check workflow logs for errors

### "Parameter not recognized" errors
Update workflows to v1.0 parameters. See [CLAUDE_SETUP.md](.github/CLAUDE_SETUP.md#migration-from-beta-versions)

### Reviews are too verbose
- Switch to `claude-haiku-4-0` for faster, more concise reviews
- Adjust the `prompt` to be more specific about what to focus on

## License

This is an experimental repository demonstrating GitHub Actions workflows with Claude AI.

## Contributing

Feel free to:
- Open issues for bugs or suggestions
- Submit PRs to improve workflows
- Share your own workflow configurations

---

**Note**: This repository uses Claude Code Action v1.0. Configuration is done through workflow files and `CLAUDE.md`, not through a separate `.github/claude-code.yml` file.
