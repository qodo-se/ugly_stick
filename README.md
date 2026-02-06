# Ugly Stick

A multi-agent system that introduces realistic code quality issues, bugs, security vulnerabilities, and performance problems into source files — creating engaging scenarios for code review training and static analysis testing.

Not meant for production code; only use for training and testing purposes.

## WARNING: DO NOT USE ON PRODUCTION CODE

This tool intentionally introduces bugs, security vulnerabilities, and quality issues. Always:

- Work on a separate git branch
- Commit clean code before running
- Have backups of important work

## What It Does

Ugly Stick coordinates 7 specialized agents to introduce 7-10 realistic issues into a target codebase:

| Agent | Role | Issues |
|-------|------|--------|
| **Scout** | Explores the codebase, identifies 5-8 target files | - |
| **Security Saboteur** | Hardcoded secrets, SQL injection, XSS, etc. | 1-2 |
| **Performance Degrader** | Inefficient loops, memory leaks, N+1 queries | 1-2 |
| **Logic Corruptor** | Off-by-one errors, race conditions, wrong operators | 1-2 |
| **Privacy Violator** | PII in logs, sensitive data exposure | 1-2 |
| **Refactoring Breaker** | Renames/changes that break dependent files | 1-2 |
| **Reporter** | Generates a detailed analysis report | - |

**Flow:** Scout analyzes the codebase, then all 5 issue agents run in parallel, then the Reporter generates `ugly_stick_report.md`.

## Output

- Modified source files with 7-10 introduced issues
- `ugly_stick_report.md` in the target directory with full documentation of all issues, detection strategies, and remediation steps

---

## Approach 1: Claude Code (Recommended)

A single slash command that orchestrates all agents automatically — no manual setup required.

### Setup

The command file at `.claude/commands/ugly-stick.md` is fully self-contained. All 7 agent prompts are inlined directly inside it. Just copy this single file.

**Project-level** (available in one repo):

```bash
mkdir -p /path/to/your/project/.claude/commands
cp .claude/commands/ugly-stick.md /path/to/your/project/.claude/commands/
```

**Global** (available in every project):

```bash
mkdir -p ~/.claude/commands
cp .claude/commands/ugly-stick.md ~/.claude/commands/
```

### Usage

```
/ugly-stick /path/to/target/codebase
```

Or just `/ugly-stick` and the orchestrator will ask you which directory to target.

### How It Works

```
/ugly-stick (slash command)
  |
  +-- Orchestrator (coordinates all phases)
       |
       +-- Phase 1: Scout Agent (codebase analysis)
       |
       +-- Phase 2: 5 Issue Agents (run in parallel via Task tool)
       |     +-- Security Saboteur
       |     +-- Performance Degrader
       |     +-- Logic Corruptor
       |     +-- Privacy Violator
       |     +-- Refactoring Breaker
       |
       +-- Phase 3: Reporter Agent (generates report)
```

Each agent runs as a separate Claude Code subagent via the Task tool, allowing parallel execution and isolated context.

---

## Approach 2: Other AI Tools

The `agents/` directory contains each agent prompt as a standalone markdown file. These are tool-agnostic and can be used with any AI coding assistant or multi-agent framework.

### Agent Files

```
agents/
  scout.md                # Run first — identifies target files
  security_saboteur.md    # Hardcoded secrets + security vulnerabilities
  performance_degrader.md # Performance degradation issues
  logic_corruptor.md      # Logic bugs and race conditions
  privacy_violator.md     # Sensitive data in logging
  refactoring_breaker.md  # Breaking changes across dependent files
  reporter.md             # Compiles results into a report
```

### Orchestration Flow

Wire up the agents in your tool of choice following this sequence:

1. **Run Scout** against the target directory. It returns a JSON list of target files with dependency mappings.
2. **Assign files** to each issue agent (avoid giving two agents the same file to prevent edit conflicts).
3. **Run all 5 issue agents**, passing each its assigned files and the Scout's codebase summary. Run them in parallel if your tool supports it.
4. **Run Reporter**, passing it the JSON output from all issue agents. It generates `ugly_stick_report.md`.

Each agent prompt includes its mission, a step-by-step process, insertion guidelines, and a JSON output schema.

---

## File Structure

```
.claude/commands/ugly-stick.md   # Claude Code command (self-contained, all agents inlined)
agents/                          # Standalone agent prompts (tool-agnostic)
  scout.md
  security_saboteur.md
  performance_degrader.md
  logic_corruptor.md
  privacy_violator.md
  refactoring_breaker.md
  reporter.md
```

The command file and the agent files contain the same prompts. If you modify one, update the other to keep them in sync.
