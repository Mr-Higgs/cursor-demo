# Cursor × Deep Agents Speedrun

**4-hour hackathon — 6:00pm to 10:00pm**
**~150 attendees | Teams of 4–5 | Prizes, cash, and credits**

---

## The Challenge

You will receive a product brief at kickoff. In **2.5 hours**, build a working AI agent using **Cursor** and the **Deep Agents framework** — then deploy it publicly so anyone can use it.

At 9:00pm, an automated eval harness fires 10 tasks at your agent. Highest score wins.

---

## Product Brief

*(Revealed at kickoff — 6:15pm)*

---

## What You Must Build

**4 required deliverables:**

1. **Filled PRD** (`system-design-prd-template.md`) — fill this out before you write code. Architecture thinking first.
2. **Agent** — built with Deep Agents (`create_deep_agent`) + Cursor as your editor
3. **Hosted public URL** — your agent deployed with anonymous auth; anyone opens the URL in a browser, no login required
4. **`.cursorrules` file** — shows how you used Cursor during the build

**Submit your public URL before 9:00pm.**

---

## Rules

- Max teams of **4–5 people** Min of **2 people**
- **Cursor** must be your primary editor
- Agent runtime must use **Deep Agents** (`create_deep_agent` or a LangGraph subgraph as a sub-agent)
- Must deploy via `deepagents deploy` to LangGraph Platform
- **Anonymous auth only** — no login walls, no API keys required from users
- One public URL submitted per team before the deadline

---

## Judging

### Automated Gauntlet — 10 points

At 9:00pm, the eval harness automatically fires 10 tasks at your deployed agent URL. Each task passes or fails. **No subjectivity — your agent works or it doesn't.**

Tasks range from basic (respond coherently) to complex (complete a full 5-step chain in one session). Details revealed at kickoff.

### UI/UX Bonus — up to +2 points

Roving judges circulate **8:00–9:00pm** and score custom frontends:

- **+0** — Default Deep Agents chat UI
- **+1** — Custom UI, functional but basic
- **+2** — Polished; feels like a real product (animations, branding, great UX copy, streaming)

Any frontend stack works: Next.js, Svelte, plain HTML, whatever you want.

### Stage Demo — top 5 only

If you're in the top 5, you get **3 minutes on stage** at 9:05pm. Show it live, explain your architecture, make it memorable.

---

## Scores & Tie-Breaking

**Final score = Gauntlet (0–10) + UI/UX bonus (0–2)**

Tie-breaker: **earliest submission timestamp wins.**

---

## Timeline

| Time | What's happening |
|------|-----------------|
| 6:00–6:15 | Opening demo — Cursor tips, architecting from scratch, Deep Agents |
| 6:15–6:30 | Team up, brief revealed, starter kit walkthrough |
| 6:30–9:00 | Build time |
| 8:00–9:00 | Roving judges scoring UI/UX bonuses |
| 8:55 | 5-minute warning |
| **9:00** | **Submission deadline** |
| 9:05 | Leaderboard posted — top 5 announced |
| 9:05–9:35 | Top 5 demos on stage (3 min each) |
| 9:35–10:00 | Prize ceremony |

---

## Prizes

| | Prize |
|--|-------|
| 1st place | Cash + credits |
| 2nd place | Cash + credits |
| 3rd place | Credits |
| Best Architecture | Bonus credits — judges pick during build hours |
| Best UI/UX | Bonus credits — most polished custom frontend |
| Best .cursorrules | Bonus credits — sharpest Cursor rules file |

---

## Starter Kit

When you sit down, you have access to:

- **`deepagents.toml`** — pre-configured with anonymous auth and frontend enabled
- **`agent.py`** — bare skeleton with `create_deep_agent`, nothing else
- **Example filled PRD** — reference only, write your own
- **Sample `.cursorrules`** — fork it, override it, or ignore it
- **Submission form** — URL + team name, timestamp auto-captured

---

## Quick Start

There are 3 paths. Pick one — all can deploy to a public URL.

---

### Path A — CLI (recommended for most teams)

The CLI scaffolds your project, runs a local dev server, and deploys to LangGraph Platform in one command. The hosted chat UI is included for free.

```bash
# 1. Install
uv tool install deepagents-cli

# 2. Scaffold
deepagents init my-agent && cd my-agent

# 3. Build in Cursor — edit agent.py (add tools, sub-agents, system prompt)
#    and deepagents.toml (model, skills, auth)

# 4. Preview locally at http://localhost:2024
deepagents dev

# 5. Deploy (do this early — takes ~2 min, gets you a public URL)
deepagents deploy
# → submit this URL

# 6. Submit your URL at [submission form link]
```

The starter `deepagents.toml` already has anonymous auth and the frontend enabled — anyone can open your URL in a browser with no login.

---

### Path B — Python SDK directly

Use this if you want full control over the graph, want to build a custom frontend, or don't want the CLI wrapper.

```bash
# 1. Install the SDK
uv add deepagents langchain-anthropic langgraph-cli
```

```python
# agent.py — minimum working agent
from deepagents import create_deep_agent
from langchain_anthropic import ChatAnthropic
from langgraph.checkpoint.memory import MemorySaver

agent = create_deep_agent(
    model=ChatAnthropic(model="claude-sonnet-4-6"),
    checkpointer=MemorySaver(),
    system_prompt="You are an orchestrating agent.",
)
```

```bash
# 2. Run locally with LangGraph dev server
langgraph dev
# → http://localhost:2024 (built-in chat UI)

# 3. Deploy to LangGraph Platform
langgraph deploy
# → submit your public URL
```

**When to use Path B:**
- You're building a custom Next.js / React frontend on top of the streaming API
- You need a custom LangGraph subgraph as a sub-agent
- You want to wire up your own tools without the `deepagents.toml` config format

---

### Path C — TypeScript / Node.js SDK

Use this if your team prefers TypeScript or wants to build a full-stack Next.js app with the agent baked in.

```bash
# 1. Install
npm install deepagents langchain @langchain/core
# or: pnpm add deepagents langchain @langchain/core
```

```ts
// agent.ts — minimum working agent
import { createDeepAgent } from "deepagents";
import { tool } from "langchain";
import * as z from "zod";

const agent = createDeepAgent({
  systemPrompt: "You are an orchestrating agent.",
  tools: [], // add your custom tools here
});

// Invoke
await agent.invoke({
  messages: [{ role: "user", content: "Help me research..." }],
});
```

```bash
# 2. Run locally with LangGraph dev server
npx @langchain/langgraph-cli dev
# → http://localhost:2024

# 3. Deploy to LangGraph Platform
npx @langchain/langgraph-cli deploy
# → submit your public URL
```

**When to use Path C:**
- Your team is more comfortable in TypeScript than Python
- You're building a Next.js frontend and want the agent in the same codebase
- You want end-to-end type safety on tool inputs/outputs (Zod schemas)

---

**Tip:** Deploy early and often. A working deploy at 7:30pm beats a crashed one at 8:59pm.

---

## Resources

- [Deep Agents docs](https://docs.langchain.com/oss/python/deepagents/overview)
- [Deep Agents GitHub](https://github.com/langchain-ai/deepagents)
- [LangGraph Platform docs](https://langchain-ai.github.io/langgraph/concepts/langgraph_platform/)
- `system-design-prd-template.md` — in this repo
- Starter `agent.py` — in this repo
