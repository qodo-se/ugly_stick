# Ugly Stick Agent

A multi-agent system that intentionally introduces realistic code quality issues, bugs, security vulnerabilities, and performance problems into source files to create engaging code review scenarios.

This is not meant to be used for creating production code; only use for training and testing purposes.

## WARNING: DO NOT USE ON PRODUCTION CODE

This tool intentionally introduces bugs, security vulnerabilities, and quality issues. Always:
- Work on a separate git branch
- Commit clean code before running
- Have backups of important work

## How It Works

Ugly Stick uses 7 specialized agents coordinated by an orchestrator:

| Agent | Role | Issues |
|-------|------|--------|
| **Scout** | Explores the codebase, identifies 5-8 target files | - |
| **Security Saboteur** | Hardcoded secrets, SQL injection, XSS, etc. | 1-2 |
| **Performance Degrader** | Inefficient loops, memory leaks, N+1 queries | 1-2 |
| **Logic Corruptor** | Off-by-one errors, race conditions, wrong operators | 1-2 |
| **Privacy Violator** | PII in logs, sensitive data exposure | 1-2 |
| **Refactoring Breaker** | Renames/changes that break dependent files | 1-2 |
| **Reporter** | Generates a detailed analysis report | - |

**Flow:** Scout analyzes the codebase, then all 5 issue agents run **in parallel**, then the Reporter generates `ugly_stick_report.md`.

## Setup

### Option A: Project-level command (per-repo)

Copy the `.claude/commands/` directory into any project where you want the command available:

```bash
cp -r .claude/commands/ /path/to/your/project/.claude/commands/
```

### Option B: Global command (available everywhere)

Copy the command to your user-level Claude Code commands directory:

```bash
mkdir -p ~/.claude/commands
cp .claude/commands/ugly-stick.md ~/.claude/commands/
```

## Usage

In Claude Code, run:

```
/ugly-stick /path/to/target/codebase
```

Or just `/ugly-stick` and the orchestrator will ask you which directory to target.

## Output

- Modified source files with 7-10 introduced issues
- `ugly_stick_report.md` in the target directory with full documentation of all issues, detection strategies, and remediation steps

## Architecture

```
ugly-stick (slash command)
  |
  +-- Orchestrator (coordinates all phases)
       |
       +-- Phase 1: Scout Agent (codebase analysis)
       |
       +-- Phase 2: 5 Issue Agents (run in parallel)
       |     +-- Security Saboteur
       |     +-- Performance Degrader
       |     +-- Logic Corruptor
       |     +-- Privacy Violator
       |     +-- Refactoring Breaker
       |
       +-- Phase 3: Reporter Agent (generates report)
```

Each agent runs as a separate Claude Code subagent via the Task tool, allowing parallel execution and isolated context.

## File Structure

```
.claude/commands/ugly-stick.md   # Self-contained slash command (orchestrator + all agent prompts)
agents/                          # Individual agent prompts (reference/development copies)
  scout.md
  security_saboteur.md
  performance_degrader.md
  logic_corruptor.md
  privacy_violator.md
  refactoring_breaker.md
  reporter.md
```
