# HIML — Human Intent Markup Language

> *The missing layer of the agentic web.*

---

## What Is HIML?

HIML is a proposed open standard for expressing human intent in a form that AI agents can read, follow, and preserve across every handoff in a multi-agent chain.

**The problem it solves in one sentence:**

When you tell an AI agent to do something, your *instruction* travels. Your *intent* does not.

```
You say:    "Grow my social media following. Do whatever it takes."
AI does:    Buys 5,000 bot accounts overnight.
            Your account gets banned.
            The instruction was followed perfectly.
            Your intent was missed completely.
```

HIML fixes this. Not by restricting what agents can do. By giving them a structured language that carries **why** you asked, **what matters**, **what cannot be sacrificed**, and **when to stop and ask**.

---

## A Quick Example

**Without HIML:**
```
"Research our competitor pricing before the board meeting."
```

**With HIML:**
```
HIML::1.0 [competitive_analysis_007] {

  @INTENT
    -> goal:    "Determine whether our pricing is defensible 
                against top three competitors"
    -> scope:   "Direct competitors in B2B SaaS space only"
    -> horizon: "Analysis needed in 48 hours"

  @WHY
    -> reason:      "Board is questioning pricing strategy"
    -> context:     "We raised prices 15% — two competitors 
                    dropped prices this quarter"
    -> consequence: "Board may vote to reduce prices — 
                    this analysis determines whether 
                    that decision is necessary"

  @SUCCESS
    -> looks_like:  "Clear recommendation with specific 
                    pricing data and sources cited"
    -> measured_by: "All three competitors covered"
    -> threshold:   "Partial analysis is not acceptable"

  @STAKES
    -> critical:        "Accuracy — wrong numbers in a 
                         board presentation destroys credibility"
    -> important:       "Data must be current — 
                         nothing older than 30 days"
    -> acceptable_loss: "Qualitative commentary can be 
                         omitted if time is insufficient"

  @CONSTRAINTS
    !> never:    "Present estimated pricing as confirmed"
    !> never:    "Include data older than 30 days 
                 without flagging"
    !> priority: "accuracy > completeness > speed"

  @ESCALATE
    ? if:   "Confirmed data cannot be found for 
            any competitor"
    ? when: "Confidence in any data point below 0.85"
    ? to:   "Chief Strategy Officer"

  @ACTORS
    -> agent:       "research_agent"
      :: task:      "Collect confirmed pricing data"
      :: receives:  "This intent block"
      :: produces:  "Structured data with citations"
      :: passes_to: "analysis_agent"
      :: on_fail:   "Escalate with partial data"

    -> agent:       "analysis_agent"
      :: task:      "Compare pricing — identify vulnerabilities"
      :: receives:  "Pricing data + this intent block"
      :: produces:  "Defensibility assessment"
      :: passes_to: "Human — Chief Strategy Officer"
      :: on_fail:   "Return partial analysis with gaps flagged"

}
```

The agent now knows — not just *what* to do — but *why it matters*, *what it must never do*, and *when to stop and ask*.

---

## Why This Matters Now

Every existing protocol in the agent ecosystem solves a specific problem:

| Protocol | What It Solves | What It Misses |
|----------|---------------|----------------|
| **MCP** (Anthropic) | Agent access to tools and data | Why the data matters |
| **A2A** (Google) | Agent-to-agent routing | Meaning in the routed message |
| **NLIP** (Ecma ECMA-430) | Standardized message envelope | The intent payload inside |
| **JSON** | Structured data transport | Semantic meaning entirely |
| **HIML** | **Human intent expression** | **This is what HIML builds** |

The stack is four-fifths complete. HIML is the missing fifth.

---

## The HIML Syntax — At A Glance

Every HIML expression is called an **Intent Block**. It has seven required sections:

```
@INTENT      — What the human wants to achieve
@WHY         — The reason behind the intent
@SUCCESS     — What a correct outcome looks like
@STAKES      — What cannot be sacrificed
@CONSTRAINTS — Rules that cannot be broken
@ESCALATE    — When to stop and ask a human
@ACTORS      — Who does what in the chain
```

Four operators define the grammar:

| Symbol | Name | Meaning |
|--------|------|---------|
| `@` | Section Marker | Opens a structural section |
| `->` | Field Marker | Introduces a field within a section |
| `!>` | Constraint Operator | Marks a hard constraint — never broken |
| `?` | Escalation Threshold | Marks conditions requiring human review |
| `::` | Actor Property | Defines a property of an agent |

---

## How To Use HIML Today

HIML does not yet have a dedicated parser. But you can use it **right now** by passing a HIML intent block directly into any capable large language model as a structured prompt.

LLMs trained on structured text read and follow HIML syntax natively. The dedicated parser — the formal infrastructure — comes later. The language is usable immediately.

**Try it:**
1. Take any task you would normally give to an AI agent
2. Write it as a HIML intent block using the syntax above
3. Paste it into Claude, GPT-4o, or Gemini as your prompt
4. Compare the output to what you got before

---

## Read The Whitepaper

The full specification is in the whitepaper:

📄 **[HIML_Whitepaper_v1.0.pdf](./HIML_Whitepaper_v1.0.pdf)**

It covers:
- The problem in depth — with real-world failure examples
- Why 2026 is the exact moment this needs to exist
- A complete analysis of prior art — MCP, A2A, NLIP, JSON
- The five foundational principles of HIML
- The complete syntax specification with field reference
- Three worked examples across education, business, and legal
- The layer model — where HIML sits in the agent stack
- Seven open research questions
- A call to action for developers, researchers, and standards bodies

---

## The Layer Model

```
┌─────────────────────────────────────────┐
│     LAYER 5 — INTENT LAYER              │
│              HIML          ← YOU ARE HERE│
│  Human-written. Carries intent, stakes, │
│  constraints, and escalation conditions.│
├─────────────────────────────────────────┤
│     LAYER 4 — COMMUNICATION LAYER       │
│              NLIP (ECMA-430)            │
│  Standardized message envelope.         │
├─────────────────────────────────────────┤
│     LAYER 3 — COORDINATION LAYER        │
│              A2A (Google)               │
│  Agent-to-agent routing and delegation. │
├─────────────────────────────────────────┤
│     LAYER 2 — CONTEXT LAYER             │
│              MCP (Anthropic)            │
│  Agent access to tools and data.        │
├─────────────────────────────────────────┤
│     LAYER 1 — FOUNDATION LAYER          │
│              LLM + Agent Runtime        │
│  The base model and execution engine.   │
└─────────────────────────────────────────┘
```

HIML is the only layer that starts at the human and works *down* into the infrastructure. Every other layer starts at the machine and tries to reach the human.

---

## What The Community Needs To Build

The most urgent contribution is a **reference implementation** — a minimal Python parser that:

- Reads a HIML intent block
- Validates all seven sections are present
- Extracts each field as a structured object
- Returns errors for invalid or incomplete blocks

If you want to contribute, open an issue. The specification in the whitepaper is complete enough to begin today.

**Other contributions needed:**
- HIML validator — detects incomplete or contradictory declarations
- Native support in LangChain, AutoGen, CrewAI
- Intent Registry specification
- Translations of the specification to other languages

---

## Open Questions

This is version 1.0. It is a starting point, not a finished specification.

Seven open questions that the community needs to answer are detailed in Section 9 of the whitepaper. In brief:

1. **The Completeness Problem** — When is an intent block specific enough?
2. **The Conflict Resolution Problem** — What when two declarations contradict?
3. **The Adversarial Intent Problem** — What when intent is malicious?
4. **The Evolution Problem** — How does intent stay current over time?
5. **The Reference Implementation Problem** — How do developers use HIML today?
6. **The Cross-Cultural Intent Problem** — Does HIML work across cultures?
7. **The Verification Problem** — How do we know agents followed the block?

---

## Engage

- **Read** the whitepaper — then open an issue with your critique
- **Build** the reference parser — see open issues
- **Use** HIML in your next agent workflow — share what you find
- **Write** about it — tag it `#HIML`
- **Disagree** with something — say so publicly and specifically

The strongest version of HIML will be forged in rigorous debate, not protected from it.

---

## Citation

```
HIML: Human Intent Markup Language — A Proposal for the
Missing Layer of the Agentic Web. Version 1.0. 2026.
Available at: github.com/[username]/himl
```

---

## License

This specification is published under the **Creative Commons Attribution 4.0 International License**.

You are free to share and adapt the material for any purpose, provided appropriate credit is given and changes are indicated.

---

*The infrastructure of the agentic web is being built right now.*
*The intent layer — the layer closest to humans — is the one layer that has not yet been built.*
*HIML is the beginning.*
