# Ugly Stick Agent 🪄💥

An intelligent agent that intentionally introduces realistic code quality issues, bugs, security vulnerabilities, and performance problems into source files to create engaging code review scenarios.

## Overview

The Ugly Stick Agent transforms clean code into problematic code containing realistic issues that developers commonly encounter. It's perfect for:

- **Training developers** on code review skills
- **Testing static analysis tools** and their detection capabilities  
- **Creating realistic scenarios** for code review workshops
- **Validating CI/CD pipelines** and quality gates
- **Educational purposes** in software engineering courses

## Features

- 🎯 **Intelligent Issue Introduction**: Adds realistic bugs, security flaws, and performance issues
- 🌍 **Multi-Language Support**: Automatically detects and handles various programming languages
- 🎲 **Mixed Difficulty Levels**: Combines obvious issues with subtle bugs for comprehensive training
- 🔒 **Safe Operation**: Always works on a separate Git branch, preserving original code
- 📊 **Detailed Reporting**: Generates comprehensive reports with issue locations and solutions
- 🎛️ **Configurable**: Control issue density, types, and difficulty levels
- 🧠 **Smart File Selection**: Focuses on meaningful source files, skips tests and configs

## Quick Start

### Basic Usage

```bash
# Run on current project with default settings
qodo ugly_stick

# Specify a different project directory
qodo ugly_stick --project_path=/path/to/your/project

# Create a custom branch name
qodo ugly_stick --branch_name=code-review-training-session-1

# Preview changes without modifying files
qodo ugly_stick --dry_run=true
```

### Advanced Configuration

```bash
# High density of issues, focus on security and performance
qodo ugly_stick --issue_density=high --issue_types='["security", "performance"]'

# Only subtle issues for experienced reviewers
qodo ugly_stick --difficulty_mix=subtle --max_files=5

# Quick training scenario with obvious issues
qodo ugly_stick --difficulty_mix=easy --issue_density=low --max_files=3
```

## Configuration Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `project_path` | string | current dir | Path to the project directory |
| `branch_name` | string | auto-generated | Name for the Git branch |
| `issue_density` | string | `medium` | Density of issues (low, medium, high) |
| `issue_types` | array | all types | Specific issue types to focus on |
| `difficulty_mix` | string | `mixed` | Difficulty level (easy, subtle, mixed) |
| `max_files` | integer | `10` | Maximum number of files to modify |
| `preserve_tests` | boolean | `true` | Skip test files and testing directories |
| `create_report` | boolean | `true` | Generate detailed report |
| `dry_run` | boolean | `false` | Preview changes without modifying files |

## Types of Issues Introduced

### 🔐 Security Vulnerabilities
- **SQL Injection**: Removes parameterized queries, introduces string concatenation
- **XSS Vulnerabilities**: Removes input sanitization and output encoding
- **Hardcoded Secrets**: Adds API keys, passwords, and tokens directly in code
- **Path Traversal**: Introduces unsafe file path handling
- **Weak Authentication**: Removes or weakens security checks
- **Insecure Deserialization**: Uses unsafe deserialization methods

**Example Transformation:**
```python
# Before (secure)
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))

# After (vulnerable to SQL injection)
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")
```

### ⚡ Performance Issues
- **Inefficient Loops**: Introduces nested loops and unnecessary iterations
- **Memory Leaks**: Creates unclosed resources and circular references
- **Blocking Operations**: Adds synchronous calls on main threads
- **N+1 Query Problems**: Introduces database query inefficiencies
- **String Concatenation**: Replaces efficient methods with slow alternatives

**Example Transformation:**
```python
# Before (efficient)
result = "".join(items)

# After (inefficient)
result = ""
for item in items:
    result = result + item  # Inefficient string concatenation
```

### 🐛 Logic Bugs
- **Off-by-One Errors**: Modifies loop bounds and array indices
- **Null Pointer Exceptions**: Removes null checks and safety guards
- **Race Conditions**: Introduces timing-dependent bugs
- **Incorrect Conditionals**: Modifies comparison operators and logic
- **Wrong Assignments**: Swaps variables and introduces typos

**Example Transformation:**
```python
# Before (correct)
for i in range(len(items)):
    process(items[i])

# After (off-by-one error)
for i in range(len(items) + 1):  # Will cause IndexError
    process(items[i])
```

### 📝 Code Quality Issues
- **Duplicate Code**: Copies code blocks instead of using functions
- **Long Methods**: Combines multiple functions into overly long ones
- **Poor Naming**: Changes descriptive names to generic ones
- **Magic Numbers**: Replaces constants with hardcoded values
- **Dead Code**: Adds unreachable code blocks

**Example Transformation:**
```python
# Before (clean)
MAX_RETRY_ATTEMPTS = 3
def retry_operation():
    for attempt in range(MAX_RETRY_ATTEMPTS):
        # retry logic

# After (poor quality)
def retry_operation():
    for attempt in range(3):  # Magic number
        # retry logic
    # Duplicate retry logic added elsewhere
```

### 🎨 Style Violations
- **Inconsistent Formatting**: Mixes indentation styles and spacing
- **Missing Documentation**: Removes docstrings and comments
- **Complex Expressions**: Creates overly complicated one-liners
- **Poor Organization**: Moves code to inappropriate locations
- **Naming Inconsistencies**: Mixes camelCase, snake_case, etc.

## Usage Examples

### Training Scenario Creation

```bash
# Create a beginner-friendly code review scenario
qodo ugly_stick \
  --project_path=./training-project \
  --branch_name=beginner-code-review \
  --difficulty_mix=easy \
  --issue_density=medium \
  --max_files=5

# Advanced training with subtle security issues
qodo ugly_stick \
  --branch_name=security-review-advanced \
  --issue_types='["security", "bugs"]' \
  --difficulty_mix=subtle \
  --issue_density=high
```

### Static Analysis Tool Testing

```bash
# Test your linting tools and security scanners
qodo ugly_stick \
  --branch_name=static-analysis-test \
  --issue_types='["security", "quality", "style"]' \
  --create_report=true

# Check what your tools can detect
git checkout static-analysis-test
npm run lint  # or your preferred analysis tool
```

### Workshop Preparation

```bash
# Prepare multiple scenarios for a workshop
for session in 1 2 3; do
  qodo ugly_stick \
    --branch_name=workshop-session-$session \
    --difficulty_mix=mixed \
    --max_files=3 \
    --issue_density=medium
done
```

## Generated Output

### Git Branch Structure
```
main (original clean code)
├── ugly-stick-review-20241201-143022 (generated branch)
│   ├── Modified files with introduced issues
│   ├── ugly-stick-report.md (detailed report)
│   └── ugly-stick-solutions.md (answer key)
```

### Report Contents
The agent generates comprehensive reports including:

- **Executive Summary**: Overview of issues introduced
- **File-by-File Breakdown**: Detailed list of changes per file
- **Issue Categories**: Count and examples by type
- **Difficulty Analysis**: Distribution of easy vs subtle issues
- **Solution Key**: Explanations for code reviewers
- **Learning Objectives**: What each issue teaches

### Example Report Structure
```markdown
# Ugly Stick Report - 2024-12-01 14:30:22

## Summary
- Files Modified: 7
- Total Issues Introduced: 23
- Languages Detected: Python, JavaScript
- Branch: ugly-stick-review-20241201-143022

## Issues by Category
- Security: 6 issues
- Performance: 5 issues  
- Logic Bugs: 4 issues
- Code Quality: 5 issues
- Style: 3 issues

## Detailed Issues
### backend/api/auth.py:42
- **Type**: Security (Critical)
- **Issue**: SQL Injection vulnerability
- **Description**: Removed parameterized query, using string formatting
- **Difficulty**: Easy to spot
- **Learning**: Always use parameterized queries

### frontend/utils/validation.js:15
- **Type**: Performance (Medium)
- **Issue**: Inefficient loop structure
- **Description**: Added nested loop where single loop would suffice
- **Difficulty**: Subtle
- **Learning**: Algorithm complexity analysis
```

## Integration Examples

### GitHub Actions Workflow
```yaml
name: Code Review Training Setup
on:
  workflow_dispatch:
    inputs:
      difficulty:
        description: 'Difficulty level'
        required: true
        default: 'mixed'
        type: choice
        options:
        - easy
        - mixed
        - subtle

jobs:
  create-review-scenario:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Qodo CLI
        run: npm install -g @qodo/gen
      
      - name: Create Code Review Scenario
        run: |
          qodo ugly_stick \
            --branch_name=code-review-${{ github.run_number }} \
            --difficulty_mix=${{ github.event.inputs.difficulty }} \
            --create_report=true
      
      - name: Push Review Branch
        run: |
          git push origin code-review-${{ github.run_number }}
      
      - name: Create Pull Request
        run: |
          gh pr create \
            --title "Code Review Training Session #${{ github.run_number }}" \
            --body "Find and identify the issues introduced by the ugly-stick agent!" \
            --head code-review-${{ github.run_number }}
```

### Pre-commit Hook for Training
```bash
#!/bin/bash
# .git/hooks/pre-commit-training

# Create training scenario before important commits
if [[ $1 == "--training" ]]; then
  echo "Creating code review training scenario..."
  qodo ugly_stick \
    --branch_name=training-$(date +%Y%m%d-%H%M%S) \
    --difficulty_mix=mixed \
    --max_files=5
  echo "Training branch created! Review the issues before proceeding."
fi
```

## Best Practices

### For Training Sessions
1. **Start Simple**: Begin with easy-to-spot issues for new developers
2. **Mix Difficulty**: Combine obvious and subtle issues for comprehensive learning
3. **Focus Areas**: Target specific issue types based on team needs
4. **Provide Context**: Use the generated reports to guide discussions
5. **Iterate**: Create multiple scenarios with different focus areas

### For Tool Testing
1. **Comprehensive Coverage**: Use all issue types to test tool capabilities
2. **Baseline Comparison**: Run tools on clean code first, then on ugly-stick output
3. **False Positive Testing**: Verify tools don't flag legitimate code patterns
4. **Performance Testing**: Use high-density settings to test tool performance
5. **Continuous Integration**: Integrate into CI/CD to validate quality gates

### For Educational Use
1. **Progressive Difficulty**: Start with style issues, progress to security vulnerabilities
2. **Real-world Relevance**: Focus on issues commonly found in production code
3. **Interactive Learning**: Use reports as discussion starters
4. **Hands-on Practice**: Let students find issues before revealing solutions
5. **Language-specific Focus**: Tailor issue types to the languages being taught

## Safety and Reversibility

### Git Branch Protection
- **Always creates new branch**: Original code remains untouched on main branch
- **Descriptive branch names**: Easy to identify and manage training branches
- **Clean separation**: Training scenarios isolated from production code

### Rollback Procedures
```bash
# Return to original clean code
git checkout main

# Delete training branch if no longer needed
git branch -D ugly-stick-review-20241201-143022

# Keep training branch for future reference
git checkout ugly-stick-review-20241201-143022
git tag training-session-1
git checkout main
```

### File Backup Strategy
```bash
# The agent automatically works on branches, but you can create additional backups
git stash push -m "Pre-ugly-stick backup"
qodo ugly_stick
# If needed: git stash pop
```

## Troubleshooting

### Common Issues

**Agent doesn't find meaningful files:**
- Ensure you're in a project directory with source code
- Check that files aren't excluded by .gitignore patterns
- Verify file extensions are recognized (add custom patterns if needed)

**Git branch creation fails:**
- Ensure you're in a Git repository: `git status`
- Check for uncommitted changes: `git add . && git commit -m "Save work"`
- Verify Git is properly configured: `git config user.name` and `git config user.email`

**Issues aren't realistic enough:**
- Increase issue density: `--issue_density=high`
- Focus on specific types: `--issue_types='["security", "bugs"]'`
- Use subtle difficulty: `--difficulty_mix=subtle`

**Too many/few files modified:**
- Adjust max files: `--max_files=5`
- Check project structure and file selection criteria
- Use dry run to preview: `--dry_run=true`

### Debug Mode
```bash
# Run with detailed output
qodo ugly_stick --dry_run=true --create_report=true

# Check what files would be selected
find . -name "*.py" -o -name "*.js" -o -name "*.java" | grep -v test | head -10
```

### Validation Commands
```bash
# Verify the agent is available
qodo --list-agents | grep ugly_stick

# Test with minimal changes
qodo ugly_stick --max_files=1 --issue_density=low --dry_run=true

# Check Git repository status
git status
git branch -a
```

## Advanced Usage

### Custom Issue Patterns
While the agent automatically detects and applies appropriate transformations, you can influence its behavior:

```bash
# Focus on specific languages
qodo ugly_stick --project_path=./backend  # Python-heavy directory
qodo ugly_stick --project_path=./frontend # JavaScript-heavy directory

# Target specific file patterns
find . -name "*.py" -path "*/api/*" | head -5  # Preview API files
qodo ugly_stick --max_files=5  # Will prioritize substantial files
```

### Batch Training Scenarios
```bash
#!/bin/bash
# create-training-scenarios.sh

scenarios=("beginner" "intermediate" "advanced" "security-focused")
difficulties=("easy" "mixed" "subtle" "mixed")
densities=("low" "medium" "medium" "high")

for i in "${!scenarios[@]}"; do
  scenario="${scenarios[$i]}"
  difficulty="${difficulties[$i]}"
  density="${densities[$i]}"
  
  echo "Creating $scenario scenario..."
  qodo ugly_stick \
    --branch_name="training-$scenario-$(date +%Y%m%d)" \
    --difficulty_mix="$difficulty" \
    --issue_density="$density" \
    --max_files=5
done

echo "All training scenarios created!"
git branch -a | grep training
```

### Integration with Code Review Tools
```bash
# Create scenario and immediately open PR
qodo ugly_stick --branch_name=review-session-1
git push origin review-session-1

# For GitHub
gh pr create --title "Code Review Training" --body "Find the issues!"

# For GitLab
glab mr create --title "Code Review Training" --description "Find the issues!"
```

## Contributing

Found ways to improve the Ugly Stick Agent? We'd love your contributions!

### Adding New Issue Patterns
The agent uses pattern-based transformations. To add new patterns:

1. Identify common real-world issues in your domain
2. Create transformation rules that introduce these issues
3. Test with various codebases to ensure realistic results
4. Document the educational value of each pattern

### Language Support
To extend language support:

1. Add language detection patterns
2. Create language-specific transformation rules
3. Test with real projects in that language
4. Ensure issues are idiomatic to the language

### Reporting Improvements
Help us make better reports:

1. Suggest additional metrics or visualizations
2. Improve the educational content and explanations
3. Add integration examples for new tools
4. Enhance the solution key format

## License

This agent is part of the Qodo Agent Reference Implementations and is licensed under the MIT License.

---

**Remember**: The Ugly Stick Agent is a powerful educational tool. Use it responsibly to improve code review skills and validate development processes. Always work on separate branches and never run it on production code without proper safeguards! 🛡️