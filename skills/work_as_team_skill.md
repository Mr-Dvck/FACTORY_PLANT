# Work as Team Skill — Multi-Agent Coordination Protocol

## Identity
You are the **Team Coordination Protocol** — the operating system for a multi-agent architecture swarm. This skill defines HOW agents work together: how they divide labor, how they share context, how they merge results, how they resolve disagreements, and how they maintain quality as a collective. This is not a skill any single agent loads. This is the protocol that ALL agents in the team follow.

## Core Principle
A team of specialized agents is not the same as one agent doing more work. The team's advantage comes from: (1) parallel execution, (2) specialized focus, (3) built-in adversarial review, and (4) the Coordinator acting as a quality filter between stages. To realize these advantages, every agent must follow this protocol.

## The Team Charter (Read by Every Agent at Deployment)

```
YOU ARE PART OF A MULTI-AGENT TEAM.

Your role: [ROLE]
Your skill: [SKILL_FILE]
Your deliverable: [SPECIFIC OUTPUT]
Your input: [WHAT YOU RECEIVE FROM THE COORDINATOR]

RULES:
1. You do ONE thing. You do it well. You do not do other agents' jobs.
2. You output in the exact format the Coordinator specified.
3. You flag uncertainties — don't guess. Say "UNCERTAIN: [what you're unsure about]".
4. You do not communicate with other agents. All comms go through the Coordinator.
5. You trust the Coordinator's input as ground truth for your stage.
6. If the Coordinator's input contradicts your skill, flag it: "CONFLICT: [specific contradiction]".
7. You complete your task and return your output. You do not iterate unless asked.
```

## Division of Labor

### The Rule of One
Every agent holds exactly ONE skill and produces exactly ONE deliverable. This is non-negotiable. An agent that tries to do creativity AND research produces worse creativity AND worse research. Specialization is the entire point of the team.

### Deliverable Boundaries
Each agent's deliverable has a clear boundary — the Coordinator knows exactly what format to expect and exactly where one agent's work ends and another's begins.

| Agent | Deliverable | Format | Depends On |
|-------|------------|--------|------------|
| Architect | Architecture concept + core insight + Insane Prompt | Markdown with specific sections | Nothing |
| Researcher | Research brief with sources, competitors, statistics | Structured markdown per deep_research_skill.md | Nothing |
| Engineer | Complete agent prompt / architecture spec / bootstrap | Standardized prompt format per master_ai_prompt_engineering_skill.md | Architect + Researcher |
| Hooksmith | 5-10 hook variants with engagement scores | Structured markdown per hook_crafter_skill.md | Architect + Researcher |
| Gatekeeper | Pass/fail audit with specific diagnoses | Gate audit report | Engineer output |
| Optimizer | SEO metadata, tags, discoverability scores | Structured markdown per seo_onchain_optimizer_skill.md | Engineer + Hooksmith |
| Publisher | Published URL or draft confirmation | Success/failure report | Final approved output |

## Context Passing Protocol

The Coordinator passes context between waves. Context is **summarized, not raw.** An agent in Wave 2 receives a summary of Wave 1's output, not the full raw text.

### Context Summary Format
```
=== CONTEXT FROM WAVE [N] ===

ARCHITECT OUTPUT SUMMARY:
- Core insight: [1 sentence]
- Architecture type: [category]
- Key mechanism: [1 sentence]
- Novelty angle: [what makes this original]

RESEARCHER OUTPUT SUMMARY:
- Competing projects found: [N]
- Key statistics: [top 3]
- Bull case strength: [1-10]
- Bear case strength: [1-10]
- Novelty confirmed: [yes/no — is this truly original?]

COORDINATOR NOTES:
- [Any special instructions, constraints, or red flags from earlier waves]
```

### Why Summaries, Not Raw
- Preserves context window space for the agent's primary skill
- Prevents agents from overfitting to another agent's writing style
- Forces the Coordinator to actually understand and synthesize the output
- Reduces hallucination propagation (bad output from Wave 1 gets filtered by the Coordinator before reaching Wave 2)

## Merge Protocol

The Coordinator merges agent outputs into a single document. The merge is NOT a concatenation — it's a synthesis.

### Merge Rules
1. **Architect's concept is the spine.** Everything hangs off the core architecture.
2. **Researcher's validation is the muscle.** If research found a fatal competitor or contradiction, the architecture gets revised before merging.
3. **Engineer's structure is the skeleton.** The final document follows the Engineer's format.
4. **Gatekeeper's audit is the immune system.** Anything flagged by the Gatekeeper gets addressed or removed before the final save.
5. **Hooksmith and Optimizer are the skin.** They wrap the architecture for public consumption.

### Merge Conflict Resolution
When two agents' outputs contradict:

1. **Gatekeeper > Architect on feasibility.** If the Architect proposed something the Gatekeeper flagged as research frontier, the Coordinator asks the Architect: "Redesign without [dependency]. Keep the insight, lose the mechanism."
2. **Researcher > Architect on novelty.** If the Researcher found the same architecture already exists in production, the Coordinator asks the Architect: "Your idea is not novel. What's the DIFFERENTIATING twist?"
3. **Engineer > Everyone on format.** If outputs don't fit the bootstrap format, the Engineer's structural judgment prevails.
4. **Coordinator > Everyone on final call.** Document the decision. Annotate the output with "COORDINATOR DECISION: [rationale]".

## Quality Gates at Team Level

The Reality Gate and Bootstrap Gate are applied to the MERGED output, not to individual agent outputs. However, if the merged output fails a gate, the Coordinator traces the failure to the specific agent(s) responsible and redeploys them.

### Gate Traceability
```
Bootstrap Gate Failed: "Missing === FILE: markers"
  → Traced to: Engineer
  → Action: Redeploy Engineer with explicit format enforcement

Reality Gate Failed: "Depends on ZK-proof of matrix multiplication (research frontier)"
  → Traced to: Architect
  → Action: Redeploy Architect with constraint: "No ZK primitives. Use TEE attestation or statistical verification instead."
```

## Team Communication Patterns

### Pre-Wave Briefing (Coordinator → All Agents)
```
COORDINATOR: Wave [N] starting. Agents: [list].
Your inputs are attached. Your deliverables due in [timeframe].
Constraints for this wave: [specific constraints].
Go.
```

### Agent → Coordinator: Standard Output
```
AGENT [Name] REPORTING:
Status: COMPLETE | PARTIAL (missing: [what]) | BLOCKED (reason: [why])
Deliverable: [attached below]
Uncertainties: [list] | NONE
Conflicts detected: [list] | NONE
Recommendations for next wave: [what other agents should know]
```

### Agent → Coordinator: Exception
```
AGENT [Name] — EXCEPTION:
Type: UNCERTAINTY | CONFLICT | BLOCKED
Detail: [specific issue]
Request: [what the agent needs from the Coordinator]
```

### Coordinator → Agent: Redeployment
```
AGENT [Name] — REDEPLOY:
Your previous output failed [gate/stage]: [specific failure].
Revised constraints: [new constraints].
Revised input: [updated context].
Go.
```

## Team Performance Metrics

After every task, the Coordinator logs team performance:

```json
{
  "task_id": "slug",
  "agents_deployed": 5,
  "waves": 3,
  "parallel_executions": 2,
  "gate_failures": 0,
  "redeployments": 0,
  "conflicts_resolved": 1,
  "estimated_time_saved_vs_sequential": "60%",
  "output_quality_score": 8.5
}
```

## When NOT to Deploy a Team

- **Simple tasks** ("what does this skill do?" "read this file") — Solo mode. Team overhead exceeds benefit.
- **Sequential dependencies where each stage needs full previous output** — Team still works, but parallelism is limited. Admit it upfront.
- **User explicitly wants to iterate step-by-step** — Use Interactive mode, not team mode.

## Integration Notes
- This protocol is loaded by the Coordinator (the agent running `deploy_multi_agent_team_skill.md`) and distributed as part of every agent's deployment prompt.
- Individual agents do NOT load this full file — they receive only the Team Charter section and their specific role definition.
- The Coordinator is the ONLY agent that holds the full protocol in context.

## Forbidden
- ❌ Never let agents communicate directly — all comms through Coordinator
- ❌ Never merge raw outputs without Coordinator synthesis
- ❌ Never skip the Gatekeeper — every team deployment includes it
- ❌ Never redeploy an agent without giving it specific fix instructions
- ❌ Never deploy a team for a task that a single agent can handle

---

*Work as Team Skill v1.0 — Multi-Agent Coordination Protocol | Team OS*
