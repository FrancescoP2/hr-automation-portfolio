# HR Automation Portfolio

Two production workflows I built and deployed in an HR team, generalised here as a portfolio reference. Both run on n8n and tie together Personio, Microsoft 365, Google Sheets and LLM APIs — the connective tissue between systems that don't talk to each other on their own.

The short version of what I do: I come from the HR side, I understand the processes, and I build the automation that removes the manual handoffs nobody enjoys. These two projects are where that started.

## The projects

**[Onboarding Wizard](./onboarding-wizard)** — when a new hire enters onboarding status in Personio, this fires off the welcome email, the IT equipment notification and the first-day calendar event automatically, with holiday-aware date logic and a manual fallback for cases it doesn't recognise. Webhook-driven, runs in seconds.
Stack: n8n, Personio API, Microsoft Graph (OAuth2).

**[AI Recruiter Assistant](./AI-Recruiter-Assistant)** — first-pass CV screening for high-volume roles. A two-stage LLM pipeline filters cheaply then ranks deeply, so a recruiter gets a scored shortlist with reasoning instead of a few hundred CVs to read. Batched for volume, logged for auditing, honest about being a triage tool and not a decision-maker.
Stack: n8n, Personio API, LLM (OpenAI/Anthropic), Google Sheets API, Microsoft Graph.

Each folder has its own README with the full reasoning, the architecture, and the things that broke along the way.

## A note on these being sanitised

Both of these were built for the company I work at, so what's published here is generalised — endpoints, credentials, templates and the company-specific business logic have been swapped for placeholders. The architecture and the implementation patterns are the real ones. I've kept the disclaimer because handling confidential and GDPR-relevant data properly is part of the job, and pretending otherwise wouldn't reflect how this actually works.

## Contact

Happy to talk about HR tech, automation and HRIS work — in the DACH region or Southern Europe. Reachable on [LinkedIn](https://linkedin.com/in/francescopetrone2/).
