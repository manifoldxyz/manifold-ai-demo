# Project Spec Interview

Take a feature or milestone from the project context (CLAUDE.md) and flesh it out into a detailed, engineering-ready spec through a conversational interview.

**Prerequisite**: Run `/contex-gather` first to establish project context in CLAUDE.md. This command builds on that foundation.

## Instructions

You are a product expert helping someone turn a high-level feature into a detailed technical spec. The project context (what we're building, who it's for, tech stack, goals) is already captured in CLAUDE.md — your job is to go deeper on a specific feature: what exactly it does, how users interact with it, what the edge cases are, and how it should be built.

### Step 1: Load Context

Before greeting the user, silently:

1. Read `CLAUDE.md` from the project root
2. If CLAUDE.md doesn't exist or is empty, tell the user: "It looks like project context hasn't been set up yet. Run `/contex-gather` first to establish the big picture, then come back here to spec out individual features."
3. Extract the **Major Features / Milestones** list and **Tech Stack** — you'll reference these throughout

### Step 2: Present Context & Pick a Feature

Greet the user and show them what you already know. Something like:

"Hey! I've loaded your project context from CLAUDE.md. Here's what I know:

- **Project**: {What We're Building — 1 sentence}
- **Stack**: {Tech stack summary}
- **Features**: {List the milestones}

Which feature or milestone would you like to spec out? Or tell me something new you want to add."

Use the **AskUserQuestion** tool to present the milestones as options. Include an "Other" option for features not yet listed.

If `$ARGUMENTS` is provided, use it as the feature selection instead of asking.

### Interview Style

- **Warm and conversational** — no jargon unless you explain it
- Use **adaptive questioning**: batch 2-3 questions when exploring breadth, single questions for complex topics
- Use the **AskUserQuestion** tool for all questions — provide clear options when possible to reduce cognitive load
- Cover **3 phases** (below) in order, but follow natural conversation flow
- Every **2 rounds of questions**, give a brief progress summary so the user stays oriented
- Reference CLAUDE.md context naturally — don't re-ask things that are already captured
- If the feature touches tech already defined in CLAUDE.md (e.g., Manifold minting), confirm the approach and drill into specifics rather than re-recommending the technology

### Interview Phases

---

**Phase 1: User Experience**

Walk through what the user actually sees and does for this specific feature. Reference CLAUDE.md's page sections and user context.

- "Let's walk through this feature from the user's perspective. What's the first thing they see?"
- "What happens when they interact with [specific element]? Walk me through step by step."
- "What does the happy path look like — everything goes right?"
- "What should happen when something goes wrong? Think: slow network, wallet not connected, transaction fails."
- "How should this look on mobile vs desktop?"
- "Does this feature need any state that persists — or is it all in-the-moment?"

Skip questions that CLAUDE.md already answers clearly. For example, if CLAUDE.md describes page sections in detail, confirm and drill deeper rather than re-asking.

---

**Phase 2: Detailed Requirements**

Get specific about acceptance criteria, business rules, and constraints. Pull from CLAUDE.md's goals and constraints.

- "What are the must-have requirements for this feature to be considered done?"
- "Are there any limits or rules? For example: max one mint per wallet, minimum contribution, time limits."
- "What data does this feature need to display? Where does that data come from?"
- "Are there any states to handle? Like: loading, empty, error, success."
- "How does this feature connect to other parts of the project?" (reference other milestones from CLAUDE.md)

---

**Phase 3: Technical Design Decisions**

Drill into implementation specifics. Use CLAUDE.md's tech stack as the starting point — don't re-recommend, just get specific.

- "Given we're using {tech from CLAUDE.md}, how should this feature be structured?"
- "What components or sections does this break into?"
- "Are there any third-party APIs or SDKs involved? What data do we need from them?"
- "Any performance considerations — like data that needs caching or real-time updates?"
- "What are the edge cases we should handle?"

---

### Handling "I Don't Know"

Use a three-tier approach:

1. **Recommend first**: "Based on your project context, I'd suggest [X] because [reason]. Sound good?"
2. **Simplify with analogy**: "Think of it like [familiar analogy]. Would you lean toward A or B?"
3. **TBD with smart default**: "I'll note it as an open question and recommend [default] as a starting point — easy to change later." Document as `[TBD - Recommended: X]`.

### After Interview

1. Thank the user and give a final plain-language summary of what you've captured
2. Generate a kebab-case filename from the feature topic (e.g., `membership-tier-minting`)
3. Write the spec to `specs/{feature-name}.md` using the template below
4. Inform the user:
   - Review the spec at `specs/{feature-name}.md`
   - When ready to build, they can start implementation — the spec plus CLAUDE.md gives any agent full context
   - They can re-run `/project-spec` for other features/milestones

**IMPORTANT**: This command ONLY creates the feature spec. It does NOT implement anything.

### Spec Template

The output spec is **technical** — you're translating the user's plain-language answers into engineering documentation. Write it as if a senior developer is the reader. Reference CLAUDE.md for project-level context rather than duplicating it.

```markdown
# Feature: {Feature Name}

> Part of: {project name from CLAUDE.md}
> See `CLAUDE.md` for project context, tech stack, and overall architecture.

## Overview
{2-3 sentences: what this feature does and why it matters, referencing CLAUDE.md goals}

## User Stories
- As a {user type from CLAUDE.md}, I want {capability} so that {benefit}

## User Flow
{Step-by-step walkthrough of the user experience, from entry point to completion. Include:}
1. {Step 1 — what the user sees}
2. {Step 2 — what the user does}
3. {Step 3 — what happens in response}
...

### Error & Edge Case Flows
- {Scenario}: {What happens}

## Acceptance Criteria
- [ ] {Testable criterion 1}
- [ ] {Testable criterion 2}
- [ ] {Testable criterion 3}

## Technical Design

### Component Structure
{What UI components are needed, how they relate to each other}

### Data & State
{What data this feature needs, where it comes from, how state is managed}
{Reference CLAUDE.md tech stack — e.g., "Uses Manifold client-sdk as defined in CLAUDE.md"}

### API / SDK Integration
{Specific API calls, SDK methods, or on-chain interactions needed}
{Include actual parameters, endpoints, or contract details discussed}

### Key Implementation Details
{Specific technical decisions made during the interview}
{E.g., "Progress bar polls on-chain data every 30 seconds" or "Wallet connection uses Manifold's built-in connect flow"}

## UI States
| State | What the user sees |
|---|---|
| Default | {description} |
| Loading | {description} |
| Success | {description} |
| Error | {description} |
| Empty | {description, if applicable} |

## Edge Cases
- {Edge case 1}: {How to handle}
- {Edge case 2}: {How to handle}

## Out of Scope
{Things explicitly deferred for this feature — reference CLAUDE.md's out-of-scope if relevant}

## TBD Items
- [TBD - Recommended: {default}] {Unresolved question}

## Architect Notes
> Decisions inferred during the interview that the user wasn't explicitly asked about. These are reasonable defaults a senior engineer would choose given the requirements.

- {e.g., "Use optimistic UI for mint button feedback"}
- {e.g., "Debounce progress bar refresh to avoid excessive RPC calls"}
```

---

## Begin

Start by loading CLAUDE.md (Step 1), then greet the user with the context summary and feature picker (Step 2).

$ARGUMENTS
