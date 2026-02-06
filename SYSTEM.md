# MAMAS: Multi-Adaptive Multi-Agent System

**Mandatory: All tasks MUST be routed before execution.**

---

## Task Routing (Required)

### Level 1: Keyword Matching (10 tokens, 70% hit rate)

| Keywords | Specialist | Tokens |
|----------|-----------|--------|
| chat, communication, customer, dialogue | `.nano/communications` | 150 |
| summary, condense, compress | `.nano/context-synthesizer` | 180 |
| research, investigate, analyze (single topic) | `.nano/research-analyst` | 180 |
| technical solution, architecture, tech selection (clear requirements) | `.nano/technical-architect` | 200 |
| business, product, operations (simple planning) | `.nano/business-strategist` | 190 |

**Match?** → Invoke nano specialist directly (saves ~1,600 tokens)

### Level 2: Complexity Assessment (10 tokens, 20% hit rate)

- Requires 3+ specialists collaborating?
- Cross-domain integration (technical + business + research)?
- Project assessment / comprehensive analysis / integrated solution?
- Creating a new collaboration workflow?

**Match?** → MAMAS mode (Planner → Coordinator)

### Level 3: Index Query (100 tokens, 10% hit rate)

Read `.claude/experts-index.json` to match specialist capabilities.

**No match?** → MAMAS mode

---

## Directory Structure

```
project-root/
├── SYSTEM.md                # System specification
├── specialists/             # Reusable specialist agents
│   ├── planner.md          # Decision endpoint — playbook matching & creation
│   ├── coordinator.md      # Execution supervisor — dispatches & orchestrates
│   ├── talent-architect.md # Specialist factory — creates & upgrades agents
│   ├── .nano/              # Lightweight variants (< 250 tokens)
│   └── {specialist}.md
│
├── patterns/                # Behavior constraint library (NEW)
│   ├── README.md           # Pattern system documentation
│   └── {pattern}.md        # Reusable execution patterns
│
├── source/                  # Raw materials (user-managed input)
│   ├── .meta/              # Auto-maintained digests
│   │   ├── index.json
│   │   └── summaries/      # Avoids redundant analysis
│   └── {resource}.md
│
├── output/                  # Deliverables (system-generated output)
│   └── .meta/              # Auto-maintained deliverable digests
│       └── summaries/
│
└── notes/                   # Process workspaces (process-isolated)
    └── {task-name}/
        ├── playbook.md     # Planner-generated execution plan (required)
        └── cache/          # Intermediate artifacts (this task only)
            └── .context/   # Inter-specialist context summaries
```

---

## Access Control

**Globally Accessible:**
- `source/`, `source/.meta/`, `output/`, `output/.meta/`, `specialists/`, `patterns/`

**Process-Isolated:**
- `notes/{current-task}/` — current task only
- `notes/{other-task}/cache/` — access prohibited

---

## Auto-Maintenance

**On source read:**
- Check `source/.meta/summaries/{file}.md` → if missing, generate digest first

**On output write:**
- Generate digest → write to `output/.meta/summaries/{file}.md`

---

## Token Optimization

### 1. Caching

- `source/.meta/summaries/{file}.md` — avoids redundant source analysis
- `output/.meta/summaries/{file}.md` — avoids redundant deliverable re-reading
- `notes/{task}/cache/.context/` — inter-specialist context summaries

### 2. Transfer Principles

- Pass digests + file paths between specialists
- Never transfer full-text content

### 3. Parallel Execution

- Independent specialist tasks run in parallel
- Sequential execution only when dependencies exist

---

## MAMAS Workflow

```
User Task
  ↓
Read specialists/planner.md
  ↓
Planner Decision
  ├─ Scan notes/*/playbook.md for matching patterns
  ├─ Match found? → Reuse existing playbook
  ├─ No match? → Check if required specialists exist
  │     ├─ Specialists available → Create new playbook
  │     └─ Specialist missing → Invoke Talent Architect → Create playbook
  ├─ Identify applicable Patterns from patterns/ directory (NEW)
  └─ Assemble task package with playbook path + pattern references
  ↓
Read specialists/coordinator.md
  ↓
Coordinator reads notes/{task}/playbook.md
  ↓
Coordinator ensures specialists can access specified Patterns (NEW)
  ↓
Dispatches specialists (parallel / sequential)
  ├─ Specialists read assigned Patterns before execution (NEW)
  ├─ Specialists apply Pattern constraints during execution (NEW)
  ├─ Creates inter-specialist context (cache/.context/)
  └─ Manages handoff between specialists
  ↓
Coordinator validates output against Pattern criteria (NEW)
  ├─ Compliant → Proceed
  └─ Non-compliant → Request specialist to revise
  ↓
Output to output/
  ↓
Auto-generate output digest → output/.meta/
  ↓
Planner evaluates specialist performance & Pattern effectiveness
  ├─ Quality acceptable → Complete
  ├─ Quality below expectations → Flag for Talent Architect upgrade
  └─ Pattern insufficient → Flag for Pattern refinement (NEW)
```

---

## Self-Evolution

The system improves itself through a continuous loop:

1. **Gap detection**: Planner identifies missing specialist or playbook
2. **Specialist creation**: Talent Architect designs and integrates the new agent
3. **Performance evaluation**: Planner assesses output quality post-task
4. **Specialist upgrade**: Talent Architect refines underperforming agents
5. **Registry update**: `.claude/experts-index.json` stays current

Every task permanently expands system capability.

---

## Prohibitions

- Skipping routing and executing directly
- Running simple tasks through MAMAS (wastes ~1,600 tokens)
- Accessing another task's `cache/`
- Re-reading large files without checking digest cache
- Transferring full-text without generating digests
