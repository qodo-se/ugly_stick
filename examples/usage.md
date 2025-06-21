# Ugly Stick Agent - Usage Examples

This document provides comprehensive examples of how to use the Ugly Stick Agent in various scenarios.

## Basic Usage Examples

### 1. Quick Start - Default Settings
```bash
# Run with all default settings on current project
qodo ugly_stick

# Expected output:
# ✅ Created branch: ugly-stick-review-20241201-143022
# ✅ Modified 7 files with 23 issues
# ✅ Generated report: ugly-stick-report.md
```

### 2. Specify Project Directory
```bash
# Run on a specific project
qodo ugly_stick --project_path=/path/to/my/project

# Run on a subdirectory (e.g., just the backend)
qodo ugly_stick --project_path=./backend
```

### 3. Custom Branch Name
```bash
# Create a descriptive branch name
qodo ugly_stick --branch_name=code-review-training-session-1

# Use timestamp in branch name
qodo ugly_stick --branch_name=security-review-$(date +%Y%m%d)
```

## Training Scenarios

### Beginner Code Review Training
```bash
# Easy-to-spot issues, low density
qodo ugly_stick \
  --branch_name=beginner-training \
  --difficulty_mix=easy \
  --issue_density=low \
  --max_files=3 \
  --issue_types='["style", "quality"]'

# This creates obvious issues like:
# - Inconsistent indentation
# - Poor variable names
# - Missing documentation
# - Simple logic errors
```

### Intermediate Developer Training
```bash
# Mixed difficulty with moderate density
qodo ugly_stick \
  --branch_name=intermediate-training \
  --difficulty_mix=mixed \
  --issue_density=medium \
  --max_files=5 \
  --issue_types='["bugs", "performance", "quality"]'

# This creates issues like:
# - Off-by-one errors
# - Inefficient loops
# - Memory leaks
# - Duplicate code
```

### Advanced Security Review
```bash
# Subtle security issues for experienced developers
qodo ugly_stick \
  --branch_name=security-advanced \
  --difficulty_mix=subtle \
  --issue_density=high \
  --max_files=8 \
  --issue_types='["security", "bugs"]'

# This creates issues like:
# - SQL injection vulnerabilities
# - XSS opportunities
# - Race conditions
# - Insecure deserialization
```

## Specialized Use Cases

### Testing Static Analysis Tools
```bash
# Create comprehensive test case for linting tools
qodo ugly_stick \
  --branch_name=static-analysis-test \
  --issue_types='["security", "performance", "quality", "style"]' \
  --issue_density=high \
  --max_files=10

# Then test your tools:
git checkout static-analysis-test
npm run lint
npm run security-scan
python -m flake8 .
```

### Workshop Preparation
```bash
# Create multiple scenarios for a workshop
for session in 1 2 3; do
  qodo ugly_stick \
    --branch_name=workshop-session-$session \
    --difficulty_mix=mixed \
    --max_files=4 \
    --issue_density=medium
done

# Create answer key branches
for session in 1 2 3; do
  git checkout workshop-session-$session
  # Review and document issues found
  git tag workshop-$session-solutions
done
```

### Language-Specific Training
```bash
# Focus on Python backend issues
qodo ugly_stick \
  --project_path=./backend \
  --branch_name=python-security-review \
  --issue_types='["security", "performance"]' \
  --max_files=6

# Focus on JavaScript frontend issues  
qodo ugly_stick \
  --project_path=./frontend \
  --branch_name=js-quality-review \
  --issue_types='["bugs", "quality", "style"]' \
  --max_files=5
```

## Preview and Testing

### Dry Run - Preview Changes
```bash
# See what would be changed without modifying files
qodo ugly_stick \
  --dry_run=true \
  --max_files=5 \
  --create_report=true

# This shows:
# - Which files would be selected
# - What types of issues would be introduced
# - Estimated number of changes
```

### Minimal Test Run
```bash
# Small test with just one file
qodo ugly_stick \
  --max_files=1 \
  --issue_density=low \
  --branch_name=test-run

# Review the changes
git checkout test-run
git diff main
```

## Integration Examples

### GitHub Actions Workflow
```yaml
# .github/workflows/code-review-training.yml
name: Create Code Review Training
on:
  workflow_dispatch:
    inputs:
      session_name:
        description: 'Training session name'
        required: true
        default: 'training-session'
      difficulty:
        description: 'Difficulty level'
        required: true
        default: 'mixed'
        type: choice
        options:
        - easy
        - mixed
        - subtle
      focus_area:
        description: 'Focus area'
        required: true
        default: 'all'
        type: choice
        options:
        - all
        - security
        - performance
        - quality

jobs:
  create-training:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Qodo CLI
        run: npm install -g @qodo/gen
      
      - name: Create Training Scenario
        run: |
          if [ "${{ github.event.inputs.focus_area }}" = "all" ]; then
            qodo ugly_stick \
              --branch_name=${{ github.event.inputs.session_name }}-$(date +%Y%m%d) \
              --difficulty_mix=${{ github.event.inputs.difficulty }}
          else
            qodo ugly_stick \
              --branch_name=${{ github.event.inputs.session_name }}-$(date +%Y%m%d) \
              --difficulty_mix=${{ github.event.inputs.difficulty }} \
              --issue_types='["${{ github.event.inputs.focus_area }}"]'
          fi
      
      - name: Push Training Branch
        run: |
          git push origin ${{ github.event.inputs.session_name }}-$(date +%Y%m%d)
      
      - name: Create Pull Request
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          gh pr create \
            --title "Code Review Training: ${{ github.event.inputs.session_name }}" \
            --body "🎯 **Training Focus**: ${{ github.event.inputs.focus_area }}<br>📊 **Difficulty**: ${{ github.event.inputs.difficulty }}<br><br>Find and identify the issues introduced by the ugly-stick agent!<br><br>📋 **Instructions**:<br>1. Review the code changes<br>2. Identify security, performance, and quality issues<br>3. Comment on problematic lines<br>4. Suggest improvements<br><br>📖 **Report**: Check the ugly-stick-report.md file for the answer key after your review!" \
            --head ${{ github.event.inputs.session_name }}-$(date +%Y%m%d)
```

### Pre-commit Hook for Training
```bash
#!/bin/bash
# .git/hooks/pre-commit-training

if [[ "$1" == "--create-training" ]]; then
  echo "🧙‍♂️ Creating code review training scenario..."
  
  # Create training branch
  qodo ugly_stick \
    --branch_name=training-$(date +%Y%m%d-%H%M%S) \
    --difficulty_mix=mixed \
    --max_files=5 \
    --create_report=true
  
  echo "✅ Training scenario created!"
  echo "📋 Review the issues in the new branch before proceeding."
  echo "🔍 Check ugly-stick-report.md for the answer key."
  
  # Show created branch
  git branch | grep training | tail -1
  exit 0
fi
```

### Makefile Integration
```makefile
# Makefile
.PHONY: training-easy training-medium training-hard training-security

training-easy:
	@echo "Creating easy training scenario..."
	qodo ugly_stick \
		--branch_name=training-easy-$(shell date +%Y%m%d) \
		--difficulty_mix=easy \
		--issue_density=low \
		--max_files=3

training-medium:
	@echo "Creating medium training scenario..."
	qodo ugly_stick \
		--branch_name=training-medium-$(shell date +%Y%m%d) \
		--difficulty_mix=mixed \
		--issue_density=medium \
		--max_files=5

training-hard:
	@echo "Creating hard training scenario..."
	qodo ugly_stick \
		--branch_name=training-hard-$(shell date +%Y%m%d) \
		--difficulty_mix=subtle \
		--issue_density=high \
		--max_files=8

training-security:
	@echo "Creating security-focused training scenario..."
	qodo ugly_stick \
		--branch_name=security-training-$(shell date +%Y%m%d) \
		--issue_types='["security", "bugs"]' \
		--difficulty_mix=mixed \
		--max_files=6

training-cleanup:
	@echo "Cleaning up training branches..."
	git branch | grep training | xargs -r git branch -D
```

## Real-World Scenarios

### Code Review Workshop
```bash
# Day 1: Introduction to code review
qodo ugly_stick \
  --branch_name=workshop-day1-intro \
  --difficulty_mix=easy \
  --issue_types='["style", "quality"]' \
  --max_files=3

# Day 2: Security focus
qodo ugly_stick \
  --branch_name=workshop-day2-security \
  --difficulty_mix=mixed \
  --issue_types='["security", "bugs"]' \
  --max_files=4

# Day 3: Performance and advanced topics
qodo ugly_stick \
  --branch_name=workshop-day3-performance \
  --difficulty_mix=subtle \
  --issue_types='["performance", "bugs"]' \
  --max_files=5
```

### Team Training Program
```bash
#!/bin/bash
# team-training-setup.sh

# Junior developers
qodo ugly_stick \
  --branch_name=junior-dev-training \
  --difficulty_mix=easy \
  --issue_density=low \
  --max_files=3 \
  --issue_types='["style", "quality"]'

# Mid-level developers  
qodo ugly_stick \
  --branch_name=mid-level-training \
  --difficulty_mix=mixed \
  --issue_density=medium \
  --max_files=5 \
  --issue_types='["bugs", "performance", "quality"]'

# Senior developers
qodo ugly_stick \
  --branch_name=senior-dev-training \
  --difficulty_mix=subtle \
  --issue_density=high \
  --max_files=8 \
  --issue_types='["security", "performance", "bugs"]'

echo "Training scenarios created for all levels!"
```

### Continuous Learning Integration
```bash
# Weekly training scenarios
# Add to crontab: 0 9 * * 1 /path/to/weekly-training.sh

#!/bin/bash
# weekly-training.sh

WEEK=$(date +%Y-W%U)
FOCUS_AREAS=("security" "performance" "quality" "bugs")
FOCUS=${FOCUS_AREAS[$(($(date +%U) % 4))]}

qodo ugly_stick \
  --branch_name=weekly-training-$WEEK \
  --issue_types="[\"$FOCUS\"]" \
  --difficulty_mix=mixed \
  --max_files=4

echo "Weekly training scenario created: Focus on $FOCUS"
echo "Branch: weekly-training-$WEEK"
```

## Output Examples

### Successful Execution
```
🧙‍♂️ Ugly Stick Agent Starting...

🔍 Scanning project directory: /Users/dev/my-project
📁 Found 45 source files across 3 languages
🎯 Selected 7 meaningful files for modification
🌿 Created branch: ugly-stick-review-20241201-143022

🔧 Introducing issues:
  ├── backend/api/auth.py: 4 issues (2 security, 1 performance, 1 bug)
  ├── backend/models/user.py: 3 issues (1 security, 2 quality)
  ├── frontend/utils/validation.js: 5 issues (1 security, 2 bugs, 2 style)
  ├── frontend/components/Login.jsx: 3 issues (1 performance, 2 quality)
  ├── backend/services/email.py: 4 issues (2 security, 1 bug, 1 quality)
  ├── frontend/api/client.js: 2 issues (1 security, 1 performance)
  └── backend/utils/helpers.py: 2 issues (1 bug, 1 style)

📊 Summary:
  ├── Total Issues: 23
  ├── Security: 8 issues
  ├── Performance: 4 issues
  ├── Bugs: 6 issues
  ├── Quality: 4 issues
  └── Style: 1 issue

📋 Generated report: ugly-stick-report.md
🔑 Generated solutions: ugly-stick-solutions.md

✅ Ugly stick operation completed successfully!

🚀 Next steps:
  1. git checkout ugly-stick-review-20241201-143022
  2. Review the code changes
  3. Find and document the issues
  4. Check ugly-stick-report.md for answers
```

### Dry Run Output
```
🧙‍♂️ Ugly Stick Agent - Dry Run Mode

🔍 Scanning project directory: /Users/dev/my-project
📁 Found 45 source files across 3 languages

🎯 Would select these files for modification:
  ├── backend/api/auth.py (234 lines, Python)
  ├── backend/models/user.py (156 lines, Python)  
  ├── frontend/utils/validation.js (89 lines, JavaScript)
  ├── frontend/components/Login.jsx (145 lines, JavaScript)
  └── backend/services/email.py (98 lines, Python)

🔧 Would introduce approximately:
  ├── Security issues: 6-8
  ├── Performance issues: 3-5
  ├── Logic bugs: 4-6
  ├── Quality issues: 3-5
  └── Style violations: 2-3

🌿 Would create branch: ugly-stick-review-20241201-143022

⚠️  DRY RUN - No files were actually modified
```

## Troubleshooting Examples

### No Files Selected
```bash
# If no files are found
qodo ugly_stick --dry_run=true

# Check what files exist
find . -name "*.py" -o -name "*.js" -o -name "*.java" | grep -v test | head -10

# Adjust file selection
qodo ugly_stick --max_files=20  # Increase limit
```

### Git Issues
```bash
# Ensure clean working directory
git status
git add .
git commit -m "Save work before ugly-stick"

# Then run ugly-stick
qodo ugly_stick
```

### Language Detection Issues
```bash
# Check what languages are detected
file backend/*.py frontend/*.js

# Force specific directory
qodo ugly_stick --project_path=./backend  # Python focus
qodo ugly_stick --project_path=./frontend # JavaScript focus
```

These examples should give you a comprehensive understanding of how to use the Ugly Stick Agent in various scenarios. Experiment with different combinations of parameters to create the perfect training scenarios for your needs!