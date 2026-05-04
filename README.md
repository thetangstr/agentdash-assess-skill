# assess-agentic

Claude Code skill for agentic workflow readiness assessment.

## What it does

1. **Self-install** — bootstraps itself from GitHub if not yet installed
2. **Pre-flight check** — verifies OMC (oh-my-claudecode) is installed
3. **Intake** — collects company name, URL, and assessment type
4. **Research** — fetches and summarizes the company website
5. **Deep-interview** — hands off to OMC's `deep-interview` skill for adaptive Socratic requirements gathering
6. **Report** — outputs a structured markdown + JSON readiness report
7. **Handoff** — offers execution options (omc-plan, autopilot, etc.)

## Install

```bash
# One-liner — skill self-installs on first run:
claude

# Then just say:
/assess-agentic
```

The skill will detect it's not installed, clone itself from GitHub, update your CLAUDE.md, and prompt you to run again.

## Test on a fresh machine (no setup)

```bash
# Create a new macOS user account:
sudo sysadminctl -addUser testuser -password testpass

# Log in as testuser, open Terminal, install Claude Code:
# https://docs.anthropic.com/en/docs/claude-code/getting-started

# Start Claude Code:
claude

# Just say this:
/assess-agentic

# The skill will self-install from GitHub, then the pre-flight check
# will detect OMC is missing and guide the OMC install
```

## Test with Docker

```bash
git clone https://github.com/thetangstr/agentdash-assess-skill.git /tmp/agentdash-assess-skill
docker build -t assess-skill-test /tmp/agentdash-assess-skill
docker run -it assess-skill-test

# Inside container — Claude Code is installed, OMC is NOT:
claude

# Then just say:
/assess-agentic

# Skill self-installs, pre-flight detects OMC missing, guides install
```

## Dev workflow

```bash
# After editing SKILL.md locally:
cd ~/skills/assess-agentic
git pull  # or push from your working copy

# Restart Claude Code session to pick up changes
```
