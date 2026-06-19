# How to implement this project (give this to a fresh agent)

Open a new Claude Code session **in this directory** (`/Users/nidhirbhavsar/Desktop/cognee_hackathon`)
and paste the prompt below verbatim.

---

## Copy-paste prompt for the implementing agent

```
We are building the Cognee "Company Brain" hackathon project. Everything is designed, decided, and
the cloud connection is already verified working. Your job is to IMPLEMENT the written plan, nothing else.

START HERE, in order:
1. Read HANDOFF.md (current state, verified facts, locked decisions, watch-outs).
2. Read docs/superpowers/plans/2026-06-19-company-brain.md (the 11-task implementation plan — full code in every step).
3. Use the cognee skill (.claude/skills/cognee/SKILL.md) for any cognee API question.

THEN: invoke the superpowers:subagent-driven-development skill and execute the plan task-by-task with it
(fresh subagent per task, two-stage review between tasks). Do NOT batch all tasks blindly.

RULES:
- Follow the plan's TDD steps exactly: write failing test → run → implement → run → commit. One commit per task.
- Use the project venv for everything: `.venv/bin/python` (Python 3.14, cognee==1.2.0.dev1 already installed).
- Pure-logic tasks (2,3,4,5,8) need NO network — run their unit tests with `.venv/bin/python -m pytest`.
- Cloud tasks (1,6,7,9) are gated integration tests (`-m integration`); they use the working creds in .env.
  Task 1's cloud smoke test MUST pass before proceeding — it proves the env still connects.
- Before coding Task 9 (skills loop), run its Step 1 grep to confirm the real `skill_improvement`/`improve`
  payload shape from the installed cognee source — the hackathon brief's names are approximate.
- Do NOT regenerate the data in data/, SEED_DATA.md, TEST_EXAMPLES.md, or REPORT.md — already done and verified.
- Stop and show me the result after each task before moving to the next.
- Fill implementation.md (Task 11) with decisions as you go.

GOAL: a runnable `demo.py` that ingests both clients, answers 3-4 headline questions WRONG before lint and
RIGHT after, prints `clashes N->0` + path stats, and populates receipts.md. That before→after flip is the demo.
```

---

## Notes for you (the human)
- Pick **subagent-driven** when the new session asks — it's in the plan header and keeps context lean.
- If `pytest-asyncio` is missing the agent should `.venv/bin/pip install pytest-asyncio` and add `asyncio_mode = auto` to `pytest.ini` — already noted in the plan (Task 1 Step 5).
- At kickoff you may get a fresh Cognee tenant/key — just replace the values in `.env`; nothing else changes.
- Caveman mode is optional; the implementing agent doesn't need it.
- Everything the agent needs is in: `HANDOFF.md`, the plan, `SEED_DATA.md`, `data/`, and `.claude/skills/cognee/SKILL.md`.
