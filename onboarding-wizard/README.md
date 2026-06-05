# Onboarding Wizard

An n8n workflow that handles new-hire onboarding automatically, triggered by a status change in Personio. Built for the Munich office and currently running there as the standard onboarding flow.

## Why I built it

Onboarding a new hire involved a handful of small manual steps that were easy to forget and annoying to chase. HR had to send the welcome email, IT found out about equipment setup through a Teams message (or didn't, until day one), and someone had to remember to put the orientation event on the calendar. None of it was hard. It was just scattered across three people and prone to slipping through the cracks, especially in a busy week.

So I moved the whole thing behind a single trigger: when an employee enters "Onboarding" status in Personio, everything fires on its own within a few seconds.

## What it does

The workflow listens for a Personio webhook. When someone moves into onboarding status, it pulls their full record from the Personio API, checks which office they belong to, and then — for Munich — sends the welcome email, notifies IT to prep equipment, and creates the first-day calendar event in Outlook. If the office isn't one it knows how to handle yet, it doesn't fail silently: it pings the HR Business Partner so a human picks it up.

The onboarding date calculation is the part that took the most iteration. The first version just counted forward a fixed number of days, which broke the moment a start date landed near a Bavarian public holiday — Bavaria has a few that the rest of Germany doesn't (Mariä Himmelfahrt, Heilige Drei Könige, Fronleichnam). I added holiday-aware logic after that became an obvious problem, so the dates it produces actually land on working days.

## Architecture

![Onboarding Wizard Architecture](./onboarding-wizard-architecture.jpg)

```
Personio webhook (status change)
        │
   Status = "Onboarding"? ──no──→ respond to webhook, stop
        │ yes
   Personio auth + GET employee details
        │
   Office = Munich? ──no──→ notify HRBP, let a human handle it
        │ yes
   Calculate onboarding date (holiday-aware)
        ├──→ welcome email to the new hire
        ├──→ IT notification for equipment
        └──→ Outlook calendar event for day one
```

## Stack

Built on n8n for orchestration. Talks to the Personio REST API (bearer token) for employee data, and to Microsoft Graph (OAuth2) for the email and calendar work. The date logic and a couple of routing decisions live in JavaScript Code nodes.

## A few things worth noting

The architecture is webhook-driven rather than polled — nothing runs on a schedule, it reacts to the status change directly, which is why it fires within seconds instead of waiting for the next cron tick.

The office routing is deliberately built to fail loud, not quiet. The "unknown office" path was the first thing I added once I realised the bigger risk wasn't a bug, it was a new hire falling into a gap nobody noticed. Routing everything unrecognised to a human felt safer than trying to handle every case up front.

Email templates are prepared in German and English, since hires aren't all local.

## Running it yourself

This is a generalised version, so you'd need to wire in your own credentials and adapt it:

1. Import `workflow.json` into your n8n instance.
2. Add credentials: Personio API key, Microsoft Graph OAuth2.
3. Swap the placeholder office names for your own.
4. Adjust the email templates to your branding.
5. Set the sender email and a couple of other environment values.

## Note

This is a sanitised version of something I built and deployed for the Munich office of my current employer. Endpoints, templates, office names and the specifics of the business logic have been generalised — the structure and the approach are intact, the company details are not. Published as a portfolio reference, not as a drop-in tool.

The screenshot shows a warning on the "GET Employee Details" node, by the way — that's just because the live API credentials aren't connected in the public copy. It runs fine when they are.
