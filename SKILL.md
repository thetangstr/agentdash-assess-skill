---
name: assess-agentic
description: Assess an organization for agentic workflow readiness
level: 3
---

<Purpose>
Assess an organization for its readiness to design and sustain an agentic workflow with closed-loop support. Collect the minimum necessary intake (company name, URL, assessment type), then hand off to the deep-interview skill for adaptive Socratic requirements gathering — framed specifically for agentic workflow design. The output is a crystallized spec ready for execution.
</Purpose>

<Use_When>
- User wants to assess whether their organization is ready for AI agent workflows
- User says "assess our readiness", "agentic workflow assessment", "evaluate us for AI agents", "can we do agents here"
- User wants a project-level or company-level readiness assessment before committing to building
- User wants a closed-loop agentic system (autonomous execution + monitoring + feedback + adaptation)
</Use_When>

<Do_Not_Use_When>
- User already has a detailed PRD or spec file — use the appropriate planning or execution skill directly instead of this assessment workflow
- User wants competitive intelligence or market research — this is readiness assessment, not market analysis
- User wants a quick one-question answer — use a direct answer instead
</Do_Not_Use_When>

<Why_This_Exists>
Most agentic workflow failures don't stem from technology — they stem from unclear requirements: nobody defined what "success" means, nobody named the systems that need to integrate, nobody established error tolerance or escalation paths. This skill applies mathematical clarity gates (via deep-interview) before any execution begins, specifically for the closed-loop agentic workflow domain.
</Why_This_Exists>

<Execution_Policy>
- Prerequisites (OMC/OMX, deep-interview, pandoc) MUST be installed BEFORE running this skill — do not attempt to install them inside the skill
- If any prerequisite is missing, stop immediately and display the blocking message — do not proceed, do not offer to install
- Collect intake FIRST, then hand off to deep-interview — do not skip intake
- Use the company URL to fetch and summarize the website before seeding deep-interview
- Frame the deep-interview with domain-specific seed context: closed-loop agentic workflow, sustainability, error tolerance
- Use `--standard` deep-interview depth for most assessments
- After deep-interview crystallizes, format the spec as a structured markdown report
- pandoc is required for DOCX export — fail hard if missing before intake
- Use the consultant frameworks (knowledge.md) to inform interview framing
- Use the report templates (strategy.md) to structure the output
</Execution_Policy>

<Steps>

## Phase 0: Environment Verification

**Policy:** Check all prerequisites first. If anything is missing, offer to install it via Bash — the user will interact with the setup wizard if needed. Do not exit silently.

### Step 1: Detect active runtime

Execute in parallel:

```
omc --version 2>/dev/null | head -1
omx --version 2>/dev/null | head -1
```

### Step 2: Verify required executables

Execute in parallel:

```
pandoc --version 2>/dev/null | head -1
```

### Step 3: Present missing prerequisites with install options

After all checks complete, handle each missing item.

**If OMC is not found:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OMC NOT FOUND — Installing OMC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Running the OMC installer. Follow the prompts in the terminal.
This takes a few minutes on first run.
```

Then run via Bash:
```
npm i -g oh-my-claude-sisyphus@latest && omc setup
```
The `omc setup` wizard is interactive — the user will make choices at the terminal.

**If OMX is not found:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OMX NOT FOUND — Installing OMX
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Running the OMX installer. Follow the prompts in the terminal.
```

Then run via Bash:
```
npm install -g @openai/codex oh-my-codex && omx setup
```
The `omx setup` wizard is interactive — the user will make choices at the terminal.

**If pandoc is not found:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PANDOC NOT FOUND
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

pandoc is required to generate the DOCX client deliverable.
```

Options (via AskUserQuestion):
1. **Show install instructions** — Display: `brew install pandoc` (macOS), `sudo apt-get install pandoc` (Linux)
2. **Skip DOCX** — Continue without DOCX export; markdown and JSON will still be produced
3. **Cancel and exit**

**If deep-interview is not found (after OMC/OMX installed):**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
deep-interview NOT FOUND
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The deep-interview skill is not available for the active runtime.
```

Run `omc setup` or `omx setup` again via Bash to refresh skills.

**If all prerequisites are confirmed present:** Proceed to Step 4.

### Step 4: Verify deep-interview is accessible

For OMC: Run `omc skills list 2>/dev/null | grep deep-interview`.

For OMX: Run `omx skills list 2>/dev/null | grep deep-interview`.

If deep-interview still not found after setup, present the manual install instructions and ask to rerun `omc setup` or `omx setup`.

### Step 5: Set runtime context

Record the following variables:

| Variable | OMC Value | OMX Value |
|----------|-----------|-----------|
| `runtime` | `OMC` | `OMX` |
| `runtime_host` | `Claude Code` | `Codex` |
| `runtime_output_dir` | `.omc/specs` | `.omx/specs` |

---

**Phase 0 Complete Gate:** Proceed to Phase 1 only when OMC or OMX is confirmed, deep-interview is confirmed available, and the user has chosen how to handle pandoc.

---

## Phase 1: Client Intake

**Prerequisite:** Phase 0 must be fully complete. Do not begin intake until the environment verification gate passes.

### Phase 1 Header

Present the following header to the user:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 1 OF 5 — CLIENT INTAKE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Collecting assessment parameters. Please provide the following:
```

### Intake Collection

Collect all fields via `AskUserQuestion` using a multi-question form:

1. **Company Name** — Full legal name or commonly-used brand name of the organization being assessed.
2. **Company Website** — Primary website URL (used for market research and industry context).
3. **Assessment Type** — Select one:
   - `Company` — Full organizational readiness assessment (all workflows, all departments)
   - `Project` — Single workflow assessment (one specific process or use case)
4. **If Project selected:** Project Name and a one-sentence description of the target workflow.

### Phase 1 Complete Gate

After all fields are collected, present a confirmation summary:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INTAKE CONFIRMED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Company:        [company_name]
  Website:        [url]
  Assessment:     [Company | Project]
  [Project scope: project_name — description]

Proceeding to research phase.
```

Proceed to Phase 2 only after presenting this confirmation. Do not proceed if any required field is absent or ambiguous.

---

## Phase 2: Market Research

**Prerequisite:** Phase 1 must be fully complete with all intake fields confirmed.

### Phase 2 Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2 OF 5 — MARKET RESEARCH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Researching [company_name] and industry context.
```

### Research Protocol

1. Fetch the company website via `WebFetch` or `curl`.
2. Extract plain text (first 3,000 characters sufficient).
3. Identify primary industry/sector via keyword analysis.
4. Generate a one-paragraph company summary.

### Research Output

After content extraction, present:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
RESEARCH SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Company:     [company_name]
  Industry:    [detected_industry | Industry: Unknown]
  Website:     [url — reachable | unreachable]

  Summary:
  [one-paragraph company description]
```

If the URL is unreachable or returns no content, note it and proceed with the company name only.

### Phase 2 Complete Gate

Proceed to Phase 2b only after the research summary is written and displayed. The Pre-Interview Pulse (Phase 2b) requires this context.

---

## Phase 2b: Pre-Interview Context

**Prerequisite:** Phase 2 must be fully complete with research summary displayed.

### Phase 2b Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2b OF 5 — PRE-INTERVIEW CONTEXT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Before the structured interview, we need to establish
the business context. These questions define success.
```

### Context Questions

Present as a structured form. All eight questions are mandatory. Frame each as shown:

**BUSINESS OUTCOMES**

1. **What business problem are we solving?**
   *(Describe the actual pain — not the IT symptom, but the business impact. Who is affected, and how?)*

2. **What does success look like in business terms?**
   *(Revenue gained, cost reduced, time saved, errors prevented. Not "we want AI agents." What does winning look like?)*

3. **What is the cost of doing nothing?**
   *(Hours lost per week? Revenue leaking? Customers churning? Competitive disadvantage?)*

**STAKEHOLDER CONTEXT**

4. **Who is the decision-maker and DRI?**
   *(Who approves? Who runs it day-to-day? Without named accountable individuals, projects stall.)*

5. **What is the timeline pressure?**
   *(Hard deadline? Board meeting, product launch, or trade show driving this? Or exploratory?)*

6. **What is the budget envelope for the pilot?**
   *(Approximate range — $5K, $50K, $500K? Shapes build vs. buy vs. hybridize recommendation.)*

**OPERATIONAL CONTEXT**

7. **What systems does this workflow touch today?**
   *(Salesforce, SAP, SharePoint, Slack, etc. No wrong answers — name what comes to mind.)*

8. **What is the approximate size of the organization?**
   *(Employee count, revenue range, or industry vertical. Calibrates solution scale.)*

### Phase 2b Complete Gate

After all eight questions are answered, present:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRE-INTERVIEW CONTEXT — CONFIRMED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Business problem:    [answer]
  Success outcome:     [answer]
  Cost of inaction:    [answer]
  Decision maker/DRI:  [answer]
  Timeline:            [answer]
  Budget envelope:     [answer]
  Systems in scope:    [answer]
  Organization size:   [answer]

Proceeding to domain research.
```

Inject all eight answers into the Phase 3 seed prompt. The deep-interview must not re-ask these questions — it builds on this context.

---

## Phase 2c: Domain & Competitive Research

**Prerequisite:** Phase 2b must be fully complete with all context questions answered.

### Phase 2c Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 2c OF 5 — DOMAIN & COMPETITIVE RESEARCH
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Researching market context and comparable solutions.
```

**Policy:** This phase is mandatory for all assessments. Do not skip or abbreviate. The interview questions are sharpened by current market intelligence.

### Research Scope

**For Project-Level Assessments — Domain Technology Research:**

| Project Domain | Research Questions |
|----------------|-------------------|
| Knowledge management | Leading knowledge management platforms? AI features in Notion, Confluence, SharePoint, Guru? Enterprise RAG patterns? |
| Customer support | AI agent solutions for Zendesk, Freshdesk, Intercom? Deflection rates achieved? |
| CRM / Sales | AI agents for Salesforce, HubSpot? AI CRM data entry ROI? |
| Data analysis / BI | Leading AI analytics agents? Microsoft Copilot for Analytics vs. custom? |
| Document processing | Leading document AI platforms? Kira Systems, Leverton, Luminance comparison? |
| Code generation | State of AI coding agents — Codex, Cursor, Copilot Workspace? Productivity data? |
| HR / Recruiting | AI agents for resume screening, scheduling, onboarding? |
| Finance / Accounting | AI agents for AP automation, reconciliation, audit? Moody's, Bloomberg AI? |

**For Company-Level Assessments — Industry & Competitive Research:**

1. **Industry context** — Major trends, key players, competitive landscape
2. **AI adoption benchmarks** — Reported adoption rates, ROI data for this industry
3. **Peer examples** — 2–3 public case studies of AI agent projects
4. **Regulatory environment** — Industry-specific AI regulations or compliance needs
5. **Regional factors** — Multi-region considerations, if applicable

**Search commands:**
```
WebSearch: "{industry|domain} AI agent trends 2026"
WebSearch: "{industry|domain} AI automation case study"
WebSearch: "{industry|domain} AI ROI statistics"
```

### Research Output

After research, write and display:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DOMAIN RESEARCH — [company_name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Domain Brief:
  [2-3 sentence summary of current state-of-the-art]

  Competitive Landscape:
  • [Vendor/Platform]: [key AI features], [pricing], [notable customers]
  • [Vendor/Platform]: ...

  Industry Benchmarks:
  [2-3 specific statistics or findings]

  Comparable Case Studies:
  [1-2 public examples with outcomes, if available]
```

**Time limit:** 10 minutes maximum. Record findings and note gaps for the interview to probe.

### Phase 2c Complete Gate

Proceed to Phase 3 only when the Domain Research output is written and displayed. Inject this output into the Phase 3 seed prompt.

---

## Phase 3: Structured Interview — Deep Interview

**Prerequisite:** Phases 1, 2, 2b, and 2c must ALL be fully complete. Do not invoke deep-interview until all prior phase outputs are confirmed.

### Phase 3 Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 3 OF 5 — STRUCTURED INTERVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Initiating deep-interview for [company_name].
This phase uses a Socratic methodology to clarify
requirements, define success metrics, and classify
agent tier readiness.
```

### Invocation

**For OMC (Claude Code):**
```
Skill("oh-my-claudecode:deep-interview", "--standard <company_name> agentic workflow readiness assessment")
```

**For OMX (Codex):**
```
$deep-interview "--standard <company_name> agentic workflow readiness assessment"
```

**Invalid invocations (will silently fail — do not use):**
- `Skill("deep-interview", ...)` without `oh-my-claudecode:` prefix
- `/deep-interview --standard ...` — user-level chat syntax, not valid in skill body
- Bash execution of `$deep-interview` unless OMX documentation explicitly supports it

Pass the scope argument: `assess_project` for project-level, `cos_onboarding` for company-level.

### Seed Prompt Construction

The arguments passed to deep-interview constitute the initial seed. Include all prior phase outputs:

```
--standard [company_name] agentic workflow readiness assessment

Pre-interview context — do not re-ask; build on this:
- Business problem: [from Phase 2b]
- Success outcome: [from Phase 2b]
- Cost of inaction: [from Phase 2b]
- Decision maker / DRI: [from Phase 2b]
- Timeline: [from Phase 2b]
- Budget envelope: [from Phase 2b]
- Systems in scope: [from Phase 2b]
- Organization size: [from Phase 2b]
- Company research: [from Phase 2]
- Industry context: [from Phase 2c]
- Assessment type: [company | project]

Consultant framing: Forward-deployed strategy consultants. Bias toward hybridize-first. Fixed 4-6 week pilot scope. Working software over slides. Ask about specific systems, specific metrics, specific workflows — not generic intentions.

Five assessment dimensions: specificity (30%), systems (25%), success (20%), risk (15%), fit (10%). Probe closed-loop architecture (outcome signal, judge, memory, update mechanism). Classify target tier (1-5); reject up-tier shortcuts when organizational readiness is low.
```

### Hard Stop Condition

**If the invocation returns an error or deep-interview is unavailable:** Stop immediately. Output:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ASSESSMENT INTERRUPTED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

The deep-interview skill could not be invoked.
Error: [error_message]

Please ensure:
  • OMC/OMX is properly installed
  • deep-interview skill is available
  • Runtime is restarted if recently installed

Restart the host and invoke /assess-agentic to resume.
```

Do not proceed with manual interviewing. Exit the skill.

---

## Phase 4: Interview Completion

**Prerequisite:** Phase 3 must be invoked. This phase is passive — it begins when Phase 3 completes and ends when deep-interview returns.

### Phase 4 Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 4 OF 5 — INTERVIEW IN PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Deep-interview is conducting the Socratic assessment.
This phase runs autonomously until the interview
reaches clarity threshold or round limit.
```

### Behavior During Phase 4

- **Do not answer questions** posed by deep-interview — it handles the interview loop
- **Do not intervene** in the Socratic questioning
- **Wait** for deep-interview to signal completion

### Completion Criteria

Deep-interview ends when:
- Ambiguity score ≤ 0.2
- All five dimensions ≥ 0.7
- Round 20 cap reached
- User requests early exit

### Post-Completion Gate

After deep-interview returns:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INTERVIEW COMPLETE — CRISTALLIZING SPEC
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Crystallized spec saved to:
[runtime_output_dir]/deep-interview-[slug].md

Ambiguity score: [score]
All dimensions ≥ 0.7: [pass | fail]

Proceeding to deliverable generation.
```

Proceed to Phase 5. Do not begin Phase 5 until deep-interview explicitly returns.

---

## Phase 5: Deliverable Generation & Client Delivery

**Prerequisite:** Phase 4 must complete with deep-interview returning a crystallized spec.

### Phase 5 Header

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PHASE 5 OF 5 — DELIVERABLE GENERATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Generating client-ready assessment artifacts.
```

### Step 1 — Read crystallized spec

Read the file at `{runtime_output_dir}/deep-interview-{slug}.md`. Confirm the file exists and contains content. If the file is missing or empty, stop and report:

```
Assessment spec not found. Expected:
[runtime_output_dir]/deep-interview-[slug].md

Please restart the assessment: /assess-agentic
```

### Step 2 — Save markdown artifact

Write the spec content to `{runtime_output_dir}/assess-{slug}.md`.

### Step 3 — Save JSON artifact

Extract and write structured fields to `{runtime_output_dir}/assess-{slug}.json`:
- company, type, dimensions, ambiguity, spec content, timestamp

### Step 4 — Export DOCX

Verify pandoc is available, then export:

```bash
pandoc {runtime_output_dir}/assess-{slug}.md \
  -o {runtime_output_dir}/assess-{slug}.docx \
  --from=markdown \
  --toc \
  --toc-depth=2 \
  --metadata title="Agentic Workflow Readiness Assessment: {company_name}" \
  --metadata author="AgentDash Consulting" \
  --metadata date="{ISO_date}"
```

**If DOCX export fails:** Stop immediately. Report the pandoc error. Do not downgrade to markdown-only delivery.

### Step 5 — Present deliverables

Present the following to the user:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ASSESSMENT DELIVERED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[company_name] Agentic Workflow Readiness Assessment
Assessment Type: [Company | Project]
Date: [ISO_date]

ARTIFACTS:
  • Markdown spec:  [runtime_output_dir]/assess-[slug].md
  • Word document:  [runtime_output_dir]/assess-[slug].docx
  • JSON artifact:  [runtime_output_dir]/assess-[slug].json

NEXT STEP:
Share the .docx with your stakeholders for review.
This document is the project charter for your AI agent pilot.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ASSESSMENT COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Policy:** Stop here. Do not offer planning, execution, or refinement. The DOCX is the deliverable — it is the stakeholder review artifact and the end of this engagement.

</Steps>

<Tool_Usage>
- Use `WebFetch` or `Bash` with `curl` for website research
- Use `AskUserQuestion` for intake collection (structured form, not open Q&A)
- Use `Skill()` to invoke deep-interview only for OMC; use `$deep-interview` as an OMX in-session Codex skill for OMX
- Use `Read` to load the crystallized spec from deep-interview output
- Use `Write` to save markdown and JSON artifacts
- Use `Bash` with required `pandoc` for DOCX export
</Tool_Usage>

<Examples>
<Good>
Intake form presented clearly:
```
Let me assess [Acme Corp] for agentic workflow readiness.

Company: Acme Corp
URL: https://acme.com
Assessment Type: Company
```
Why good: User can see exactly what's being assessed, correct any errors before proceeding.
</Good>

<Good>
Research note before seeding:
```
[Research] Fetched acme.com — detected industry: Financial Services
Summary: Acme Corp provides B2B payment processing for mid-market retailers.
```
Why good: Provides transparency about what Claude learned before asking deep questions.
</Good>

<Good>
Closed-loop probe in interview question:
```
You mentioned the RFP responder agent will write back to Salesforce. How do you measure whether the drafted RFP was actually accepted or rejected by the sales team? What is the outcome signal that closes the loop?
```
Why good: Probes specifically for closed-loop architecture components, not just workflow description.
</Good>

<Good>
Report with clear readiness verdict:
```
## Readiness Verdict
Acme Corp has STRONG readiness for a narrow pilot (customer support deflection)
but WEAK readiness for company-wide deployment (legacy ERP integration gap, no DRI assigned).
Target a pilot scope first.
```
Why good: Actionable, specific, tells the organization what to do next.
</Good>

<Good>
Tier-downgrade rejection in report:
```
Tier 5 recommendation rejected — customer has zero agents in production.
Downgraded to Tier 3 (Workflow Runner) as the wedge that opens the path.
```
Why good: Honest assessment of organizational readiness, not just what the customer wants to hear.
</Good>

<Good>
IT-layer problem translated to business outcome:
```
Client: "We need to organize our SharePoint."
Consultant: "What business outcome is SharePoint disorganization blocking?"
Client: "Our sales team can't find technical specs, so they guess."
Consultant: "What does a wrong guess cost you per deal, and how often does it happen?"
Client: "Probably $50K missed per deal on average, maybe 20% of deals are affected."
Consultant: "That's $X per quarter in at-risk revenue. Let's solve that."
```
Why good: Never accepted the IT problem as the goal. Converted SharePoint chaos into a dollar figure on the second probe. The agentic workflow recommendation will be framed around protecting that revenue, not cleaning up SharePoint.
</Good>
</Examples>

<Escalation_And_Stop_Conditions>
- If the user declines to provide a URL, proceed with company name only (skip research phase)
- If the selected runtime's deep-interview invocation returns an error or deep-interview is unavailable: **hard stop.** Do not proceed with manual interviewing. Report the error and exit.
- If the company has no clear industry signal from the website, note "Industry: Unknown" and proceed
- Hard stop at deep-interview round 20 regardless of ambiguity
- If customer asks for Tier 5 with zero agents in production, explicitly downgrade and explain why
- If the interview captures an IT-layer problem (e.g., "fix SharePoint", "build a RAG chatbot") without a business-outcome translation, do NOT let it pass. Probe until the dollar/time cost is named
</Escalation_And_Stop_Conditions>

<Final_Checklist>
- [ ] **Phase 0 — Prerequisites confirmed present (FAIL HARD if any missing):**
  - [ ] OMC or OMX installed and verified
  - [ ] deep-interview skill confirmed available through selected runtime
  - [ ] pandoc installed and verified
- [ ] Intake collected (company, URL, type)
- [ ] Website researched and summary generated
- [ ] Pre-interview pulse collected (all 8 questions answered)
- [ ] Domain & competitive research performed (Phase 2c — mandatory)
- [ ] Deep-interview seeded with consultant persona + domain-specific framing + closed-loop probes
- [ ] Deep-interview completed (or user exited early)
- [ ] Crystallized spec read from `{runtime_output_dir}/deep-interview-{slug}.md`
- [ ] Markdown saved to `{runtime_output_dir}/assess-{slug}.md`
- [ ] JSON artifact saved to `{runtime_output_dir}/assess-{slug}.json`
- [ ] DOCX exported to `{runtime_output_dir}/assess-{slug}.docx`
- [ ] Deliverable presented to user
</Final_Checklist>

Task: {{ARGUMENTS}}
