# assess-agentic

Claude Code skill for agentic workflow readiness assessment — delivered as a forward-deployed consulting engagement.

## What it does

1. **Self-install** — bootstraps itself from GitHub if not yet installed
2. **Pre-flight check** — verifies OMC (oh-my-claudecode) is installed, guides install if missing
3. **Intake** — collects company name, URL, and assessment type (company vs. project)
4. **Pre-Interview Pulse** — collects desired outcome, success metric, and top 2-3 pain points before the Socratic interview
5. **Research** — fetches and summarizes the company website, detects industry
6. **Deep-interview** — hands off to OMC's `deep-interview` skill with consultant persona framing, business-outcome discipline, and closed-loop architecture probes
7. **Report** — outputs a structured markdown + JSON readiness report (company portfolio scan or project charter format)
8. **PDF Export** — generates a client-ready project charter PDF (if pandoc is installed)
9. **Handoff** — offers execution options (omc-plan, autopilot, refine, or exit)

## The consulting discipline

This skill operates as a Senior Strategy Consultant at AgentDash Consulting. Key principles:

- **IT problems are never the goal.** "Clean up SharePoint" or "build a RAG chatbot" are symptoms. The skill always drills to: what business outcome is blocked, what does it cost in dollars or time?
- **Business-outcome first.** Every system/integration question is reframed through: "what decision does this enable, and what does wrong output cost?"
- **Hybridize first.** Buy commodity primitives; build only the differentiator.
- **Ship working software, not slides.** The pilot deliverable is an agent in production with board controls wired in — not a strategy deck.

## Install

```bash
# One-liner — skill self-installs on first run:
claude

# Then just say:
/assess-agentic
```

The skill will detect it's not installed, clone itself from GitHub, update your CLAUDE.md, and prompt you to run again.

## Pre-requisites

- **Claude Code** installed
- **OMC (oh-my-claudecode)** — **required.** The skill will not run without it. Install first:
  ```bash
  curl -fsSL https://raw.githubusercontent.com/oh-my-claude/oh-my-claude/main/install.sh | bash
  ```
  The skill performs a hard pre-flight check — if OMC is missing, it stops and prompts you to install before proceeding.
- **pandoc** (optional) — for PDF export:
  ```bash
  brew install pandoc
  brew install --cask wkhtmltopdf  # for best PDF quality
  ```

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

## Output artifacts

After a complete assessment, three files are saved:

| File | Description |
|------|-------------|
| `.omc/specs/assess-{slug}.md` | Canonical markdown report — company portfolio scan or project charter |
| `.omc/specs/assess-{slug}.json` | Structured JSON with dimensions breakdown and crystallized spec |
| `.omc/specs/assess-{slug}.pdf` | Client-ready PDF project charter (if pandoc installed) |

## Report formats

**Company-level assessment** — full portfolio scan:
- Executive Summary, AI Maturity Assessment, Layer Inflection Exposure
- AI-Native Operating Diagnosis, Revenue & Cost Opportunities
- Priority Matrix, Implementation Roadmap, Investment & ROI, Risk Factors

**Project-level assessment** — project charter:
- Project Brief, Go/No-Go Decision, Business Case
- Agent Architecture (Tier, org chart, buy/build per layer)
- Closed-Loop Architecture, Success Metrics table, Team/RACI
- Build List, Research Checklist, Phased Rollout (Week 1-6)
- Token-Maxing Budget, Risk Register, Pre-Kickoff Blockers

Both end with an **agent-readable tail block** — machine-extractable TODO list (`- [ ] **TODO Week 1, Day 1:**`) ready to paste into ChatGPT, Codex, or Claude Code.

## Dev workflow

```bash
# After editing SKILL.md locally, push to GitHub:
# The skill self-installs from github.com/thetangstr/agentdash-assess-skill
git clone https://github.com/thetangstr/agentdash-assess-skill.git ~/skills/assess-agentic

# Or pull latest from the git repo:
cd ~/skills/assess-agentic && git pull

# Restart Claude Code session to pick up changes
```

## Project structure

| File | Purpose |
|------|---------|
| `SKILL.md` | Main skill definition — intake, research, deep-interview seeding, report generation |
| `knowledge.md` | Consultant frameworks — persona, interrogation ladder, dimensions, tier classification |
| `strategy.md` | Report templates — company/project formats, go/no-go criteria, PDF config |
| `ROADMAP.md` | Ongoing development tracking — backlog, version history |
| `Dockerfile` | Fresh-machine testing image |

## Skill pipeline

```
assess-agentic → deep-interview → omc-plan → autopilot
```

The skill chains into OMC's standard pipeline after the assessment crystallizes. Handoff options at the end:
1. **Plan the agentic workflow** — omc-plan --consensus --direct
2. **Execute a pilot** — autopilot with spec as context
3. **Refine the assessment** — continue deep-interview to reduce ambiguity
4. **Save and exit** — keep the spec for later
