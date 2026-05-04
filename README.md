# assess-agentic

Claude Code skill for agentic workflow readiness assessment.

## What it does

1. **Pre-flight check** — verifies OMC (oh-my-claudecode) is installed
2. **Intake** — collects company name, URL, and assessment type
3. **Research** — fetches and summarizes the company website
4. **Deep-interview** — hands off to OMC's `deep-interview` skill for adaptive Socratic requirements gathering
5. **Report** — outputs a structured markdown + JSON readiness report
6. **Handoff** — offers execution options (omc-plan, autopilot, etc.)

## Install

```bash
# Clone the skill
git clone https://github.com/thetangstr/agentdash-assess-skill.git ~/skills/assess-agentic

# Add to your CLAUDE.md
@import ~/skills/assess-agentic/SKILL.md
```

## Test in Docker (fresh environment)

```bash
# Build the test image
docker build -t assess-skill-test github.com/thetangstr/agentdash-assess-skill

# Run the container
docker run -it assess-skill-test

# Inside the container — verify clean state
which omc  # should return empty (OMC not installed)

# Start Claude Code (will prompt for auth on first run)
claude

# In the Claude Code session, import and run the skill:
@import ~/skills/assess-agentic/SKILL.md

# Then trigger:
/assess-agentic

# The pre-flight check should fire, telling you to install OMC
```

## Fresh macOS test (no VM)

```bash
# Create a test directory with no CLAUDE.md
mkdir ~/test-assess && cd ~/test-assess

# Open Claude Code (it will start with no project context)
claude

# Import the skill:
@import ~/skills/assess-agentic/SKILL.md

# Run:
/assess-agentic
```
