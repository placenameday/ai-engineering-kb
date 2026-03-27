---
name: 12-factor-agents
description: >
  Twelve engineering principles for building production-grade LLM applications, modeled after Heroku's
  12-Factor App methodology. Core thesis: a reliable agent is just well-engineered software that uses
  an LLM only where probabilistic reasoning truly helps. By Dex Horthy (HumanLayer), 2025.
survey_date: 2026-03-05
lang: en
---

# 12-Factor Agents (12要素智能体)

> Survey date: 2026-03-05.

---

**Core idea:** Twelve engineering principles for building production-grade LLM applications, modeled after Heroku's 12-Factor App methodology. Core thesis: "A reliable agent is just well-engineered software that sprinkles an LLM only where probabilistic reasoning truly helps."

**Key proponent:** Dex Horthy (HumanLayer founder, AI Engineer World's Fair 2025).

**The 12 factors:**
1. Natural Language → Tool Calls
2. Own Your Prompts (no framework abstraction)
3. Tools Are Just Structured Outputs (JSON)
4. Own the Control Flow (no opaque framework loops)
5. Own Your Context Window (curate attention)
6. Launch/Pause/Resume (human review suspension points)
7. Contact Humans via Tool Calls
8. Compact Errors into useful context
9. Small, Focused Agents (do one thing well)
10. Trigger from Anywhere (webhooks, cron, Slack, APIs)
11. Unify Execution and Business State
12. Pre-fetch Context (reduce mid-execution lookups)

**Why it matters:** Addresses the "last 20% problem" — getting from impressive demo to production reliability. The 70-80% functionality comes fast; disciplined engineering bridges the gap.

**Sources:**
- [12-Factor Agents official site](https://www.humanlayer.dev/12-factor-agents)
- [MLOps Community talk](https://home.mlops.community/public/videos/12-factor-agents-patterns-of-reliable-llm-applications-dexter-horthy-agents-in-production-2025-2025-08-06)
