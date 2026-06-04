# Hackathon Organizer Runbook — Internal

## Event Overview

- **Format:** "Cursor × Deep Agents Speedrun"
- **Demo/Presentation:** 4:00–6:00pm (separate block before hackathon)
- **Hackathon:** 6:00–10:00pm (4 hours)
- **Attendees:** ~150 (self-organize into teams of 4–5, ~30 teams)
- **Judging:** Automated eval harness — top 5 advance to stage demos

---

## Run-of-Show

### Pre-Hackathon: Demo Block

| Time | Activity | Owner |
|------|----------|-------|
| 4:00–6:00pm | Demo/presentation: Cursor tips, architecting from scratch (PRD template), Deep Agents | Jerry |

### Hackathon Block

| Time | Activity | Owner |
|------|----------|-------|
| 6:00–6:15 | Team formation, product brief reveal, starter repo walkthrough | Jerry or Ben? |
| 6:15–9:00 | Build time (~2h45min). Roving judges circulate at 8:00 for UI bonus scoring. | Judges |
| 8:55 | 5-min warning announcement | Ben |
| 9:00 | Submission deadline. Eval harness runs automatically. | Eval system |
| 9:05 | Top 5 scores posted to leaderboard. Teams notified. | Ben |
| 9:05–9:35 | Top 5 lightning demos — 3 min each on stage | Top teams |
| 9:35–10:00 | Prize ceremony + credits distribution | Ben or Jerry or Sam? |

---

## Product Brief (Revealed at 6:15) **we can always revise this before we publish details**

> *"Build an agent that helps a startup founder go from a raw idea to a validated 1-week sprint plan — including user research tasks, an MVP scope, and a prioritized risk list."* 

Keep this off slides until kickoff. Build the energy around the reveal.

---

## Automated Gauntlet Eval Suite (10 Tasks)

The eval harness fires these tasks against each team's deployed agent URL at 9:00pm sharp. Pass/fail automated. No judges needed.

| # | Task Input | Pass Condition | Points |
|---|------------|----------------|--------|
| 1 | "What can you help me with?" | Coherent, relevant response | 1 |
| 2 | "Read `brief.txt` and summarize it" (file injected) | Uses file tool, returns accurate summary | 1 |
| 3 | "Break this idea into 3 user research tasks" | Returns exactly 3 actionable tasks | 1 |
| 4 | "Define the MVP scope in exactly 5 bullets" | Returns structured list of 5 items | 1 |
| 5 | "What's the biggest risk in this plan?" | Returns a specific, reasoned risk | 1 |
| 6 | "Remember: our target user is a solo founder" → follow-up "Who is our target user?" | Returns "solo founder" | 1 |
| 7 | "Delegate the risk analysis to a sub-agent and report back" | Evidence of sub-agent delegation in response | 1 |
| 8 | Empty string input `""` | Does not crash — returns clarifying question | 1 |
| 9 | "Write a sprint plan to a file called `sprint.md`" | `sprint.md` created and has content | 1 |
| 10 | Full chain: idea → research tasks → MVP scope → risk list → `sprint.md` written | All steps complete in one session | 1 |

**Scoring:** 1 pt per task. +2 bonus for custom UI (judge scored). Tie-breaker: earliest submission timestamp.
**Top 5 total scores advance to stage.**

---

## Hosting Path (What Teams Do)

```bash
deepagents init my-agent && cd my-agent
# In deepagents.toml:
#   [frontend]
#   enabled = true
#   [auth]
#   provider = "anonymous"
deepagents deploy
# → submit public URL to leaderboard form
```

Anonymous auth means anyone with the URL can interact with the agent — no API keys, no login walls. This is required.

---

## Judge Roles

| Role | Count | When Active | Responsibility |
|------|-------|-------------|----------------|
| Eval judges | 2 | 9:00–9:10pm | Monitor harness, handle submission errors, post leaderboard |
| Roving judges | 2 | 8:00–9:00pm | Circulate, score UI/UX bonus (+2 pts max per team) — rubric: visual design, streaming UX, product feel |
| Stage judges | 3–4 | 9:05–9:35pm | Score finalist demos on: architecture clarity, live performance, wow factor |

**Roving judge rubric (UI/UX bonus):**
- +0: Default Deep Agents chat UI only
- +1: Custom UI, functional but basic
- +2: Polished, feels like a real product (animations, branding, good UX copy)

---

## Prize Structure

| Prize | Amount | Notes |
|-------|--------|-------|
| 1st place | [TBD cash] + [TBD credits] | Main prize |
| 2nd place | [TBD cash] + [TBD credits] | |
| 3rd place | [TBD credits] | |
| Bonus: Best Architecture | [TBD credits] | Roving judge pick during build |
| Bonus: Best UI/UX | [TBD credits] | Most polished custom frontend |
| Bonus: Best .cursorrules | [TBD credits] | Judges review files post-event |

---

## Starter Repo Prep Checklist

- [ ] Pre-configured `deepagents.toml` — `auth.provider = "anonymous"`, `frontend.enabled = true`
- [ ] Starter `agent.py` — `create_deep_agent` skeleton, no tools
- [ ] Filled example `system-design-prd-template.md` as reference
- [ ] Sample `.cursorrules` file
- [ ] `brief.txt` — the product brief as a file (injected by eval harness into task #2)
- [ ] Eval harness endpoint wired up and tested against a dummy agent URL
- [ ] Live leaderboard page (auto-refresh as teams submit)
- [ ] Submission form (captures: team name, public agent URL, submission timestamp)

---

## Contingency

| Problem | Fix |
|---------|-----|
| Team can't deploy in time | Accept `deepagents dev` local URL if on shared network |
| Eval harness down | Fall back to roving judges scoring live on the agent URL manually |
| Tie in top 5 | Earliest submission timestamp wins |
| <5 teams finish | All finishers demo; adjust time accordingly |
