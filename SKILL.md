---
name: assess-agentic
description: Assess an organization for agentic workflow readiness — intake, deep-interview, and structured output
argument-hint: [--company|--project] [--url <company_url>]
pipeline: [assess-agentic, deep-interview, omc-plan, autopilot]
next-skill: deep-interview
next-skill-args: --standard
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
- User already has a detailed PRD or spec file — use ralph or autopilot directly
- User wants competitive intelligence or market research — this is readiness assessment, not market analysis
- User wants a quick one-question answer — use a direct answer instead
</Do_Not_Use_When>

<Why_This_Exists>
Most agentic workflow failures don't stem from technology — they stem from unclear requirements: nobody defined what "success" means, nobody named the systems that need to integrate, nobody established error tolerance or escalation paths. This skill applies mathematical clarity gates (via deep-interview) before any execution begins, specifically for the closed-loop agentic workflow domain.
</Why_This_Exists>

<Execution_Policy>
- Collect intake FIRST, then hand off to deep-interview — do not skip intake
- Use the company URL to fetch and summarize the website before seeding deep-interview
- Frame the deep-interview with domain-specific seed context: closed-loop agentic workflow, sustainability, error tolerance
- Use `--standard` deep-interview depth for most assessments
- After deep-interview crystallizes, format the spec as a structured markdown report
</Execution_Policy>

<Steps>

## Phase 0: Pre-flight check

Before starting, verify OMC (oh-my-claudecode) is installed. This skill depends on the `deep-interview` skill which is part of OMC.

1. **Check if OMC is installed**: Try invoking `Skill("omc-reference")` or check if `/oh-my-claudecode` commands work in the current session.
2. **If OMC is not installed**, tell the user:

> "This skill requires OMC (oh-my-claudecode) to be installed first, because it relies on the `deep-interview` skill for adaptive interviewing. To install OMC, run:
>
> ```bash
> curl -fsSL https://raw.githubusercontent.com/oh-my-claude/oh-my-claude/main/install.sh | bash
> ```
>
> After installing OMC, restart your Claude Code session and try again."

3. **If OMC is installed but deep-interview is not available**, the skill will still work — but it will skip the deep-interview handoff and fall back to a manual intake-based assessment. Notify the user: "Deep-interview skill not found — running in limited mode."

Proceed to Phase 1 only after the pre-flight check passes.

## Phase 1: Intake

Collect these fields from the user using `AskUserQuestion`:

1. **Company name** — "What organization are we assessing?"
2. **Company website URL** — "What's their primary website URL?" (used for research fetch)
3. **Assessment type** — `company` (full organization readiness) or `project` (single workflow assessment)
4. **If project**: Project name + one-line description of the workflow

Present them as a concise intake form, not a raw Q&A.

## Phase 2: Research

Fetch the company website at the provided URL:
- Use `WebFetch` or a bash `curl` call to retrieve the HTML
- Strip HTML to plain text (first ~3000 characters is sufficient)
- Detect the primary industry/sector from the content via keyword matching
- Produce a one-paragraph company summary

If the URL is unreachable or empty, note it and proceed with the company name only.

## Phase 3: Seed deep-interview

Invoke the `deep-interview` skill with a domain-specific seed:

```
Skill("deep-interview", "--standard <company_name> agentic workflow readiness assessment")
```

The `initialIdea` passed to deep-interview should be framed as:

> "Assess [company name] for readiness to design and sustain an agentic workflow. [company summary]. The goal is a closed-loop system: autonomous execution, monitoring, feedback, and adaptation. We need to understand the operational context, integration points, error tolerance, success metrics, and organizational capacity to sustain AI agents."

Use `scope: "assess_project"` for project-level assessments, `scope: "cos_onboarding"` for company-level.

## Phase 4: Interview Loop

Monitor the deep-interview rounds. After each round, show the user:
- Current ambiguity score
- Which dimension is being targeted
- The question being asked

Continue until `ambiguity ≤ threshold` (0.2) or the user chooses to exit early.

## Phase 5: Report

When the spec crystallizes, format it as a markdown report saved to `.omc/specs/assess-{slug}.md`:

```markdown
# Agentic Workflow Readiness Assessment: {company_name}

## Metadata
- Assessment Type: {company | project}
- Date: {timestamp}
- Assessor: Claude Code
- URL: {company_url}
- Industry: {detected_industry}
- Final Ambiguity: {score}%
- Status: READY | BELOW_THRESHOLD

## Company Profile
{company_summary_from_research}

## Readiness Verdict
{one-sentence verdict based on final ambiguity and dimension scores}

## Key Findings
### Strengths
- {what the organization has in place}
### Gaps
- {what needs to be addressed before agentic workflow deployment}

## Deep Interview Summary
- Rounds completed: {n}
- Final Ambiguity: {score}%
- Goal Clarity: {score}
- Constraint Clarity: {score}
- Success Criteria: {score}
- Context Clarity: {score} (brownfield only)

## Spec
{crystallized spec content from deep-interview}

## Next Steps
1. Review the spec with the organization stakeholders
2. Identify integration dependencies (systems, APIs, data sources)
3. Design the closed-loop architecture (execution → monitoring → feedback → adaptation)
4. Run a pilot with a narrow scope before full rollout
```

Also save the raw JSON spec:
```json
{
  "company": "{company_name}",
  "url": "{company_url}",
  "type": "{company|project}",
  "industry": "{detected_industry}",
  "summary": "{company_summary}",
  "finalAmbiguity": {score},
  "spec": {raw_crystallized_spec_json},
  "assessedAt": "{ISO_timestamp}"
}
```
to `.omc/specs/assess-{slug}.json`.

## Phase 6: Handoff

After the report is saved, present execution options:

> "Your agentic workflow readiness assessment is complete (ambiguity: {score}%). The crystallized spec is saved at `.omc/specs/assess-{slug}.md`. How would you like to proceed?"

Options:
1. **Plan the agentic workflow** — Invoke `omc-plan --consensus --direct` with the spec as input
2. **Execute a pilot** — Invoke `autopilot` with the spec as context
3. **Refine the assessment** — Continue the deep-interview to reduce ambiguity further
4. **Save and exit** — Keep the spec for later

</Steps>

<Tool_Usage>
- Use `WebFetch` or `Bash` with `curl` for website research
- Use `AskUserQuestion` for intake collection (structured form, not open Q&A)
- Use `Skill()` to invoke deep-interview
- Use `Write` to save the final report and JSON artifact
- Use `Read` to load the crystallized spec for report formatting
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

<Bad>
Skipping research and jumping straight to interview:
```
Starting deep interview for Acme Corp...
```
Why bad: Misses opportunity to inject real company context into the interview framing.
</Bad>

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
</Examples>

<Escalation_And_Stop_Conditions>
- If the user declines to provide a URL, proceed with company name only (skip research phase)
- If deep-interview returns an error, report it and offer to continue with manual context entry
- If the company has no clear industry signal from the website, note "Industry: Unknown" and proceed
- Hard stop at deep-interview round 20 regardless of ambiguity
</Escalation_And_Stop_Conditions>

<Final_Checklist>
- [ ] Intake collected (company, URL, type)
- [ ] Website researched and summary generated
- [ ] Deep-interview seeded with domain-specific framing
- [ ] Interview loop completed or user chose early exit
- [ ] Report saved to `.omc/specs/assess-{slug}.md`
- [ ] JSON artifact saved to `.omc/specs/assess-{slug}.json`
- [ ] Handoff options presented
</Final_Checklist>

Task: {{ARGUMENTS}}
