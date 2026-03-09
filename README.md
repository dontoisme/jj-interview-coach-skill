# Interview Coach + Job Journal

A fork of [noamseg/interview-coach-skill](https://github.com/noamseg/interview-coach-skill) that connects the coach to [Job Journal](https://github.com/dontoisme/Job-Journal) — a CLI-based career management system. The coach handles interview prep. Job Journal handles everything before and after: job discovery, resume generation, application tracking, and compliance.

Together they form a complete, AI-powered job search pipeline.

---

## Why This Fork?

The upstream interview coach is excellent — 23 commands, 5-dimension scoring, adaptive storybank, transcript analysis, outcome calibration. But it's an island. Every session starts with "paste your resume." It doesn't know what jobs you've applied to, what bullets you've already written, or where you are in your pipeline.

This fork adds a **data bridge** to Job Journal so the coach can:

- **Skip resume entry** — reads your career corpus (`~/.job-journal/corpus.md`) automatically during `kickoff`
- **Pre-fill your profile** — pulls name, title, experience, education from `~/.job-journal/profile.yaml`
- **Seed your storybank** — converts existing corpus bullets into story candidates instead of cold-starting
- **Know your pipeline** — queries your application database to reference real interview loops, not just what's in `coaching_state.md`

You say `kickoff`, the coach already knows your career history, and you're coaching in under a minute.

---

## The Full Pipeline

```
Job Journal                          Interview Coach
──────────                           ───────────────

/hunt, /jobs ─── Find roles
/score, /fit ─── Evaluate fit
/apply ───────── Apply + generate       kickoff ──── Profile + resume analysis
                 tailored resume ──────→ (auto-loaded from corpus)

                                        research ─── Company intelligence
                                        decode ───── JD analysis
                                        prep ─────── Interview prep brief
                                        stories ──── Build storybank
                                                     (seeded from corpus)
                                        practice ─── Drill progression
                                        mock ─────── Full simulation
                                        hype ─────── Pre-interview warmup

                 ←─── Real interview ──→

                                        debrief ──── Same-day capture
                                        analyze ──── Transcript scoring
                                        feedback ─── Outcome tracking
/track ──────── Update status           negotiate ── Comp coaching
/twc ────────── Compliance reporting    reflect ───── Post-search retro
```

---

## Quick Start

### Prerequisites

1. **Job Journal** (optional but recommended) — provides the data bridge:

```bash
git clone https://github.com/dontoisme/Job-Journal.git
cd Job-Journal
pip install -e .
jj init  # creates ~/.job-journal/ with profile and corpus
```

If you skip this, the coach works standalone — you'll just paste your resume manually during `kickoff`, same as upstream.

2. **This repo**:

```bash
git clone https://github.com/dontoisme/jj-interview-coach-skill.git
cd jj-interview-coach-skill
cp SKILL.md CLAUDE.md
```

3. **Start coaching**:

```bash
claude  # open Claude Code in this directory
```

Then say `kickoff`. If Job Journal is set up, the coach loads your profile and corpus automatically.

Requires any paid Claude plan. Also works with Claude Code (terminal), Cursor, or any environment with file system access.

---

## Commands

All 23 commands from the upstream coach are available. Grouped by workflow stage:

### Getting Started

| Command | What It Does |
|---|---|
| `kickoff` | Profile setup (auto-loaded from Job Journal if available) + time-aware action plan |

### Before Applying

| Command | What It Does |
|---|---|
| `research [company]` | Company intelligence with fit assessment (3 depth levels) |
| `decode` | JD analysis with 6 decoding lenses + batch triage for multiple JDs |
| `pitch` | Core positioning statement + context variants (elevator, networking, TMAY) |
| `linkedin` | LinkedIn profile optimization against recruiter search mechanics |
| `resume` | Resume audit — ATS, recruiter scan, bullet quality, keyword coverage |
| `outreach` | Networking outreach coaching — cold messages, warm intros, follow-ups |

### Interview Prep

| Command | What It Does |
|---|---|
| `prep [company]` | Role-specific prep brief with predicted questions + story mapping |
| `concerns` | Anticipate and counter interviewer concerns |
| `questions` | Generate tailored questions to ask them |
| `present` | Presentation round coaching — structure, timing, Q&A prep |
| `salary` | Early-process comp coaching — scripts for the recruiter screen |
| `stories` | Build storybank with STAR text, earned secrets, retrieval drills |
| `practice` | 8-stage drill progression with gating thresholds |
| `mock [format]` | Full 4-6 question simulation (behavioral, system design, panel, etc.) |
| `hype` | Pre-interview confidence + psychological warmup |

### After the Interview

| Command | What It Does |
|---|---|
| `debrief` | Same-day rapid capture while details are fresh |
| `analyze` | Transcript analysis — auto-detects format, scores per-unit, triages coaching |
| `feedback` | Capture recruiter feedback and outcomes |
| `progress` | Trend analysis, self-calibration, scoring drift detection |
| `thankyou` | Post-interview follow-up drafts |
| `negotiate` | Post-offer negotiation with exact scripts |
| `reflect` | Post-search retrospective + archive |

---

## How the Integration Works

The coach reads from Job Journal's data directory (`~/.job-journal/`) at specific moments:

| Coach Command | Job Journal Data Used |
|---|---|
| `kickoff` | `profile.yaml` (name, title, experience) + `corpus.md` (bullets, skills, summaries) |
| `stories` | `corpus.md` — seeds storybank candidates from existing bullets |
| `pitch` | `corpus.md` — draws positioning from role summaries |
| `resume` | `corpus.md` — references as the canonical bullet library |
| `prep`, `research` | `journal.db` — checks active pipeline for real interview context |

No data flows back automatically. The coach writes to its own `coaching_state.md`; Job Journal writes to its own SQLite database. They share read access to the same career data.

---

## Standalone vs. Integrated

| | Standalone (upstream) | With Job Journal (this fork) |
|---|---|---|
| Resume input | Paste manually each kickoff | Auto-loaded from corpus |
| Profile | Re-enter name, title, etc. | Pre-filled from profile.yaml |
| Storybank | Cold start | Seeded from corpus bullets |
| Pipeline awareness | Manual Outcome Log | Reads real application data |
| Job discovery | Not covered | `/hunt`, `/jobs`, `/greenhouse` |
| Resume generation | Coach gives feedback only | JD-targeted PDF generation |
| Application tracking | Not covered | Full pipeline with status lifecycle |
| Compliance | Not covered | TWC weekly activity tracking |

---

## Upstream

This is a fork of [noamseg/interview-coach-skill](https://github.com/noamseg/interview-coach-skill) (v3). All coaching logic, scoring rubrics, and command workflows come from the upstream project. This fork adds the Job Journal integration layer.

To pull upstream updates:

```bash
git fetch upstream
git merge upstream/main
cp SKILL.md CLAUDE.md  # re-apply the activation step
```

Then re-append the Job Journal integration section to `CLAUDE.md` (it's gitignored, so it won't be in upstream's SKILL.md).

---

## License

MIT — same as upstream.
