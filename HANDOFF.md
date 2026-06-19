# Handoff: Cognee Hackathon — BUILD the Company Brain

## Next session's focus
**Execute the implementation plan.** Design is locked, decisions resolved, cloud verified.
The plan is written and bite-sized — just run it task by task.

- **Plan:** `docs/superpowers/plans/2026-06-19-company-brain.md` (11 TDD tasks, real code in every step).
- **Suggested skill:** `superpowers:subagent-driven-development` (fresh subagent per task, review between) — recommended in the plan header. Alternative: `superpowers:executing-plans` (inline, batched checkpoints).
- Use the installed `cognee` skill (`.claude/skills/cognee/SKILL.md`) for any cognee API help.

## Project in one line
A self-cleaning "Company Brain" on Cognee for a Deloitte-like AI consultancy (Helix). Ingests scattered
client docs, answers questions, lints itself — and **never silently forgets**: every removal/override
writes a receipt. Derived from the user's agent-drift research (Constraint Lease Mesh, §8.2 "silent
disappearance is forbidden"), specialized to knowledge memory. Hackathon: Cognee Cloud, Berlin, 2026-06-19,
6pm hack → 9pm submit, €1,200 pool.

## What is already TRUE (verified tonight, not assumed)
- `cognee==1.2.0.dev1` installed in `.venv` (Python 3.14). venv at project root.
- **Cloud connection WORKS.** `serve(url, api_key)` → `CloudClient` with `remember/recall/search/forget/improve/cognify/add/close`. A live remember→recall smoke test returned the correct synthesized answer. No kickoff dependency — buildable now.
- `.env` (gitignored) holds working creds: `COGNEE_API_KEY` (64-hex), `COGNEE_CLOUD_URL` (tenant URL), `COGNEE_TENANT_ID`, `COGNEE_USER_ID`. Plan adds `ENABLE_BACKEND_ACCESS_CONTROL=false`.
- `recall()` returns `list[dict]`, answer in `["text"]`, scoped by `datasets=[...]`. Dataset scoping = the cross-client firewall.
- `remember()` kwargs: `dataset_name, session_id, self_improvement(default True), content_type, skill_improvement`. Use `self_improvement=False` on the drift control-path.
- Brief's `improve_skill(proposal_id, apply=True)` is wrong — real verb is `cognee.improve(dataset=...)`; proposals via `remember(skill_improvement={"apply":False})`. **Exact payload shape still unverified** — Task 9 Step 1 greps the source to confirm before coding.

## Locked decisions (the grill output)
1C hybrid (structured fact-records drive drift + real docs ingested) · liteparse for PDF/docx · answer-key
eval (wrong→right + score) · Cognee Cloud = backbone (LLM + hosting) · use brief API directly (no adapter
needed) · judge LLM = cloud · both clients ingested, narrate 3-4 flips · two-tier shown lightly ·
**two self-improvement loops** (our drift router + cognee native skills loop) · access-control off.
Full table in the plan header + to be logged in `implementation.md` (Task 11).

## Key design insight (de-risks the build)
The headline demo cases resolve **deterministically** — trust+recency override, hard-fact HOLD vs
low-trust source, multivalue topics = fake-contradiction kept-both, age-ratio staleness, value-equality
redundancy, no-source quarantine. So Tasks 2-5,8 are pure-logic + fully unit-tested offline. The LLM judge
is escalation-only and **PARKs when unsure** (safe). Cloud tasks (1,6,7,9) are gated integration tests.

## Data already generated (do NOT regenerate)
- `data/baustein/**` + `data/vitalis/**` — 22 realistic files: 2 PDFs (LaTeX contracts/DPA/BAA), 6 Word
  .docx (wiki/notes/onboarding), 2 Slack JSON+transcripts, 2 unsourced .txt, plus .md/.tex sources. All render-verified.
- `SEED_DATA.md` — fact catalog (source of the canonical JSONL the plan authors in Task 2).
- `TEST_EXAMPLES.md` — query tests (drives `eval/questions.jsonl`).
- `REPORT.md` — full inventory + 9 edge-case classes + limitations.
- Diagram (low-jargon, ELI5): https://claude.ai/code/artifact/72414e72-5ada-4748-ab82-a21ad70ad17f
- Decisions sheet: https://claude.ai/code/artifact/de52e73f-d541-4c69-a36d-e68e78ea685f

## First moves for the next session
1. Read the plan. Confirm execution style (subagent-driven recommended).
2. Task 1 first (skeleton + config + the gated cloud smoke test must pass — proves env still works).
3. If `pytest-asyncio` missing: `.venv/bin/pip install pytest-asyncio` + `asyncio_mode=auto` in pytest.ini.
4. Task 9 Step 1 (grep `skill_improvement`/`improve` in `.venv/.../cognee/api/v1/`) before coding the skills loop.
5. Build order is linear; pure-logic tasks (2-5,8) need no network and can go fast.

## Watch-outs
- `forget()` is coarse (dataset-level); demo relies on re-`remember`ing winners so recall favors them — the BEFORE/AFTER grade in Task 10 confirms this empirically. If recall still returns stale values, that's the place to harden.
- Kickoff may hand a fresh tenant/key — config is `.env`-driven, so just swap values.
- Caveman mode active this session (terse). "stop caveman" for prose.

## Status
Design ✅ · grill ✅ · plan ✅ · cloud verified ✅ · **code not started.** This handoff = green light to implement.
