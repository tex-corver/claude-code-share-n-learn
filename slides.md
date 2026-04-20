---
theme: default
title: Claude Code — Share & Learn
info: |
  ## Claude Code — Share & Learn
  Three shifts that changed how I use Claude Code.
  Presented by Duy Bui · 21/04/2026
author: Duy Bui
presenter: true
download: false
exportFilename: claude-code-share-and-learn
highlighter: shiki
lineNumbers: false
drawings:
  persist: false
transition: slide-left
mdc: true
colorSchema: dark
fonts:
  sans: Inter
  mono: Fira Code
routerMode: hash
hideInToc: false
themeConfig:
  primary: '#d97757'
layout: cover
---

# Claude Code

## Share & Learn

<div class="pt-12">
  <div class="text-xl opacity-80">Three shifts that changed how I use it every day</div>
</div>

<div class="abs-bl mx-14 my-12 text-sm opacity-70">
  <div><strong>Duy Bui</strong></div>
  <div>21/04/2026</div>
</div>

<!--
Welcome. I'll do a 25-minute talk and we'll have 5 minutes for Q&A at the end.
Don't rush the opening. Let the audience settle. You have their attention — use it.
-->

---
layout: center
class: text-center
---

# "I was using it wrong."

<div v-click class="mt-8 text-xl opacity-80">
For the first couple of months.
</div>

<div v-click class="mt-12 max-w-2xl mx-auto text-left text-base opacity-90">

My loop was: paste requirements, say "implement this," watch it go. Then spend half my time cleaning up.

Then I changed one thing. I stopped asking it to write code first. **I started asking it to write a design doc first.**

My rework dropped. My reviews got easier — because I wasn't reviewing code, I was reviewing **thinking**.

</div>

<!--
DELIVERY:
- Pause after the first line. Let it land.
- "about 5 months" sounds more honest than precise numbers.
- Don't oversell. "My rework dropped" is specific and believable.
- This is where you earn the next 25 minutes. Slow down.
-->

---
layout: center
class: text-center
---

# Three shifts

<div class="mt-8 grid grid-cols-1 gap-5 max-w-xl mx-auto text-left">

<div v-click class="flex items-center gap-6">
  <div class="text-4xl font-bold opacity-30 leading-none w-10 text-right">1</div>
  <div class="text-2xl">Plan before you code</div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-4xl font-bold opacity-30 leading-none w-10 text-right">2</div>
  <div class="text-2xl">Extend it with MCP</div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-4xl font-bold opacity-30 leading-none w-10 text-right">3</div>
  <div class="text-2xl">Thinking partner, not typing partner</div>
</div>

</div>

<!--
Say them slowly. This is your table of contents.
Each click reveals one — use the pause to make eye contact.
-->

---
layout: section
---

# 1 · Core concepts

<div class="opacity-60 text-lg mt-4">Four mental models, not a feature tour</div>

---
layout: center
---

# Context is everything

<div class="text-xl opacity-80 mt-6">
Claude's context window holds <em>everything</em> it knows about your session.
</div>

<div class="text-lg opacity-60 mt-10">
Most people think it's just "the chat so far."<br>
It's much more than that.
</div>

<!--
Set up the mental model before the breakdown.
Next slide shows what actually lives in that window.
-->

---

# What's in your context window <span class="text-sm opacity-60 font-normal">(200k total)</span>

<div class="grid grid-cols-2 gap-8 mt-4 text-sm">

<div>

### Loaded before you type

<v-clicks>

- System prompt — **4,200**
- Environment info — **280**
- `~/.claude/CLAUDE.md` — **320**
- Project `CLAUDE.md` — **1,800**
- Skill descriptions — **450**
- MCP tool names *(deferred)* — **120**
- Auto memory (`MEMORY.md`) — **680**

</v-clicks>

</div>

<div>

### Added as Claude works

<v-clicks>

- Single file read (`auth.ts`) — **2,400**
- Every tool result
- Path-scoped rules (on match)
- Hook outputs
- Its own responses

</v-clicks>

</div>

</div>

<div v-click class="mt-8 text-center text-xl">

**File reads dominate.** One file ≈ your whole `CLAUDE.md`.

</div>

<!--
Source: code.claude.com/docs/en/context-window
Real numbers from the interactive timeline. 200k budget total.
When sessions get slow or "forget" — 9/10 times it's context pressure.
-->

---

# Three commands to learn this week

<div class="mt-4">

| Command | What it does | When to use |
|---|---|---|
| `/context` | Live context usage by category, with suggestions | Session feels slow or expensive |
| `/memory` | Shows which `CLAUDE.md` and auto-memory files loaded | Claude seems to be "forgetting" |
| `/compact` | Replace history with structured summary | Long session, want to keep going |

</div>

<div v-click class="mt-10 text-center text-xl">

*"Manage context like you'd manage memory in a tight loop."*

</div>

<!--
These three are free leverage. Most people never run /context once.
-->

---
layout: center
---

# Live: `/context`

<div class="mt-8 text-lg opacity-80 max-w-2xl mx-auto text-left">

1. Run `/context` in a session open for a while
2. Point at the biggest eater — usually conversation or unused MCP tools
3. **"This is why your sessions get slow. It's not the model — it's the context you're feeding it."**

</div>

<div v-click class="mt-10 p-6 border border-dashed border-gray-500 rounded max-w-xl mx-auto text-left">

<div class="text-xs opacity-70 mb-2">FALLBACK (if live breaks)</div>

```
Context: 142k / 200k tokens (71%)

  System prompt           4.2k
  CLAUDE.md (project)     1.8k
  Skill descriptions      0.5k
  MCP tool names          0.1k
  ─────────────────────────────
  File reads             48.0k  ← dominant
  Conversation           78.0k
  Tool results            9.4k
```

</div>

<!--
60-second demo max. If it fails, just describe the output — everyone's seen a token counter.
-->

---

# `CLAUDE.md` — free leverage, or free tax

<div class="mt-4 text-sm">

| ✅ Include | ❌ Exclude |
|---|---|
| Bash commands Claude can't guess | Anything Claude can figure out by reading code |
| Code style rules **that differ from defaults** | Standard conventions Claude already knows |
| Testing instructions, preferred test runners | Detailed API docs *(link instead)* |
| Repo etiquette (branches, PRs) | Info that changes frequently |
| Architectural decisions specific to the project | File-by-file descriptions of the codebase |
| Dev environment quirks (required env vars) | Self-evident practices like "write clean code" |

</div>

<div v-click class="mt-6 text-center opacity-80">

For each line ask: <em>"would removing this cause Claude to make mistakes?"</em> If not — cut it.

</div>

<!--
Source: code.claude.com/docs/en/best-practices
Quote: "Treat CLAUDE.md like code: review it when things go wrong, prune it regularly."
Bloated CLAUDE.md → Claude ignores half of it. Rule gets lost in noise.
-->

---

# A minimal `CLAUDE.md` <span class="text-sm opacity-60 font-normal">(from the docs)</span>

````md
# Code style
- Use ES modules (import/export) syntax, not CommonJS (require)
- Destructure imports when possible (eg. import { foo } from 'bar')

# Workflow
- Be sure to typecheck when you're done making a series of code changes
- Prefer running single tests, and not the whole test suite, for performance
````

<div v-click class="mt-6 grid grid-cols-2 gap-4 text-sm">

<div>

**Where it lives:**

- `~/.claude/CLAUDE.md` — global
- `./CLAUDE.md` — project (commit it)
- `./CLAUDE.local.md` — personal (gitignore)

</div>

<div>

**Compose with imports:**

```md
See @README.md for overview.
Git workflow: @docs/git.md
```

</div>

</div>

<!--
Source: code.claude.com/docs/en/best-practices, memory
Point out: imports via @path are underused — lets you split CLAUDE.md without bloat.
-->

---

# Also worth knowing

<div class="grid grid-cols-2 gap-8 mt-8">

<div>

### Subagents

Separate context window. Only the summary returns.

<div class="mt-3 p-3 bg-gray-800/40 rounded text-sm font-mono">
"Use subagents to investigate how<br>our auth system handles token refresh."
</div>

<div class="text-sm opacity-70 mt-2">Subagent reads 40 files — your context gets the 200-word summary.</div>

</div>

<div>

### Path-specific rules

Live alongside files, **auto-load when Claude touches that path**.

Great for *"when editing this directory, follow these conventions."*

</div>

</div>

<!--
Subagent pattern is one of the highest-leverage techniques in the docs.
"Since context is your fundamental constraint, subagents are one of the most powerful tools available."
-->

---
layout: two-cols-header
---

# Explore → Plan → Implement → Commit

::left::

### The 4-phase workflow

<v-clicks>

1. **Explore** *(plan mode)* — read files, ask questions
2. **Plan** — "what files change? what's the session flow?"
3. **Implement** — switch to normal mode, code against plan
4. **Commit** — descriptive message + PR

</v-clicks>

<div v-click class="mt-4 text-sm opacity-70">
Enter plan mode: <kbd>Shift</kbd>+<kbd>Tab</kbd> · <kbd>Ctrl</kbd>+<kbd>G</kbd> opens the plan in your editor
</div>

::right::

### When to skip it

<v-clicks>

- Fixing a typo
- Adding a log line
- Renaming a variable

</v-clicks>

<div v-click class="mt-4 text-base opacity-80">

*"If you could describe the diff in one sentence — skip the plan."*

</div>

<!--
Source: code.claude.com/docs/en/best-practices
Nuance matters. Plan Mode adds overhead. Reach for it when:
- scope is unclear
- change spans multiple files
- you don't know the code being modified
-->

---

# Sessions — pick up where you left off

<v-clicks>

- `claude --continue` — resume the most recent conversation
- `claude --resume` — pick from a list of recent sessions
- `/rename oauth-migration` — give it a name you'll find later
- `/rewind` or <kbd>Esc</kbd>+<kbd>Esc</kbd> — restore conversation, code, or both to any checkpoint

</v-clicks>

<div v-click class="mt-8 text-center opacity-80 max-w-2xl mx-auto">

Treat sessions like branches. Different workstreams → separate, persistent contexts.

</div>

<!--
Checkpoints are new and powerful. Every Claude action auto-checkpoints.
You can try risky things knowing you can rewind.
Note: checkpoints only track Claude's changes — not a git replacement.
-->

---

# Permission modes — <kbd>Shift</kbd>+<kbd>Tab</kbd> cycles them

<div class="text-sm mt-2">

| Mode | What runs without asking | Best for |
|---|---|---|
| **`default`** | Reads only | Unfamiliar code, sensitive work |
| **`acceptEdits`** | Reads + file edits + `mkdir/mv/cp` | Iterating on code you're reviewing |
| **`plan`** | Reads only (proposes changes, won't apply) | Exploring before changing |
| **`auto`** | Everything, with a classifier safety net | Long tasks, reducing prompt fatigue |
| **`bypassPermissions`** | Everything. No checks. | Isolated containers only |

</div>

<div v-click class="mt-4 text-sm opacity-80">

**`auto` blocks by default:** `curl | bash`, prod deploys, force-push to `main`, mass deletes.<br>
**`auto` allows:** edits in your dir, reading `.env` to its matching API, pushing to your own branch.

</div>

<div v-click class="mt-4 text-center opacity-80">

Scope the blast radius. Don't review the blast.

</div>

<!--
Source: code.claude.com/docs/en/permission-modes
Auto mode requires Max/Team/Enterprise/API + Sonnet 4.6+ or Opus 4.6+.
If the classifier blocks 3-in-a-row or 20 total, auto mode falls back to prompts.
-->

---
layout: section
---

# 2 · Live demo

<div class="opacity-60 text-lg mt-4">Plan-first workflow — from prompt to design doc</div>

---

# Demo: notification service design

<div class="text-lg opacity-80 mt-4">
Email, Slack, or webhook delivery. Retries. Dead-letter queue.
</div>

<div class="mt-8 text-sm opacity-70">Demo steps (presenter cue card):</div>

<div class="mt-2 grid grid-cols-2 gap-x-8 gap-y-3 text-sm">

<div><strong>1.</strong> Empty dir → <code>claude</code></div>
<div class="opacity-70">"No code yet. Thinking partner."</div>

<div><strong>2.</strong> Ask it to interview me (5–7 questions)</div>
<div class="opacity-70">This is the move.</div>

<div><strong>3.</strong> Answer briefly (pre-prepared)</div>
<div class="opacity-70">10k/day, peaks 500/min, Slack limits, eventual consistency</div>

<div><strong>4.</strong> Ask for <code>design.md</code></div>
<div class="opacity-70">Problem, goals, arch, failure modes, open questions</div>

<div><strong>5.</strong> Review live — skim headers</div>
<div class="opacity-70">Point out Slack rate limit pickup, DLQ split</div>

<div><strong>6.</strong> Iterate</div>
<div class="opacity-70">"Rewrite failure modes as a table. Add observability."</div>

</div>

<!--
PRE-PREPARED ANSWERS for step 3:
- Scale: ~10k notifications/day, peaks of 500/min
- Constraints: must handle Slack rate limits gracefully
- Trade-off preference: eventual consistency over complexity

FALLBACK: if demo breaks, "network's being weird" + switch to next slide (example design.md excerpt).
-->

---
layout: center
---

# I didn't write any code yet.

<div v-click class="mt-10 text-xl opacity-80 max-w-2xl mx-auto">

In 5 minutes I have a design doc that would've taken me 45 minutes to draft.

</div>

<div v-click class="mt-8 text-xl opacity-80 max-w-2xl mx-auto">

I'd take this doc to my tech lead, get sign-off, and **then** ask Claude to implement it.

</div>

<div v-click class="mt-12 text-2xl">

That's the workflow.

</div>

<!--
Land this hard. It's the core message of the whole talk.
The 5-minute doc is worth more than the 5-minute implementation.
-->

---
layout: section
---

# 3 · Extending Claude Code

<div class="opacity-60 text-lg mt-4">Six extension points. Two high-leverage ones today.</div>

---

# The full map — six extension points

<div class="mt-2">

| Feature | One-line |
|---|---|
| **`CLAUDE.md`** | Always-on context *(covered)* |
| **Skills** | Reusable workflows as slash commands |
| **MCP** | Connect Claude to external services *(demo)* |
| **Subagents** | Isolated context windows for side-quests |
| **Hooks** | Deterministic scripts on events (lint-on-edit) |
| **Plugins** | Packaging layer — bundle & share |

</div>

<div v-click class="mt-6 text-center opacity-80">

code.claude.com/docs/en/features-overview

</div>

<!--
90-second overview. Walk the table so the audience knows where things live.
The power isn't any single feature — it's knowing when to reach for which one.
-->

---

# When to add what

<div class="mt-6">

| If this happens... | Add... |
|---|---|
| Claude gets the same convention wrong twice | A line in `CLAUDE.md` |
| You paste the same multi-step prompt 3 times | A skill |
| You copy data from a browser tab Claude can't see | An MCP server |

</div>

<div v-click class="mt-10 text-center text-xl">

Don't build scaffolding you don't need.<br>
**Each feature has a trigger.**

</div>

<!--
This is the most memorable slide in the talk. Repeat the triggers out loud.
If someone only remembers one thing, make it this table.
-->

---

# Built-in slash commands — start with these

<div class="mt-4">

| Command | What it does | When to use |
|---|---|---|
| `/init` | Generates starter `CLAUDE.md` by reading your repo | First time in any codebase |
| `/review` | Reviews your current changes like a senior dev | Before committing |
| `/debug` | Bundled skill — structured bug investigation | Instead of "try again" loops |
| `/simplify` | Bundled skill — review & shrink complexity | After a feature lands |

</div>

<div v-click class="mt-6 text-center opacity-80">

Also bundled: `/batch`, `/loop`, `/claude-api`. Try `/init` today.

</div>

---

# Custom skills — your third-time trigger

<div class="text-sm opacity-70 mt-2">Lives in <code>.claude/skills/fix-issue/SKILL.md</code></div>

````md
---
name: fix-issue
description: Fix a GitHub issue
disable-model-invocation: true
allowed-tools: Bash(gh *) Edit
---

Fix GitHub issue $ARGUMENTS following our coding standards.

1. Use `gh issue view` to read the issue
2. Search the codebase for relevant files
3. Implement the fix, write tests
4. Create a descriptive commit and PR
````

<div class="mt-3 text-sm opacity-80">

Usage: <code>/fix-issue 1234</code>

</div>

<div v-click class="mt-4 text-xs opacity-70 max-w-3xl">

`description` helps Claude decide when to auto-invoke. `disable-model-invocation: true` = only you can trigger it (use for anything with side effects — deploys, commits, Slack).

</div>

<!--
Source: code.claude.com/docs/en/skills
Other useful frontmatter: allowed-tools, model, effort, paths (glob), context: fork (runs in subagent).
Skill directories can bundle templates/, examples/, scripts/ alongside SKILL.md.
-->

---
layout: center
---

# MCP — Model Context Protocol

<div class="mt-6 text-xl opacity-80 max-w-3xl mx-auto">

How you let Claude Code use tools that **aren't your file system**.

</div>

<div v-click class="mt-10 text-lg opacity-90 max-w-3xl mx-auto">

Without MCP: read files, run commands.<br>
With MCP: **read your Notion. Query your database. Modify a design file.**

</div>

<div v-click class="mt-8 text-lg opacity-80">

Think of MCP servers as plugins that **teach Claude new verbs.**

</div>

---

# One command to connect a server

````bash
# Remote HTTP — the modern way
claude mcp add --transport http notion https://mcp.notion.com/mcp

# With auth header
claude mcp add --transport http secure-api https://api.co/mcp \
  --header "Authorization: Bearer $TOKEN"

# Local stdio — for servers that run as processes
claude mcp add --transport stdio my-db -- uvx some-mcp-server
````

<div v-click class="mt-4 text-sm opacity-80">

Check what's connected: <code>/mcp</code><br>
Scope: global, project (`.mcp.json`, commit it), or plugin-bundled.

</div>

<!--
Source: code.claude.com/docs/en/mcp
Notion MCP is a real, publicly available HTTP endpoint.
The /mcp command shows connected servers + lets you debug.
-->


---

# Demo A — Notion → design doc

<div class="mt-6 text-lg opacity-80">

**Setup:** PRD "User Profile API v2" in Notion with 3–4 requirements.

</div>

<div class="mt-4 p-4 bg-gray-800/40 rounded">

```
Read the "User Profile API v2" PRD in my Notion.
Then write a technical design doc as design.md that
implements it. Don't code yet.
```

</div>

<div class="mt-6 text-sm opacity-70">Narrate while it runs:</div>

<div class="mt-2 text-sm opacity-90">

- "Using Notion MCP to search for the page"
- "Reading the content"
- "Writing the design doc based on what it read"

</div>

<div v-click class="mt-6 text-center text-xl">

Notion → design → code. **No copy-paste.**

</div>

---

# Demo B — pencil.dev → React component

<div class="mt-6 text-lg opacity-80">

**Setup:** pencil file with a login screen or dashboard card.

</div>

<div class="mt-4 p-4 bg-gray-800/40 rounded">

```
Open the design in my pencil file and describe what you see.
Then generate a React component that matches it — TypeScript,
Tailwind, no external UI libraries.
```

</div>

<div v-click class="mt-6 text-center text-xl">

Designer hands me a file.<br>
Claude reads it directly. No eyeballing spacing.

</div>

<!--
FALLBACK: describe what the output would be — a TSX component with flex/grid layout, Tailwind classes matching the design's colors and spacing.
-->

---
layout: center
---

# If you use it daily and it has an MCP server — connect it.

<div class="mt-10 text-lg opacity-80">

Notion · GitHub · Slack · Postgres · Figma · pencil · Sentry · Linear · your browser

</div>

<div v-click class="mt-8 opacity-70">

Start with one. See if it sticks. Add more.

</div>

---
layout: section
---

# 4 · Brainstorm first, code later

<div class="opacity-60 text-lg mt-4">The pattern for greenfield — and for bugs</div>

---

# Greenfield — four steps

<div class="mt-6 grid grid-cols-1 gap-4">

<div v-click class="flex items-center gap-6">
  <div class="text-3xl font-bold opacity-30 leading-none w-8 text-right">1</div>
  <div>
    <div class="text-xl">Brainstorm in plain text</div>
    <div class="opacity-70">Empty dir. Describe the problem. Ask it to question <em>you</em>.</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-3xl font-bold opacity-30 leading-none w-8 text-right">2</div>
  <div>
    <div class="text-xl">Produce a design doc</div>
    <div class="opacity-70">Ask for <code>design.md</code>. Iterate on it.</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-3xl font-bold opacity-30 leading-none w-8 text-right">3</div>
  <div>
    <div class="text-xl">Get human review</div>
    <div class="opacity-70">Tech lead. Team. Revise.</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-3xl font-bold opacity-30 leading-none w-8 text-right">4</div>
  <div>
    <div class="text-xl">Then implement</div>
    <div class="opacity-70"><em>"Implement the design in design.md."</em></div>
  </div>
</div>

</div>

<div v-click class="mt-6 text-center opacity-80">

Steps 1–3 are cheap. Step 4+ is where bugs live.

</div>

---
layout: center
class: text-center
---

# Debug-by-dice-roll

<div v-click class="mt-8 text-xl opacity-80 max-w-3xl mx-auto text-left">

You hit a bug. Paste the stack trace. "Try again." Still broken. "Try again." Forty minutes later, five failed attempts in your scrollback, bug still there, no idea why.

</div>

<div v-click class="mt-10 text-xl max-w-3xl mx-auto">

Every "try again" is a **reroll with worse context.**

</div>

<div v-click class="mt-6 text-lg opacity-80 max-w-3xl mx-auto">

Claude can't see *why* the previous attempts failed — only that they did.

</div>

<!--
Pause after "Be honest — who's done this?" Almost everyone will nod.
This is the most relatable moment in the talk. Land it.
-->

---

# Bug fix — four steps (same pattern)

<div class="mt-6 grid grid-cols-1 gap-3">

<div v-click class="flex items-center gap-6">
  <div class="text-2xl font-bold opacity-30 leading-none w-8 text-right">1</div>
  <div>
    <div class="text-lg font-semibold">Write a <code>bug.md</code>, not a prompt</div>
    <div class="opacity-70 text-sm">Observed, expected, what you've tried, current hypothesis</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-2xl font-bold opacity-30 leading-none w-8 text-right">2</div>
  <div>
    <div class="text-lg font-semibold">Ask Claude to investigate, not fix</div>
    <div class="opacity-70 text-sm">"List 3–5 hypotheses. Rank by likelihood. Tell me what you'd check."</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-2xl font-bold opacity-30 leading-none w-8 text-right">3</div>
  <div>
    <div class="text-lg font-semibold">Confirm the diagnosis</div>
    <div class="opacity-70 text-sm">Add a log. Write a failing test. Reason through the code path.</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-2xl font-bold opacity-30 leading-none w-8 text-right">4</div>
  <div>
    <div class="text-lg font-semibold">Then fix</div>
    <div class="opacity-70 text-sm">Trivial, once diagnosis is right.</div>
  </div>
</div>

</div>

<!--
Real example to share:
Two weeks ago, race condition in [Ares/OpenClaw]. Three failed synchronization attempts — wasn't where it looked.
Wrote bug.md, asked for hypotheses. First hypothesis on the list was the actual cause.
10 minutes to diagnose. 2 minutes to fix.
-->

---

# One pattern. Two cases.

<div class="mt-6">

| Phase | Greenfield | Bug fix |
|---|---|---|
| **1. Brainstorm** | What are we building and why? | What's actually broken and why? |
| **2. Document** | `design.md` | `bug.md` with hypotheses |
| **3. Review** | Human sign-off | Agree on root cause |
| **4. Execute** | Implement the design | Write the fix |

</div>

<div v-click class="mt-10 text-center text-xl max-w-3xl mx-auto">

*"Write the document first.<br>
The code is the cheap part. The thinking is the expensive part."*

</div>

---
layout: section
---

# Closing

---
layout: center
class: text-center
---

# Three takeaways

<div class="mt-8 grid grid-cols-1 gap-6 max-w-2xl mx-auto text-left">

<div v-click class="flex items-center gap-6">
  <div class="text-4xl font-bold opacity-30 leading-none w-10 text-right">1</div>
  <div>
    <div class="text-2xl">Plan before you code</div>
    <div class="opacity-70 mt-1">On anything non-trivial, ask for a plan first.</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-4xl font-bold opacity-30 leading-none w-10 text-right">2</div>
  <div>
    <div class="text-2xl">Extend with MCP</div>
    <div class="opacity-70 mt-1">Notion, design, tickets, DB — all in the loop.</div>
  </div>
</div>

<div v-click class="flex items-center gap-6">
  <div class="text-4xl font-bold opacity-30 leading-none w-10 text-right">3</div>
  <div>
    <div class="text-2xl"><code>CLAUDE.md</code> is free leverage</div>
    <div class="opacity-70 mt-1">5 minutes now saves hours later.</div>
  </div>
</div>

</div>

---
layout: center
class: text-center
---

# This week

<div class="mt-10 text-xl max-w-2xl mx-auto text-left space-y-4">

<div v-click>✦ Install Claude Code</div>
<div v-click>✦ Run <code>/init</code> on a repo you already know</div>
<div v-click>✦ Try the brainstorm-first workflow on <strong>one real task</strong></div>

</div>

<div v-click class="mt-12 text-lg opacity-80">

Come find me after — I'd love to hear what broke and what worked.

</div>

---

# What I didn't cover

<div class="text-sm opacity-70 mt-2">For the curious — we skipped these today.</div>

<div class="mt-6 grid grid-cols-2 gap-6 text-base">

<div>

- **Sub-agents & parallel execution**
- **Auto mode** — reduced approvals
- **Hooks** — scripts on events

</div>

<div>

- **Routines / scheduled tasks**
- **Remote Control** — phone-driven
- **SDK / headless mode**

</div>

</div>

<div class="mt-12 text-center text-lg">

code.claude.com/docs · weekly digest

</div>

---
layout: center
class: text-center
---

# Questions?

<div class="mt-10 text-lg opacity-70 max-w-xl mx-auto">

Duy Bui · 21/04/2026

</div>

<!--
ANTICIPATED QUESTIONS — your cheat sheet:

1. "Is it safe to let it run commands?"
   - Reviewing every call is not a workflow people stick to.
   - Three options: default (high friction), allowlist (where most should live), auto mode (tight loops).
   - Principle: scope the blast radius, don't review the blast.
   - YOLO mode exists (--dangerously-skip-permissions), don't recommend outside sandbox.

2. "What if it makes a mistake?"
   - It will. Review the plan, review the diff, run the tests.
   - Speed-up comes from volume, not from skipping review.

3. "Can I use it with [our internal tool]?"
   - If there's an MCP server, yes. If not, write one — they're just small programs exposing tools.
-->
