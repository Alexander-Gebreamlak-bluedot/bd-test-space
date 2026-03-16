# Meeting Summary: Setting Up Complex Projects with Claude

**Presenter:** James Aufricht
**Date:** March 9, 2026

---

## Core Thesis

The standard advice of "just write a great prompt" sets users up for failure. Instead, complex projects need a structured **Brainstorm → Plan → Implement** framework that leverages Claude's effort, not its intelligence.

---

## The Problem

- Claude is marketed as if you can dump a vague idea and get perfect output
- This creates a frustrating back-and-forth cycle: Claude guesses wrong, you correct, it guesses wrong again
- The "perfect prompt" mindset from LinkedIn thought-leaders is also wrong — no single prompt solves this

---

## What Claude Is Actually Good At

- Managing large amounts of context
- **Sticking to a plan** and closing tasks quickly
- Adapting to feedback retroactively (e.g., propagating a mid-project correction back through earlier work)

**Key mindset:** Rely on Claude's **effort**, not its **intelligence**. Set up high-effort, low-intelligence workflows.

---

## The Three-Step Framework: Brainstorm → Plan → Implement

### Step 1: Brainstorm

- Conversational phase where **you** tell the tool what matters
- Not "build this for me" — instead: *"My goal is X, I'm solving Y"*
- Your first prompt should ask Claude to **set up the project scaffolding**, not deliver the final product

**Practical how-to:**
Your first prompt should look something like:

> *"My goal is X. I am looking to solve this problem. Make me a folder with the following documents: `project-spec.md`, `to-dos.md`, and a `reference/` folder."*

Using git and your filesystem:
```
my-project/
├── project-spec.md      # What we're building, constraints, design references
├── to-dos.md            # Tracked task list with checkboxes
├── narrative.md          # Source content (if applicable)
└── reference/            # Supporting materials, screenshots, specs
```

- **Create a git repo** (or folder within one) so every iteration is versioned
- `project-spec.md` is the single source of truth — audience, goals, format, technical constraints, build procedure
- `to-dos.md` is the living checklist Claude executes against

### Step 2: Plan

- Build up the spec and the task list in `to-dos.md` collaboratively
- Break work into sequenced, checkable items grouped by phase (pre-build, build, post-build)
- Claude proposes; you approve, reorder, or refine
- This is where you encode your preferences, edge cases, and non-obvious requirements *before* any code is written

### Step 3: Implement

- Claude executes against the task list, checking items off as it goes
- Add human checkpoints wherever you want review gates
- Mid-project corrections are easy: update the spec or narrative, then tell Claude to propagate changes backward through completed work
- Doesn't need to be perfect up front — the structure makes iteration cheap

---

## A Real Example

The [NIID Presentation project](https://github.com/jamesaufricht-bd/product-internal/blob/main/feb-niid-presentation/project-spec.md) demonstrates this framework end-to-end:

**Folder structure:**
```
feb-niid-presentation/
├── project-spec.md             # Detailed spec: audience, sections, components, technical reqs
├── narrative.md                # Source content (24 slides across 4 sections)
├── to-dos.md                   # Phased checklist (pre-build → build → post-build)
├── activity-log.md             # Session history
├── niid-presentation.html      # v1 output
└── niid-presentation-v2.html   # v2 output (iterated from feedback)
```

**What made it work:**

- **The spec** ([`project-spec.md`](https://github.com/jamesaufricht-bd/product-internal/blob/main/feb-niid-presentation/project-spec.md)) defined everything before implementation: 6 HTML sections, which design components to use, card-by-card content mapping, technical constraints (single self-contained HTML, no external images, base64 logo), and an explicit build procedure. Claude and I did this together.
- **The to-do list** ([`to-dos.md`](https://github.com/jamesaufricht-bd/product-internal/tree/main/feb-niid-presentation)) broke the build into 15+ discrete, checkable tasks across pre-build, build, and post-build phases — each one independently verifiable. Claude and I built this together. 
- **Mid-project adaptation was trivial:** when card formatting needed to change from prose to structured bullets/sub-headers, a v2 was produced by updating the spec and re-executing — no starting from scratch
- **The remaining open items** (browser test, scroll test, final self-contained pass) show how the checklist naturally surfaces what's left to do. Claude did most of the work here. 

---

## Anti-Patterns Called Out

- Starting with "build this thing for me"
- Agonizing over a single perfect prompt
- Letting Claude guess your intent without structured context
- Skipping the spec and jumping straight to implementation

---

## Tool Recommendation: Superpowers

The Brainstorm → Plan → Implement sequence described in this talk is exactly what the open-source **[Superpowers](https://github.com/obra/superpowers)** skill framework automates for you. Once installed, you don't need to remember the steps — Claude triggers them automatically.

**What Superpowers does:**

Superpowers is a set of composable "skills" that attach to your coding agent (Claude Code, Cursor, Gemini CLI, Codex, etc.). When you start a task, the agent *doesn't* jump straight into writing code. Instead, it follows the same disciplined sequence from this talk — automatically:

| This Talk's Step | Superpowers Skill | What It Does |
|---|---|---|
| **Brainstorm** | `brainstorming` | Asks you what you're really trying to do. Refines the idea through questions, explores alternatives, presents the design in digestible chunks for your sign-off. Saves a design document. |
| **Plan** | `writing-plans` | Breaks the approved design into bite-sized tasks (2-5 min each). Every task has exact file paths, code expectations, and verification steps. |
| **Implement** | `executing-plans` / `subagent-driven-development` | Executes the plan task-by-task with human checkpoints, or dispatches parallel subagents with two-stage review (spec compliance, then code quality). |

**Additional skills that support the workflow:**

- `using-git-worktrees` — creates an isolated branch/workspace before implementation starts
- `test-driven-development` — enforces red-green-refactor during implementation
- `requesting-code-review` / `receiving-code-review` — structured review between tasks
- `verification-before-completion` — requires evidence (test output, screenshots) before declaring "done"
- `finishing-a-development-branch` — guides merge/PR/cleanup when work is complete

**Post-setup 

This also helps during your project. 

1. Claude will forget to check its context from time-to-time. If you have a good spec file, if you have good reference material, you can say: 'that's wrong, review [this doc] carefully and think about. Then try again.' 
2. We'll get into this later, but Claude won't have enough memory to do your task fully. So having these documents are critical, because you will need to start a new session where Claude has zero memory of what happened before. Telling it to 'read the spec, activity log and task list, then pick up the next task' is a vital pattern. 
3. This approach also helps you multi-task with multiple agents. You can start 2-4 agents in one session, tell them to pick up different tasks and let them rip. The review process can feel a bit heavy, but you can get a ton done this way. 
4. It also helps you think about why Claude might be screwing up: are the instructions confusing? Does it have all the research it needs in the reference folder? 


** OK, so what's my first step 

1. If you have Claude and git set-up, tell it to save this file in reference. Then write "read this document very carefully and propose next steps." 
