# Claude Code Template Usage Guide

This guide explains how to use the markdown templates with Claude Code for the ecowitt2mqtt project.

## Overview

You now have 5 markdown files that work together to provide Claude with context and structure:

1. **`claude.md`** - Base configuration (always active)
2. **`#new-feature.md`** - Feature development template
3. **`#issue-fix.md`** - Bug fix template
4. **`#test.md`** - Test development template
5. **`#reviewpr.md`** - Pull request review template

## File Structure

Place these files in your project root:

```
ecowitt2mqtt/
├── claude.md                    # Always read by Claude Code
├── #new-feature.md              # Feature development template
├── #issue-fix.md                # Bug fix template
├── #test.md                     # Test development template
├── #reviewpr.md                 # PR review template
├── ecowitt2mqtt/               # Source code
├── tests/                      # Test code
└── ... other project files
```

## How Claude Code Works

### Base Configuration (claude.md)

**Automatically loaded** - Claude Code reads `claude.md` every time you interact with it. This file contains:
- Project architecture and technology stack
- Coding standards and conventions
- Testing requirements (100% coverage)
- Development workflow
- Common patterns

You **don't need to reference this file** - it's always active.

### Task-Specific Templates (#-prefixed files)

These are **invoked explicitly** when you need specialized instructions. Use the `#` prefix in your prompts:

```bash
# Example: Start a new feature
claude-code --prompt "#new-feature" "Add support for new sensor type"

# Example: Fix a bug
claude-code --prompt "#issue-fix" "Fix temperature conversion bug"

# Example: Write tests
claude-code --prompt "#test" "Create tests for battery management module"

# Example: Review a PR
claude-code --prompt "#reviewpr" "Review PR #123"
```

## Usage Scenarios

### Scenario 1: Adding a New Feature

**Command**:
```bash
claude-code --prompt "#new-feature" "Add support for multiple MQTT brokers"
```

**What happens**:
1. Claude reads `claude.md` (project context)
2. Claude reads `#new-feature.md` (feature template)
3. Claude asks you to fill out the template sections
4. You provide requirements and specifications
5. Claude develops the feature completely with:
   - Full implementation
   - Comprehensive tests (100% coverage)
   - Documentation updates
   - Configuration examples

**Interactive Flow**:
- Claude will ask questions to fill out the template
- You make decisions on requirements and priorities
- Claude implements according to the filled template
- Result: Complete, well-tested feature ready for PR

### Scenario 2: Fixing a Bug

**Command**:
```bash
claude-code --prompt "#issue-fix" "Fix: Weather data not parsing when PASSKEY is missing"
```

**What happens**:
1. Claude reads `claude.md` (project context)
2. Claude reads `#issue-fix.md` (bug fix template)
3. Claude helps you document the bug thoroughly
4. Claude implements the fix with:
   - Root cause analysis
   - Fix implementation
   - Regression test
   - Documentation update

**Interactive Flow**:
- Claude asks for reproduction steps and evidence
- You provide logs, error messages, configuration
- Claude analyzes root cause
- Claude implements fix with regression test
- Result: Bug fixed, tested, documented

### Scenario 3: Writing Tests

**Command**:
```bash
claude-code --prompt "#test" "Write tests for ecowitt2mqtt/config.py module"
```

**What happens**:
1. Claude reads `claude.md` (testing standards)
2. Claude reads `#test.md` (test template)
3. Claude analyzes the module to be tested
4. Claude creates comprehensive test suite:
   - Unit tests for all functions
   - Edge case coverage
   - Error handling tests
   - Achieves 100% coverage

**Result**: Complete test suite with 100% coverage

### Scenario 4: Reviewing Your Own Code (Self-Review)

**Before creating a PR**, use this to self-review:

**Command**:
```bash
claude-code --prompt "#reviewpr" "Review my changes before I submit PR"
```

**What happens**:
1. Claude reads `claude.md` (coding standards)
2. Claude reads `#reviewpr.md` (review checklist)
3. Claude reviews your code against all quality criteria
4. Claude provides:
   - Code quality feedback
   - Test coverage check
   - Documentation completeness check
   - Security review
   - Suggestions for improvement

**Result**: High-confidence that your PR will pass review

## Combining Templates

You can reference multiple templates in one command:

```bash
# Develop feature with testing focus
claude-code --prompt "#new-feature #test" "Add health check endpoint with full test coverage"

# Fix bug and ensure proper testing
claude-code --prompt "#issue-fix #test" "Fix MQTT reconnection bug and add comprehensive tests"
```

## Template Customization

### When to Edit Templates

**Edit `claude.md` when**:
- Project structure changes
- New dependencies are added
- Coding standards evolve
- New patterns emerge in the codebase

**Edit task templates when**:
- You want to add new sections
- You find missing information
- You want to customize the workflow
- You need project-specific steps

### How to Customize

All templates are markdown files - just edit them like documentation:

```markdown
## Add your own section

**Your custom instructions**:
- Specific requirement for your project
- Special consideration
- Project-specific pattern
```

## Best Practices

### 1. Always Start with Requirements

When using `#new-feature`, take time to fill out the template thoroughly:
- Define all must-have requirements
- Identify edge cases upfront
- Specify acceptance criteria clearly
- Think through testing strategy

**Result**: Feature developed correctly the first time

### 2. Document Bugs Thoroughly

When using `#issue-fix`, provide complete information:
- Exact reproduction steps
- Full error logs (redact sensitive data)
- Environment details
- What you've tried already

**Result**: Faster, more accurate fixes

### 3. Request Specific Tests

When using `#test`, be specific:
- "Test the happy path"
- "Test error handling"
- "Test edge cases for empty input"
- "Test async timeout scenarios"

**Result**: Comprehensive test coverage

### 4. Self-Review Before PR

Always use `#reviewpr` before submitting:
- Catches issues early
- Ensures consistency
- Validates test coverage
- Confirms documentation

**Result**: Higher quality PRs, faster approval

## Common Commands

### Quick Reference

```bash
# New Feature
claude-code --prompt "#new-feature" "Description of feature"

# Bug Fix
claude-code --prompt "#issue-fix" "Description of bug"

# Write Tests
claude-code --prompt "#test" "Module or feature to test"

# Review Code
claude-code --prompt "#reviewpr" "What to review"

# Combine templates
claude-code --prompt "#new-feature #test" "Feature with tests"
```

## Workflow Examples

### Complete Feature Development Workflow

```bash
# 1. Design feature with requirements
claude-code --prompt "#new-feature" "Add Prometheus metrics endpoint"
# → Work with Claude to fill out requirements

# 2. Develop the feature
# → Claude implements based on requirements

# 3. Ensure tests are comprehensive
claude-code --prompt "#test" "Review test coverage for metrics endpoint"
# → Claude ensures 100% coverage

# 4. Self-review before PR
claude-code --prompt "#reviewpr" "Review metrics endpoint implementation"
# → Claude provides feedback

# 5. Create PR with confidence
git checkout -b feature/prometheus-metrics
git add .
git commit -m "feat: Add Prometheus metrics endpoint"
git push origin feature/prometheus-metrics
# Create PR via GitHub
```

### Bug Fix Workflow

```bash
# 1. Document the bug
claude-code --prompt "#issue-fix" "Weather station data corrupted on network timeout"
# → Work with Claude to document thoroughly

# 2. Fix is implemented with regression test
# → Claude implements fix + test

# 3. Verify the fix
poetry run pytest --cov=ecowitt2mqtt tests/
# → Confirm test passes and coverage maintained

# 4. Self-review
claude-code --prompt "#reviewpr" "Review timeout handling fix"
# → Claude validates quality

# 5. Create PR
git checkout -b bugfix/network-timeout
git add .
git commit -m "fix: Handle network timeouts gracefully"
git push origin bugfix/network-timeout
```

## Troubleshooting

### Claude doesn't seem to follow the template

**Solution**: Make sure you're using the exact filename with `#`:
- Correct: `--prompt "#new-feature"`
- Incorrect: `--prompt "new-feature"`
- Incorrect: `--prompt "# new-feature"`

### Claude asks for information already in claude.md

**Solution**: The template might not have clear enough connection to base config. You can:
- Reference specific sections: "Follow the testing strategy in claude.md"
- Be explicit: "Use the coding standards we defined"

### Tests aren't reaching 100% coverage

**Solution**:
```bash
# Check what's missing
poetry run pytest --cov=ecowitt2mqtt --cov-report=term-missing tests/

# Ask Claude specifically
claude-code --prompt "#test" "Add tests to cover lines 45-52 in config.py"
```

## Tips for Success

### 1. Be Specific in Prompts
- ❌ "Fix the bug"
- ✅ "Fix MQTT connection bug when broker is unreachable"

### 2. Provide Context
- ❌ "Add tests"
- ✅ "Add tests for the battery management module covering all three strategies"

### 3. Reference Evidence
- ❌ "Something is broken"
- ✅ "Here's the error log showing the issue: [paste log]"

### 4. Iterate on Requirements
- Use the templates as conversation starters
- Refine requirements as you discuss with Claude
- Don't rush through the template - thorough requirements = better code

### 5. Trust but Verify
- Claude will aim for 100% coverage
- Always run tests yourself to verify
- Review generated code even if it looks good
- Use the PR review template on your own code

## Getting Help

If you're unsure which template to use:

**Ask Claude directly**:
```bash
claude-code "I need to [describe task]. Which template should I use and how?"
```

Claude will:
- Recommend the appropriate template(s)
- Explain how to use them
- Guide you through the process

## Next Steps

1. **Copy these files to your project root**
2. **Try a simple task first**: 
   ```bash
   claude-code --prompt "#test" "Write a test for a simple function"
   ```
3. **Gradually use more complex workflows**
4. **Customize templates as you learn what works**

## Summary

- **`claude.md`** = Always active, project foundation
- **`#templates`** = Invoked when needed for specific tasks
- **Combine templates** for comprehensive workflows
- **Fill templates thoroughly** for best results
- **Self-review** before submitting PRs
- **Iterate and refine** your process

Happy coding! 🚀
