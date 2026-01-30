# AI-Skills-files-explanation

## What are skills files

skills.md tells humans what the agent means to be.
skills.json tells machines what the agent is allowed to do.

1) skills.md is not for the model.
It is for humans, governance, and the agent system around the model.
Think of it as the agent’s capability contract.

skills.md is the agent’s:
📜 Contract (what it can and cannot do)
🧭 Compass (when to act)
🛡 Safety boundary
🧩 Interoperability spec
📚 Human-readable brain map

2) skills.json is the agent’s executable constitution—hard rules that decide what the agent can and cannot do at runtime. skills.json is a machine-readable capability specification for an AI agent.
It tells the agent runtime (planner/router/executor):
“These are the exact skills this agent has, the conditions under which each skill can run, the tools it may use, and the guarantees it must enforce.”
Unlike prompts or markdown docs, skills.json is parsed, validated, and enforced at runtime.

Core Purposes of skills.json
1. Skill routing & activation
When a user sends input, a planner asks:
“Which skill matches this intent and is allowed right now?”
skills.json answers by defining:
triggers
input schemas
preconditions
If no skill matches → the agent must refuse or ask for clarification.
2. Hard safety & scope boundaries
Unlike prompts, skills.json cannot be overridden by the model.
Examples:
Allowed tools only
Allowed domains only
Max output size
Prohibited behaviors
If violated → execution is blocked, not “politely ignored.”
3. Deterministic input/output contracts
Each skill defines:
required inputs
types and enums
output schema
This enables:
tool chaining
downstream automation
reliable evaluation
multi-agent handoffs


Dimension
skills.md
skills.json
Primary audience
Humans
Machines
Purpose
Explain what and why
Enforce how and when
Readability
High (narrative)
Low (strict schema)
Executed by agent
❌ No
✅ Yes
Acts as contract
✅ Conceptual
✅ Enforced
Drift resistance
Medium
High
Required in prod
Recommended
Mandatory


Example: Same Skill, Two Representations

skills.md (Human-friendly)

Md
"This skill generates executive briefings using trusted sources only.
It must never fabricate numerical claims and must clearly state uncertainty."


skills.json (Machine-enforced)

Json
{
  "id": "exec_brief",
  "max_tokens": 700,
  "allowed_tools": ["web.search", "web.open"],
  "allowed_domains": [
    "reuters.com",
    "ft.com",
    "nature.com"
  ],
  "fabrication": false,
  "require_citations": true
}

## Where skills.md files fits in AI agent workflow 

skills.md
   ↓
agent.md (role + goals)
   ↓
planner / router
   ↓
workflow (graph / chain)
   ↓
tools + model calls
   ↓
evaluators (did it obey skills?)

## How skills.json Is Used at Runtime, in a AI workflow or AI pipeline 

Execution flow

User input
   ↓
Intent detection
   ↓
Skill match (skills.json)
   ↓
Input validation
   ↓
Tool + constraint enforcement
   ↓
LLM execution
   ↓
Output validation
   ↓
Final response

If any step fails → execution halts.

## Failure modes
If skills.md is wrong:
Humans misunderstand the agent
Design confusion
Still runs
If skills.json is wrong:
Agent misfires
Tool calls fail
Skill is unusable


## A git folder structure example whrv skills files

ai-agent-exec-brief/
│
├── README.md                     # High-level description of the agent
│
├── skills/
│   ├── skills.md                 # Human-readable capability contract (THIS FILE)
│   ├── skills.json               # Machine-readable skill definitions (optional)
│   └── skill_matrix.md           # Mapping of skills → tools → models (optional)
│
├── agents/
│   ├── executive_brief_agent.md  # Agent role, goals, personality, success criteria
│   ├── research_agent.md         # (If multi-agent) research-focused agent
│   └── risk_agent.md             # (If multi-agent) impact & risk framing agent
│
├── prompts/
│   ├── system_prompt.md          # High-level system rules
│   ├── planner_prompt.md         # Planning / routing instructions
│   ├── critic_prompt.md          # Self-review / evaluation prompt
│   └── style_prompt.md           # Output tone & formatting rules
│
├── tools/
│   ├── web_search.py              # Web search wrapper
│   ├── source_validator.py       # Enforces trusted-source policy
│   ├── citation_formatter.py     # Citation normalization
│   └── __init__.py
│
├── workflows/
│   ├── exec_brief.yaml            # End-to-end workflow graph
│   ├── research_only.yaml
│   └── escalation_rules.yaml
│
├── policies/
│   ├── safety.md                  # Safety & misuse constraints
│   ├── sourcing.md                # Allowed / disallowed source list
│   └── compliance.md              # Regulatory notes (if applicable)
│
├── memory/
│   ├── short_term.md              # Session memory schema
│   ├── long_term.md               # Persistent memory rules
│   └── vector_index.md            # Embedding + recall logic
│
├── evaluators/
│   ├── factuality_check.py        # Hallucination detection
│   ├── source_check.py            # Citation enforcement
│   └── output_quality.md
│
├── configs/
│   ├── agent_config.yaml          # Model selection, temps, budgets
│   ├── tools_config.yaml          # Tool limits & timeouts
│   └── env.example                # Environment variables template
│
├── tests/
│   ├── test_skills.py             # Ensure skills behave as defined
│   ├── test_sources.py
│   └── test_outputs.py
│
└── logs/
    └── README.md                  # Runtime logs (gitignored)










