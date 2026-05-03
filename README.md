# HR Automation Portfolio

Production-ready HR automation workflows built with **n8n**, integrating **Personio**, **Microsoft 365**, **Google Sheets**, and **LLM APIs** to streamline recruiting and onboarding processes at scale.

## About

I'm **Francesco Petrone**, an HR Operations professional based in Munich with hands-on experience in HR automation, talent acquisition, and data quality. This repository showcases sanitized versions of workflows I've designed and deployed in real enterprise environments.

- 📍 Munich, Germany
- 💼 [LinkedIn](https://linkedin.com/in/francescopetrone2/)
- 🌍 Multilingual: Italian (native), Spanish & Portuguese (C2), English (C1), German (A2)

## Projects

### 🚀 [Onboarding Wizard](./onboarding-wizard/)
A 12-node production workflow that automates new-hire onboarding from a Personio status change. Handles welcome communications, IT provisioning notifications, and calendar setup — with office-based routing and manual-fallback logic.

**Tech stack:** n8n · Personio API · Microsoft Graph API (Outlook) · OAuth2

### 🤖 [AI Recruiter Assistant](./ai-recruiter-assistant/)
A 22-node candidate screening pipeline featuring two-stage LLM ranking, batch processing, deduplication, and three-branch error handling. Pulls applications from Personio, scores them against a job description, and writes ranked output to Google Sheets with notification logic.

**Tech stack:** n8n · Personio API · LLM (OpenAI/Anthropic) · Google Sheets API · Microsoft Graph API

## Disclaimer

These workflows are **sanitized versions** of real production systems originally built for an enterprise client. All endpoints, credentials, and business-specific data have been replaced with placeholders. The architecture, logic, and implementation patterns are preserved as a portfolio reference.

Published with the goal of demonstrating real-world HR automation capabilities. Not intended for direct production use without adaptation to your specific environment.

## Contact

Open to discussing HR Tech, automation, and HRIS roles in DACH and Southern Europe markets. Feel free to reach out on [LinkedIn](https://linkedin.com/in/francescopetrone2/).
