# AI Recruiter Assistant

A production-grade n8n workflow that automates first-pass CV screening using a two-stage LLM pipeline, integrating Personio applicant data with ranked output to Google Sheets.

## Problem

High-volume recruiting positions (interns, working students, technical specialists) routinely receive 100–500+ applications per requisition. Manual screening is:

- **Slow** — 2-5 minutes per CV × hundreds of CVs = full work-weeks lost per role
- **Inconsistent** — fatigue and bias creep in after the first 30-50 CVs
- **Hard to scale** — bottlenecks the entire recruiting funnel

Off-the-shelf ATS scoring is often shallow (keyword matching), and enterprise solutions like HireVue or Eightfold are expensive and overkill for mid-market needs.

## Solution

A configurable workflow that takes a job description as input and returns a ranked, scored shortlist of candidates with rationale. Two-stage AI evaluation balances cost (cheap pre-screening pass) with quality (deeper final ranking).

## Architecture
![AI Recruiter Architecture](./ai-recruiter-architecture.jpg)

```
Form Trigger (JD + thresholds)
    ↓
Settings node (parametrize job_title, jd_text, min_score, top_n)
    ↓
Personio Auth + Get Applications
    ↓
Extract & Deduplicate (custom JS)
    ↓
Create Batches (handle large volumes without timeouts)
    ↓
┌─→ Loop Batches ──┐
│       ↓          │
│  AI Pre-Screen   │  ← Stage 1: cheap LLM call, broad filter
│       ↓          │
│  Parse + Filter  │
└─────────┬────────┘
          ↓
   Has Candidates? ──No──→ Send "No Result" Notification
          ↓ Yes
   AI Final Ranking      ← Stage 2: deeper LLM call, ranked output with rationale
          ↓
   Parse Final Ranking
          ↓
   ├──→ Google Sheets (Ranking output)
   ├──→ Google Sheets (Activity log)
   └──→ Send Notification (success / error branches)
```

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Orchestration | n8n |
| Trigger | n8n Form Trigger (web form input) |
| HRIS / ATS | Personio REST API |
| LLM | OpenAI / Anthropic API (configurable) |
| Output | Google Sheets API |
| Notifications | Microsoft Graph API (Outlook) |

## Implementation Highlights

- **Two-stage AI pipeline** — cheap pre-screen filters out obvious mismatches; expensive final ranking only runs on qualified pool. Roughly 70% cost reduction vs single-stage approach.
- **Batching with split-in-batches node** — handles 500+ CVs without API timeouts or rate-limit errors.
- **Configurable thresholds** — `min_score` and `top_n` are parametrized per requisition (default: 60, 5).
- **Three-branch notification logic** — explicit success path, error path, and "no qualified candidates" path. Recruiter is never left guessing.
- **Audit log to Google Sheets** — every run is logged for compliance and post-hoc bias auditing.
- **Deduplication step** — handles candidates who apply to multiple positions.
- **Production version 4** — iterated through three previous versions to handle real-world edge cases (malformed CVs, API timeouts, empty result sets).

## File

- [`workflow.json`](./workflow.json) — Importable n8n workflow definition

## Setup

1. Import `workflow.json` into your n8n instance
2. Configure credentials: Personio API, OpenAI/Anthropic API, Google Sheets OAuth2, Microsoft Graph OAuth2
3. Set up the Form Trigger with your custom fields (Job Title, Job Description, Minimum Score, Top N Candidates)
4. Create target Google Sheets for ranking output and activity log
5. Adapt prompts in the AI nodes to your hiring criteria and tone

## Limitations & Honest Notes

- LLM scoring should always include **human-in-the-loop validation** before final hiring decisions. This workflow is designed as a triage tool, not as an autonomous decision-maker.
- Bias considerations: prompts should be regularly audited for proxy bias on protected attributes. The activity log helps with this.
- GDPR: candidate data is processed via third-party LLM APIs — ensure DPA agreements are in place before deploying.

## Notes

This is a **sanitized version** of a real production system. Prompts, business logic, and integration endpoints have been generalized.
