# assess-agentic

A forward-deployed consulting engagement skill for agentic workflow readiness assessment. Delivered as a Claude Code and Codex skill that guides a Senior Strategy Consultant or client DRI (directly responsible individual) through a structured discovery process — from initial intake through deep Socratic interview to a client-ready project charter.

## Status

**v1.1 — Formalized** (Phase 0–5 hard gates + unified B2B messaging)

Both assessment types — Company and Project — follow the same five-phase structure with consistent formal messaging. The skill enforces sequential phase gates; no phase is skipped regardless of assessment type.

## What it does

```
Phase 0: Environment Verification
Phase 1: Client Intake
Phase 2: Market Research
Phase 2b: Pre-Interview Context
Phase 2c: Domain & Competitive Research
Phase 3: Deep-Interview (Socratic)
Phase 4: Interview Completion
Phase 5: DOCX Export & Client Delivery
```

The same five-phase structure applies to both assessment types — Company and Project. The only differences are the research scope (industry vs. domain) and the deep-interview scope argument passed to the runtime.

| Phase | Description |
|-------|-------------|
| **Phase 0** | Verifies OMC or OMX, deep-interview skill, and pandoc — hard fail if any are missing |
| **Intake** | Collects company name, URL, industry, and assessment type (company portfolio scan or project charter) |
| **Pre-Interview Pulse** | Captures desired outcome, success metrics, and top 2–3 pain points before the Socratic loop |
| **Research** | Fetches and summarizes the company website; detects industry and competitive context |
| **Domain & Competitive Research** | Identifies latest solutions for the target domain, competitive landscape, and peer case studies |
| **Deep-Interview** | Hands off to the active runtime's `deep-interview` skill with consultant persona framing, business-outcome discipline, and closed-loop architecture probes |
| **Report** | Outputs structured markdown + JSON readiness report (company portfolio scan or project charter format) |
| **DOCX Export** | Generates a client-ready Word document (requires pandoc) |
| **Stop after delivery** | Presents the artifacts and exits — no automatic handoff to planning or execution |

## Supported hosts

This skill runs on either Claude Code or Codex CLI:

| Host | Runtime | Install |
|------|---------|---------|
| **Claude Code** | OMC (oh-my-claudecode) | `curl -fsSL https://raw.githubusercontent.com/yeachan-heo/oh-my-claudecode/main/install.sh | bash && omc setup` |
| **Codex CLI** | OMX (oh-my-codex) | `npm install -g @openai/codex`, then see OMX install docs |

The skill detects the active host at runtime, installs into the matching skills directory if needed, and runs the pre-flight check for the detected runtime layer.

## Consulting discipline

This skill operates as a **Senior Strategy Consultant at AgentDash Consulting**.

| Principle | Application |
|-----------|--------------|
| **IT problems are never the goal** | "Clean up SharePoint" or "build a RAG chatbot" are symptoms — always drill to the blocked business outcome |
| **Business-outcome first** | Every system/integration question is reframed through: "what decision does this enable, and what does wrong output cost?" |
| **Hybridize first** | Buy commodity primitives; build only the differentiator |
| **Ship working software, not slides** | The pilot deliverable is an agent in production with board controls wired in — not a strategy deck |

## Prerequisites

| Dependency | Required | Notes |
|------------|----------|-------|
| **Claude Code** or **Codex CLI** | Yes | One of the two hosts required |
| **OMC (Claude Code)** or **OMX (Codex)** | Yes | Hard requirement — skill will not run without the matching runtime layer |
| **pandoc** | Yes | Required for DOCX export: `brew install pandoc` |

> **First-run install time:** Initial setup of OMC or OMX plus the deep-interview skill can take 5–10 minutes depending on network speed. Subsequent runs are near-instant. Plan accordingly before starting a client session.

## Quick start

### 1. Install the skill (one-time)

```bash
# Clone into the Claude Code skills directory — callable from anywhere after this:
git clone https://github.com/thetangstr/agentdash-assess-skill.git ~/.claude/skills/assess-agentic
```

### 2. Install the runtime layer

Install from the GitHub repos — the README on each page has the full install instructions:

- **OMC (Claude Code):** https://github.com/yeachan-heo/oh-my-claudecode
- **OMX (Codex):** https://github.com/Yeachan-Heo/oh-my-codex

### 3. Run the assessment

```bash
claude    # or: omx --madmax --high

# Then tell Claude Code or Codex:
/assess-agentic
```

Or use the prompt directly:

```
Install https://github.com/thetangstr/agentdash-assess-skill and use it to help me create an assessment for my project (or company).
```

## Output artifacts

After a complete assessment, three files are written to the active runtime's specs directory:

| File | Description |
|------|-------------|
| `assess-{slug}.md` | Canonical markdown report — company portfolio scan or project charter |
| `assess-{slug}.json` | Structured JSON with dimensions breakdown and crystallized spec |
| `assess-{slug}.docx` | Client-ready Word document generated by pandoc |

### Report formats

**Company-level assessment** — full portfolio scan:
- Executive Summary, AI Maturity Assessment, Layer Infraction Exposure
- AI-Native Operating Diagnosis, Revenue & Cost Opportunities
- Priority Matrix, Implementation Roadmap, Investment & ROI, Risk Factors

**Project-level assessment** — project charter:
- Project Brief, Go/No-Go Decision, Business Case
- Agent Architecture (Tier, org chart, buy/build per layer)
- Closed-Loop Architecture, Success Metrics table, Team/RACI
- Build List, Research Checklist, Phased Rollout (Week 1–6)
- Token-Maxing Budget, Risk Register, Pre-Kickoff Blockers

Both formats end with an **agent-readable tail block** — a machine-extractable TODO list (`- [ ] **TODO Week 1, Day 1:**`) ready to paste into ChatGPT, Codex, or Claude Code.

## Project structure

```
assess-agentic/
├── SKILL.md                  # Main skill definition — intake, research, deep-interview seeding, report generation
├── knowledge.md              # Consultant frameworks — persona, interrogation ladder, dimensions, tier classification
├── strategy.md               # Report templates — company/project formats, go/no-go criteria, DOCX config
├── CONSULTANT_GUIDE.md       # Quick-start guide for consultants running engagements
├── clients/
│   ├── README.md             # Engagement lifecycle, tracking schema, privacy guide
│   ├── .engagements/         # Active engagement records (gitignored)
│   └── examples/            # Anonymized sample outputs for sales demos
├── Dockerfile                # Fresh-machine testing image
└── README.md                # This file
```

## Testing

### Fresh-machine test (no setup)

```bash
# Create a new macOS user account:
sudo sysadminctl -addUser testuser -password testpass

# Log in as testuser, install Claude Code or Codex CLI:
# https://docs.anthropic.com/en/docs/claude-code/getting-started
# https://docs.codex.ai

claude        # or: omx --madmax --high
/assess-agentic

# Skill self-installs from GitHub; pre-flight detects the active host
# and guides matching runtime layer install if missing
```

### Docker test

```bash
git clone https://github.com/thetangstr/agentdash-assess-skill.git /tmp/agentdash-assess-skill
docker build -t assess-skill-test /tmp/agentdash-assess-skill
docker run -it assess-skill-test

# Inside container — start the host you want to test:
claude        # or: omx --madmax --high
/assess-agentic

# Skill self-installs, pre-flight detects the missing runtime layer, guides install
```

## Local development

```bash
# Clone the skill locally:
git clone https://github.com/thetangstr/agentdash-assess-skill.git ~/skills/assess-agentic

# After editing, push to GitHub — the skill self-installs from github.com/thetangstr/agentdash-assess-skill
cd ~/skills/assess-agentic && git push

# Restart the host to pick up changes
```

## Open issues

Current open issues are tracked at: [github.com/thetangstr/agentdash-assess-skill/issues](https://github.com/thetangstr/agentdash-assess-skill/issues)

Remaining work is scoped to human-consultant-only workflows (engagement tracking, proposal generation, multi-session support).

## License

Proprietary — AgentDash Consulting. All client engagement data is confidential.
