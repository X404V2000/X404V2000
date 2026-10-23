# Nhlanhla Mawelele - `@X404V2000`

**Founder & AI Developer**

I build software with **orchestrated agent teams**; not by prompting a chatbot,
but by designing a structure that constrains what each agent can see, do, and
ship. I write the charter, the boundaries, and the gates. The agents write code
inside them.

I publish as things finish, not as they start. What's open here is what I'm ready
to be judged on.

---

## How I build

Every project starts as a written design before a single line of code exists:
scope, data model, failure modes, what "done" means.

Then I stand up a team. Each agent gets an **isolated workspace** with a
`CHARTER.md` that defines what it owns and what it may never touch, a `WORKLOG.md`
that is append-only, and a directory it alone may write to. Agents don't talk to
each other directly — they hand off through **task files** (`TASK-YYYY-NNNN.yaml`)
that carry deliverables, acceptance criteria, and required gates. Cross-domain
writes are forbidden without a handoff naming the recipient.

The structure that matters most is **who can say no**.

On AutoRefill, two of eight agents hold an absolute veto: the Security Architect
and the Red Team. They can block any task at any gate. A blocked task cannot move
until the blocker is resolved — or until the Orchestrator formally accepts the
risk in a numbered `RISK-NNN.md` file. **There is no silent override.** Every
deliverable passes four gates in fixed order, and no gate may be skipped.

I built it this way because a solo developer's failure mode is optimism. A system
where an adversarial agent has to sign off is a system that tells me the truth.

**The point isn't delegation. It's verification at the boundary.**

---

## Featured projects

### [Varsity Companion](https://github.com/X404V2000/uni_companion)
**AI study companion for South African university students.**

Summarises lecture material, finds curated resources per module, plans
deadlines, and flags modules at academic risk — built for low-data,
intermittent connections.

- Long documents are chunked and merged rather than truncated, so a 60-page
  guide is covered in full
- **RAG over the student's own material**: deterministic chunking, top-K
  retrieval, answers cited back to source passages
- Retrieval is built behind a single swappable seam — deterministic keyword
  scoring today, with the embedding column already in place so semantic search
  enables by config, no schema change
- Query isolation filters on `user_id`, so one student cannot retrieve another's
  material — no cross-tenant leakage by construction
- Answer scope is bounded: the model is instructed to answer only from supplied
  excerpts and to say when the answer isn't there
- POPIA-aware: consent tracking, soft delete, hard purge on request

`Python` · `FastAPI` · `PostgreSQL` · `RAG` · `Expo` · `POPIA`

> **Public.** In active development for a hackathon, building toward a SaaS product.

---

### AutoRefill
**Prescription refill decision-support for small clinics in resource-constrained settings.**

Helps pharmacists, clinic staff, and community health workers process chronic
medication refill requests: surfaces exceptions, automates routine eligibility
checks, and keeps an append-only audit trail of every decision.

**Decision-support, not an autonomous prescriber.** A human always makes the
final dispensing decision. The system absorbs routine cases so people can focus
on the ones that need them.

**Built by an eight-agent team** — Orchestrator, Architect, Backend, Frontend,
QA, Docs, plus two veto-holding security agents — under a `SECURITY_CHARTER.md`
whose ten rules bind every agent and override any conflicting task instruction.

The charter is enforced by command, not convention. Rule 1 bans hardcoded secrets
*grep-checkable across the repo*; Rule 2 bans PHI in logs, with an opaque-identifier
rule and a verification grep. Architecture decisions live as numbered ADRs —
SQLite WAL, audit immutability, facility-token auth, offline sync, SA data
residency.

Regulatory grounding, not a generic template:

- **POPIA** — including the 2026 Health Information Regulations
- **Medicines Act s22A(6)(f)** — repeat prescription limits by schedule
- **National Health Act s14–15** — confidentiality of health records
- **HPCSA** — six-year minimum retention

`Python` · `PostgreSQL` · `multi-agent orchestration` · `compliance-aware design`

> Engine under active development. Interface to be built at the next hackathon.

---

### [TokenGen (`passkey-gen`)](https://github.com/X404V2000/passkey_generator)
**A cryptographically secure passkey and secret key generator, built on defense-in-depth.**
**Open source — Apache 2.0.**

The security properties are the project — each one a decision I can defend:

- **Kernel CSPRNG entropy** (`getrandom(2)`), not a seeded PRNG
- **Zero modulo bias** via rejection sampling — out-of-range bytes are discarded
  so every character is exactly `1 / len(charset)` likely
- **Stateless, zero caching** — nothing retained between runs
- **Type-enforced zeroization** — secrets wiped on drop, not left to a GC
- **Constant-time comparison** for anything secret-adjacent
- **Memory locking** so sensitive buffers are never swapped

Migrating from Python to Rust after verifying the Python implementation failed
its own security invariants — a decision documented as an ADR rather than quietly
made.

The repository states plainly what it cannot do and what the freeze no longer
covers. I'd rather a reader find a gap documented than discover it.

`Rust` · `Python` · `CSPRNG` · `zeroization` · `constant-time`

> **Public.** F1–F4 frozen and verified; F5+ in progress.

---

## Also building

### [TaskMaster Fix](https://github.com/X404V2000/taskmaster_fix)
**Weekly practice at the skill AI makes easy to skip: reading code you didn't write.**

Each session, an AI writes a realistic working script with 2–5 bugs planted in it.
I fix it with no hints, working only from the expected behaviour, then write up the
root cause of every one. Broken and fixed versions stay side by side, with a test
suite pinning the fixed behaviour.

A real example from session two — a `"success"` field compared to the boolean
`False` instead of the string `"False"`, which made every failure count silently
return `0`. Nothing crashed. Everything downstream was wrong. That's the failure
mode I'm training against, alongside off-by-one thresholds, mutable default
arguments, and swallowed exceptions.

`Python` · `pytest` · `debugging` · `learning in public`

> **Public.** 2 sessions, 25 tests.

**Movement Tracker** — Real-time pose estimation and trajectory prediction from
BlazePose landmarks: physics, Kalman filtering, feature extraction and state
transitions, with a test suite covering accuracy, uncertainty, and turning.

**Failed Login Tracker** — Brute-force detection: per-IP failure counting, automated
blocking at threshold, black/whitelist management, suspicious-activity reporting.

**[FNB App Academy 2026](https://github.com/X404V2000/fnb_app_academy_2026)** — Coursework
and side tasks from the FNB App Academy: calculators, an ATM simulation, grading
systems, a contact book, and a password generator built on `secrets` rather than
`random`. The foundation under everything above.

`Python`

> **Public.**

**Password Reset Console** — Command-line account management with a complete
forgot-password flow: admin-gated account creation, one-time reset codes, safe
password updates over Postgres.

---

## In closed development

**LVCK** — a digital human system, registered and operating as an independent
business. It carries the governance layer the rest of my work borrows from:
permissions, audit ledger, executor lock, succession.

It stays closed. Not because it's unfinished — because it's a commercial asset.
I'll say what it does when it's ready to be judged on its merits, not before.

Everything else on this page exists because that one needs to exist.

---

## Background

**Founder, LVCK (Pty) Ltd** — AI-driven service platform; company registered,
compliant, and operating.

**Freelance IT Technician** — remote and on-site support: hardware diagnostics,
OS deployment (Windows/Linux), network configuration, and data recovery
(300GB+ recovered from failing drives). Maintain a multi-node home lab on salvaged
hardware for security testing.

**BSc Information Science**, University of South Africa — in progress.
Coursework: information ethics, data management, ICT systems, information retrieval.

---

## Tools

`Python` `Rust` `SQL` `Bash` `FastAPI` `PostgreSQL` `Docker` `Terraform` `LangChain`
`ChromaDB` `RAG` `Expo` / React Native `Git` `Linux` `Neovim` `local LLMs`

---

## Contact

- **LinkedIn** — linkedin.com/in/nhlanhla-mawelele-846408334
- **GitHub** — github.com/X404V2000
