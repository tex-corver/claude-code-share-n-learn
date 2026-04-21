# Claude Code — Share & Learn

**Duration:** 25 min talk + 5 min Q&A
**Audience:** Mixed — some AI users, some not
**Core message:** *The code Claude Code writes is the least interesting thing it does. The leverage is in planning, extending, and thinking — before any code is written.*

**What attendees should walk away able to do:**
1. Use the plan-first workflow for non-trivial tasks
2. Understand and use built-in slash commands & plugins
3. Connect an MCP server (Notion, pencil) to extend Claude Code's reach
4. Use Claude Code as a brainstorming / design-doc partner *before* writing code

---

## 0 · Opening hook (2 min)

**Goal:** Reframe what the audience is already doing. They've used Claude Code — the hook has to tell them something they haven't heard.

### What to say

> "I've been using Claude Code daily for about 5 months now. And I'll be honest — for the first couple of months, I was using it wrong."
>
> "My loop was: open a ticket, paste the requirements, say 'implement this,' watch it go. Then spend half my time cleaning up. When it worked, it felt amazing. When it didn't, I'd blame the model, or the prompt, and try again. I thought that's just how AI coding worked."
>
> *(pause)*
>
> "Then I changed one thing. I stopped asking it to write code first. I started asking it to write a design doc first. My rework dropped. My reviews got easier — because I wasn't reviewing code, I was reviewing **thinking**. And once I saw that pattern, I realized the code Claude Code writes is actually the least interesting thing it does."
>
> "So today I'm not going to teach you what Claude Code is — you already know. I'm going to share three shifts that changed how I use it every day: **planning before coding, extending it with MCP, and using it as a thinking partner — not a typing partner.**"

### Delivery notes

- **Don't rush the pause.** The "I was using it wrong" line needs a beat to land. Audiences lean in when you admit you were wrong.
- **Drop the exact number** — "about 5 months" sounds more honest than "daily for 5 months." Mild understatement reads as confident.
- **Don't oversell the payoff.** Avoid "this changed everything" — your audience will tune out. "My rework dropped" is specific and believable.
- **Land the three shifts clearly** — that's your table of contents. Say them slowly.

### One slide

**Three shifts that changed how I use Claude Code**

1. Plan before you code
2. Extend it with MCP
3. Thinking partner, not typing partner

---

## 1 · Core concepts (7–8 min)

**Goal:** Four mental models. Not a feature tour. Section 1.1 includes a 60-second live demo of `/context`.

### 1.1 Context is everything

> **Reference:** [code.claude.com/docs/en/context-window](https://code.claude.com/docs/en/context-window) — open this slide if you want to show the official interactive timeline visualization. It's worth pulling up live for 30 seconds.

**The mental model to deliver:**

Claude's context window holds *everything* it knows about your session. Most people think of it as just "the chat so far." It's actually much more:

**Loaded before you type anything:**
- `CLAUDE.md` (project memory)
- Auto memory files
- MCP tool names (every connected server costs tokens, every turn)
- Skill descriptions
- System prompt / output styles

**Added as Claude works:**
- Every file it reads
- Path-scoped rules that auto-load with matching files
- Hook outputs that fire after each edit
- Its own responses

**Why this matters:** context is your primary constraint. Not intelligence, not speed — context. When sessions get slow, expensive, or start "forgetting," 9 times out of 10 it's context pressure.

**Three commands every attendee should know — run these live:**

| Command | What it does | When to use |
|---|---|---|
| `/context` | Shows live context usage broken down by category, with optimization suggestions | When the session feels slow or expensive |
| `/memory` | Shows which `CLAUDE.md` and auto-memory files loaded at startup | When Claude seems to be "forgetting" your conventions |
| `/compact` | Replaces the conversation history with a structured summary, freeing context | When you've been in a long session and want to keep going |

**Live demo beat (60 seconds):**

1. Run `/context` in a session you've had open for a while. Show the breakdown — MCP tools, CLAUDE.md, conversation history, etc.
2. Point at the biggest eater. Usually it's either the conversation itself or MCP tools from servers you're not actively using.
3. Say: *"This is why your sessions get slow. It's not the model — it's the context you're feeding it."*

**On `CLAUDE.md` specifically:**
- Every session re-reads it. It's free leverage, and it's also a free tax if you write it badly.
- Keep it tight — 50–150 lines. What's the stack? What are the conventions? What are the "don't do this" rules?
- Avoid: aspirations, tutorials, things Claude can infer from the repo anyway. Those are just tokens you pay for every turn.
- Use `/init` to generate a starter, then edit ruthlessly.

**Show on screen:** a minimal CLAUDE.md — 15 lines — covering:
- Stack (Python 3.11, FastAPI, Polars)
- Conventions (Clean Architecture layout, where domain lives, where infra lives)
- "Always use `uv` for dependency management, never pip directly"
- "Tests go in `tests/`, mirror the source structure"

**Also worth mentioning (one sentence each):**
- **Subagents** run in their *own* separate context window, so you can delegate big research tasks without polluting your main session. Only the summary comes back. ([docs](https://code.claude.com/docs/en/sub-agents))
- **Path-specific rules** live alongside files and auto-load when Claude touches that path — great for "when editing this directory, follow these conventions." ([docs](https://code.claude.com/docs/en/memory#path-specific-rules))

**Two one-liners to land:**
- *"Context is your primary constraint. Manage it like you'd manage memory in a tight loop."*
- *"`/context`, `/memory`, `/compact` — learn these three commands this week."*

### 1.2 Plan mode vs. act mode

**This is the concept most people miss.**

**Talking points:**
- By default, Claude Code wants to act. It will read, edit, run.
- But for anything non-trivial, you want it to **plan first**. Why?
  - Cheaper to fix a bad plan than bad code
  - Forces you to actually think about what you want
  - Turns the AI from "code generator" into "pair programmer"
- How: `/plan` mode, or just say "before you do anything, make a plan and wait for my approval."

**One-liner:** *"If the task is bigger than 10 lines of code, ask for a plan first."*

### 1.3 Sessions, resume, recap

**Talking points:**
- Each project directory gets its own session history.
- `/resume` — pick up where you left off.
- `/recap` — Claude writes you a summary of what's happened so far.
- Translation: you can close your laptop, come back tomorrow, and continue.

### 1.4 Permissions — it asks before it destroys

**Talking points:**
- Claude Code asks before running commands that change things (writing files, running bash, making network calls).
- You can allowlist patterns you trust (e.g., `pytest`, `ls`, `git status`).
- Settings live in `.claude/settings.json` — commit the team-wide ones, keep personal ones local.
- There's an "auto mode" that batches low-stakes decisions — mention it exists, don't demo it today.

**One-liner:** *"You're always in the loop. It asks. You approve."*

---

## 2 · Live demo — plan-first workflow (6–7 min)

**Goal:** Make the "planning first" principle click by showing it.

**Demo task:** *"Outline this very talk, from scratch."*

Why this task: it's the exact prompt I used to produce `claude-code-talk.md`. **I've already run it.** No live surprises, and when Section 4's meta-example slide lands, the audience will recognize the artifact they watched being built. Fortune favors demos you've already done.

### Demo flow

**Step 1 — Start in an empty directory**

```bash
mkdir talk-outline && cd talk-outline
claude
```

**Say out loud:** *"No slides yet. No repo. I'm using Claude Code as a thinking partner — same way I used it to prep this talk."*

**Step 2 — Ask it to interview you before writing**

> "I need to prep a 25-minute talk introducing Claude Code to my engineering team. Before you write anything, ask me 5–7 clarifying questions about audience, takeaways, format, and constraints. Then wait for my answers."

**Why this matters to narrate:** "Notice I didn't just say 'write me a talk outline.' I asked it to *interview me*. This is the move. The outline it produces after clarification is 10x better than a one-shot outline."

**Step 3 — Answer the questions briefly**

Have answers pre-prepared so you don't stall live:
- **Audience:** ~30 engineers, mixed — some daily Claude Code users, some haven't touched it
- **Duration:** 25 min + 5 min Q&A
- **Core message:** planning-first beats code-first; the code is the cheap part
- **Format preference:** live demos over bulleted slides
- **Constraint:** I want one demo that doubles as a meta-example (the talk's own build process)

**Step 4 — Ask for the outline**

> "Good. Now write `outline.md`. Include: section breakdown with timing, the opening hook, one-liners worth rehearsing, demo hooks, Q&A prep."

**Step 5 — Review the outline live**

Open `outline.md`. Skim it with the audience. Better yet, open `claude-code-talk.md` side-by-side. Point out:
- It picked up on the "mixed audience" constraint and hedges its explanations accordingly
- It structured time budget per section (real `claude-code-talk.md` has the same table)
- It flagged open questions at the bottom — perfect, that's what you want

**Step 6 — Iterate**

> "Good. But Section 4 should cover bug fixes too, not just greenfield work. Add an anti-pattern for debug-by-dice-roll and a 4-step corrective pattern."

**Teaching moment to verbalize during the demo:**

> "Look at what just happened. In 5 minutes I have an outline that would've taken me an hour to draft. But more importantly — **I didn't write any slides yet.** I'd now share this with a colleague, revise, and *then* let the Slidev skill scaffold the deck from it. That's the workflow."
>
> *(optional punchline)* "And here's the kicker — that's literally how the script for this talk got written. You just watched me speedrun my own prep. The meta-example slide at the end is going to show you the exact same artifacts we just produced."

### Fallback if demo breaks

Have a screenshot of a pre-generated `outline.md` ready, or just open `claude-code-talk.md` directly. Say: "network's being weird, here's what it produced in rehearsal" — keep moving. The core message lands either way.

---

## 3 · Extending Claude Code (10–11 min)

**Goal:** This is the part your audience will remember. Plugins and MCP = superpowers.

### 3.0 The extension landscape — 90 second overview

> **Reference:** [code.claude.com/docs/en/features-overview](https://code.claude.com/docs/en/features-overview) — open this page live. It's the single best "map" of how to extend Claude Code, and it's where you'll want attendees to go after the talk.

**Purpose of this beat:** give attendees the full map in 90 seconds so they know (a) there's a bigger surface area than just MCP, and (b) where the things we're *not* covering today live.

**What to say:**

> "Before we go deep on two things, I want to show you the full picture. Claude Code has six extension points. They're all on this one page."

**Show the features-overview page on screen.** Walk the top table in 60 seconds:

| Feature | One-line description |
|---|---|
| **CLAUDE.md** | Always-on context (we covered this) |
| **Skills** | Reusable knowledge + invocable workflows (`/deploy`, `/review`) |
| **MCP** | Connects Claude to external services (we'll demo this) |
| **Subagents** | Isolated context windows for side-quests |
| **Hooks** | Deterministic scripts that fire on events (lint-on-edit, etc.) |
| **Plugins** | The packaging layer — bundle all of the above to share |

**The one insight worth highlighting from this page — the "when to add what" trigger table:**

> "This is my favorite thing about this page. You don't add these features up front. Each one has a trigger. Here are three you'll hit:"

| If this happens... | Add... |
|---|---|
| Claude gets the same convention wrong twice | A line in `CLAUDE.md` |
| You paste the same multi-step prompt 3 times | A skill |
| You keep copying data from a browser tab Claude can't see | An MCP server |

**Land the transition:**

> "Today we're going to dive into the two with the highest leverage for most of us right now: **skills/slash commands**, and **MCP**. Hooks, subagents, plugins — if you want to go deeper on those, this page is your starting point."

Now move into 3.1.

### 3.1 Slash commands & skills (3 min)

**Talking points:**

Claude Code ships with useful built-ins. Don't list all of them — pick 4:

| Command | What it does | When to use |
|---|---|---|
| `/init` | Generates a starter `CLAUDE.md` by reading your repo | First time in any codebase |
| `/review` | Reviews your current changes like a senior dev | Before committing |
| `/recap` | Summarizes the current session | Returning after a break |
| `/cost` | Shows tokens used + cost | Debugging "why is this slow/expensive" |

**Say:** "Try `/init` today. Open a repo you know well, run it, see what it produces. You'll almost always want to edit the output — but it's a great starting point."

**Custom skills (briefly):**

> "Remember the trigger from a minute ago — 'you paste the same prompt three times'? That's when you write a skill. A skill is just a markdown file in `.claude/commands/`. Claude will invoke it as a slash command."

Show a tiny example — a `write-adr.md` command:

```markdown
---
description: Write an Architecture Decision Record
---

Write an ADR for the following decision. Use the standard format:
- Context
- Decision
- Consequences
- Alternatives considered

Decision topic: $ARGUMENTS
```

Usage: `/write-adr use Polars instead of Pandas for the transform layer`

**One-liner:** *"Anything you type into Claude more than twice should become a skill."*

**Plugins (one sentence):** Plugins are the packaging layer — they bundle skills, hooks, subagents, and MCP servers so you can share a whole setup across repos or teammates. There's a marketplace. Deep-dive another time.

### 3.2 MCP — connecting Claude to your world (7 min)

**Frame it clearly first:**

> "MCP — Model Context Protocol — is how you let Claude Code use tools that aren't your file system. Your Notion. Your Figma. Your Jira. Your database. If it has an MCP server, Claude can use it."
>
> "Think of MCP servers as plugins that teach Claude new verbs. Without MCP, Claude can read files and run commands. With MCP, Claude can also read your Notion docs, query your database, or modify a design file."

**Show the config:**

Claude Code MCP config is a JSON file. Show a minimal one:

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "NOTION_API_KEY": "ntn_..."
      }
    }
  }
}
```

**Teaching moment:** "That's it. Three lines of JSON and Claude Code can now read and write your Notion."

#### Live demo A — Notion (3 min)

**Setup:** Have a real Notion page ready. Suggestion: a fake PRD titled "User Profile API v2" with 3–4 bullet requirements.

**Demo flow:**

```
You: "Read the 'User Profile API v2' PRD in my Notion. Then write a
     technical design doc as design.md that implements it. Don't code yet."
```

**Narrate while it runs:**
- "It's using the Notion MCP tool to search for the page"
- "Now it's reading the content"
- "And now it's writing the design doc based on what it read"

**Pay off the planning theme:** "This is the full loop. Product writes a PRD in Notion. I ask Claude to read it, plan the implementation, write a design doc. I review. Then — and only then — I ask it to code. Notion → design → code. No copy-paste."

#### Live demo B — pencil.dev (2-3 min)

**Setup:** Have a pencil file ready with a simple UI mock — a login screen or a dashboard card.

**Why this demo lands:** It's visual. The audience sees a design, sees code come out, and the lightbulb goes on.

**Demo flow:**

```
You: "Open the design in my pencil file and describe what you see.
     Then generate a React component that matches it — TypeScript,
     Tailwind, no external UI libraries."
```

**Narrate:**
- "It's using the pencil MCP to read my design file"
- "It's describing the layout — notice it got the button styles and spacing"
- "Now it's producing the component"

**Teaching moment:** "Designer hands me a pencil file. I don't eyeball it and guess spacing. Claude reads the file directly and gives me a first draft. I review, tweak, ship."

#### Wrap MCP section

**One-liner:** *"If you use it daily and it has an MCP server, connect it. Notion, GitHub, Slack, your database, Figma, pencil — the list keeps growing."*

**Mention but don't demo:** "There are MCP servers for almost everything. GitHub, Linear, Sentry, Postgres, even your browser. Start with one, see if it sticks, then add more."

---

## 4 · The "brainstorm first, code later" pattern (3–4 min)

**Goal:** Reinforce the planning-first theme one more time and extend it to the case your audience hits most often — bug fixing.

### 4.1 The pattern for greenfield work

"Here's the workflow I want you to try this week. Four steps:"

1. **Brainstorm in plain text.** Open Claude Code in an empty directory. Describe the problem. Ask it to question you.
2. **Produce a design doc.** Ask for `design.md`. Iterate on it.
3. **Get human review.** Share the doc with your tech lead, your team. Revise.
4. **Then implement.** Point Claude at the approved doc. "Implement the design in `design.md`."

**Why this works:**
- Steps 1–3 are cheap. Steps 4+ are where bugs live.
- You catch architectural mistakes *before* they're in code.
- You get a durable artifact (the doc) that lives longer than any chat session.

**Meta-example — this talk:**

> "The thing you're watching right now was built with this exact loop. Four artifacts, in this order:"

| Step | Artifact | What happened |
|---|---|---|
| 1. Brainstorm | *(conversation)* | Claude interviewed me: audience, duration, what I wanted them to walk away able to do. No file written yet. |
| 2. Document | `claude-code-talk.md` | The full speaker script — timing, one-liners, demo flow, Q&A prep. **This** is the design doc. Not the slides. |
| 3. Review | *(me, offline)* | I read it, cut two sections, rewrote the opening hook. Revised the doc, not the slides — still no slides at this point. |
| 4. Execute — slides | `slides.md` | Invoked the Slidev skill: *"turn `claude-code-talk.md` into a Slidev deck."* It scaffolded the whole thing from the doc. |
| 4. Execute — deploy | `docs/superpowers/plans/2026-04-21-deploy-slidev.md` → Dockerfile, nginx, CI workflows | Separate sub-plan for "ship it to `claude-share.urieljsc.com`." Six tasks, each with verification steps. Executed task-by-task. |

**The payoff to call out:**

> "Notice the doc came *first* and the slides came *last*. If I'd opened Slidev and started typing slides, I'd have spent the whole time fighting layouts instead of fighting the argument. And when I wanted to change the opening — which I did, twice — I edited one markdown file, not 30 slides."
>
> "Same pattern for the deploy. I didn't let Claude just 'figure out how to ship this.' I made it write a plan with six concrete tasks and verification commands, I reviewed it, *then* it executed. The prod deploy worked first try because the thinking had already been done."

### 4.2 The pattern for bug fixing — the part most people get wrong

**The anti-pattern to call out first:**

> "Be honest — who's done this? You hit a bug. You paste the stack trace to Claude. It suggests a fix. Doesn't work. You say 'didn't work, try again.' It tries something else. Still broken. 'Try again.' Forty minutes later, you have five failed attempts in your scrollback, the bug is still there, and you have no idea why."

*(pause — almost everyone will nod)*

> "I call this **debug-by-dice-roll**. Every 'try again' is a reroll with slightly worse context. Each attempt pollutes the next. Claude can't see *why* the previous attempts failed, only that they did. Eventually you're in a thread where Claude is making random changes hoping something sticks."

**The fix: brainstorm the bug itself before asking for a fix.**

The principle is exactly the same as greenfield work — **understand the problem in writing before you touch the code.** For bugs, that means:

1. **Write a bug report first, not a prompt.** Not "fix this error" — but a `bug.md` that captures:
   - What's the observed behavior? (with exact error, logs, reproduction steps)
   - What's the expected behavior?
   - What have you already tried?
   - What's your current hypothesis about the cause?

2. **Ask Claude to investigate, not fix.** Start with:
   > *"Here's the bug in `bug.md`. Don't propose a fix yet. Read the relevant code, list 3–5 hypotheses for what could be causing this, and rank them by likelihood. For the top hypothesis, tell me what you'd check to confirm it."*

3. **Confirm the diagnosis before fixing.** Once Claude identifies the root cause, ask it to verify — add a log statement, write a failing test that reproduces the bug, or just reason through the code path with you.

4. **Then fix.** Only once you both agree on the cause. The fix itself is usually trivial once the diagnosis is right.

**Why this works:**
- You separate **diagnosis** from **remediation**. Most failed bug-fix loops conflate them.
- The bug report gives Claude a stable artifact to refer back to. Scrollback gets polluted; `bug.md` doesn't.
- If the first hypothesis is wrong, you revise `bug.md` and continue — you don't start over.
- The failing test you write during diagnosis becomes the regression test after the fix.

**Concrete example to share:**

> "Two weeks ago I had a race condition in [Ares/OpenClaw]. First instinct: paste the error, 'fix it.' Claude tried three different synchronization approaches. None worked — because the race wasn't where it looked like it was."
>
> "I stopped, wrote a `bug.md` describing the symptom and my three failed attempts, and asked Claude to list hypotheses instead of fixing. First hypothesis on the list was the actual cause — something I'd already ruled out but hadn't verified properly. 10 minutes to diagnose, 2 minutes to fix."

*(Swap this for a real example of your own — the talk needs this to feel true, not illustrative.)*

### 4.3 The unified principle

**One slide to land both cases:**

| Phase | Greenfield | Bug fix |
|---|---|---|
| **1. Brainstorm** | What are we building and why? | What's actually broken and why? |
| **2. Document** | `design.md` | `bug.md` with hypotheses |
| **3. Review** | Human sign-off | Agree on root cause |
| **4. Execute** | Implement the design | Write the fix |

**One-liner:** *"Whether you're building something new or fixing something old — write the document first. The code is the cheap part. The thinking is the expensive part."*

---

## 5 · Closing (2 min)

### Three takeaways — final slide

1. **Plan before you code.** On anything non-trivial, ask for a plan first.
2. **Extend with MCP.** Your tools — Notion, design, tickets, DB — can all be in the loop.
3. **`CLAUDE.md` is free leverage.** 5 minutes now saves hours later.

### Call to action

> "This week: install Claude Code, run `/init` on a repo you already know, and try the brainstorm-first workflow on one real task. Come find me after — I'd love to hear what broke and what worked."

### What I didn't cover — mention briefly

For the curious, here's what exists but we skipped today:
- **Sub-agents & parallel execution** — run multiple Claudes in parallel
- **Auto mode** — reduces approval prompts for low-stakes actions
- **Hooks** — run scripts on events (pre-commit, post-tool-use)
- **Routines / scheduled tasks** — Claude runs on a cron, even when your laptop's off
- **Remote Control** — drive sessions from your phone
- **SDK / headless mode** — scripting Claude Code

Point to `code.claude.com/docs` and the weekly digest.

---

## Q&A prep — likely questions

**"Is it safe to let it run commands?"**

> The honest answer: reviewing every tool call is not a workflow people actually stick to. Five minutes in, you're hitting 'approve' on autopilot anyway — which is worse than not reviewing, because you've convinced yourself you *are* reviewing.
>
> You've got three realistic options:
>
> 1. **Default permission mode.** Claude asks before anything that changes state. Best for unfamiliar tasks or first-time-in-a-repo work. High friction, high safety.
> 2. **Allowlist the safe stuff in `.claude/settings.json`.** Add patterns you trust — `pytest`, `ls`, `git status`, `rg`, read-only database queries. Now you only get interrupted for the things that actually matter. This is where most people should live.
> 3. **Auto mode.** Claude batches low-stakes decisions and only pauses for high-stakes ones (deletes, network calls, pushes). It also logs every decision so you can audit after. Best for tight agentic loops where prompt-by-prompt approval kills your flow. Available for Max subscribers on recent Opus models.
>
> There's also `--dangerously-skip-permissions` (aka "YOLO mode"). I don't recommend it outside a sandbox or a throwaway VM, but it exists. The name is honest about what it is.
>
> The real principle: **scope the blast radius, don't review the blast.** Don't run it as root. Don't give it production credentials. Run risky stuff in a container or a git worktree you can nuke. Then let auto mode or a generous allowlist do the work. You'll ship faster *and* be safer than the person clicking 'approve' every 30 seconds without reading.

**"What if it makes a mistake?"**

> It will. You review the plan, review the diff, run the tests. Same loop as code review. The speed-up comes from volume of work, not from skipping review.

**"Can I use it with [our internal tool]?"**

> If there's an MCP server for it, yes. If not, you can write one — MCP servers are just small programs that expose tools. That's a deeper talk.

---

## Speaker notes & timing cheat sheet

| Section | Time | Cumulative | Checkpoint |
|---|---|---|---|
| Opening hook | 2 min | 2 | Audience is leaning in, not on phones |
| Core concepts | 8 min | 10 | Everyone knows `/context`, `/memory`, `/compact` and the plan-first mindset |
| Live demo: design doc | 7 min | 17 | At least one "ohhh" moment in the room |
| 3.0 Extension landscape overview | 1.5 min | 18.5 | Audience has seen the full map and the trigger table |
| 3.1 Slash commands & skills | 3 min | 21.5 | They've seen `/init` and one custom skill |
| 3.2 MCP demos | 6 min | 27.5 | Both MCP demos worked OR fallback shown |
| Section 4: brainstorm-first (greenfield + bug) | 4 min | 31.5 | The debug-by-dice-roll anti-pattern landed |
| Closing | 2 min | 33.5 | Three takeaways on screen, CTA delivered |
| Q&A | 5 min | 38.5 | — |

**Buffer plan:**
- If running long: cut the pencil demo (keep Notion), or skip the `/context` live demo (just describe it), or compress 4.1 to one sentence since the design-doc demo already illustrated it.
- If running very long: cut Section 3.0 and just link to the features-overview page in the closing slide.
- If running short: expand Q&A.

**Realistic read:** the talk is now budgeted at ~33 min of content + 5 min Q&A. If you want to hit 25–30 min total, cut Section 3.0 (saves 1.5 min) AND compress Section 4.1 to one slide of bullets (saves ~2 min) — the design-doc demo already did the heavy lifting for greenfield. Section 4.2 (bug-fixing) is the part worth the full treatment since no demo covered it.

**Demo prep the night before:**
- Pre-create the Notion PRD page
- Pre-create the pencil file with a simple mock
- Have `design.md` from a rehearsal saved as fallback screenshot
- Test both MCP connections end-to-end
- Clear your terminal history — audiences notice
- Increase terminal font size to 18–20pt
