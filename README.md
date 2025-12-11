# Hybrid Identity & Automated IT Ticket Routing
Enterprise-grade IT Operations Automation with Microsoft 365

![Status](https://img.shields.io/badge/status-production-success)
![Documentation](https://img.shields.io/badge/docs-complete-blue)
![Microsoft365](https://img.shields.io/badge/Microsoft%20365-Automation-blue)
![PowerAutomate](https://img.shields.io/badge/Power%20Automate-Flow-purple)
![SharePoint](https://img.shields.io/badge/SharePoint-Ticketing%20Backend-green)

This repository contains a complete implementation of a Hybrid Identity lab and an automated IT ticket routing system built with Microsoft 365:

- Microsoft Entra ID (Azure AD)
- Active Directory (optional hybrid identity)
- Azure AD Connect / Entra Connect
- Microsoft Forms
- Power Automate
- SharePoint Online
- Microsoft 365 Groups

It demonstrates how organizations can automate IT operations, reduce manual workload, and create scalable support systems aligned with modern workplace engineering.

---

# Key Features

## Hybrid Identity Architecture (AD + Entra ID)

- Supports cloud-only and hybrid synchronized identity
- Unified authentication for automation workflows
- Group-based team assignment
- Identity lifecycle aligned with enterprise IAM

---

## Standardized Request Intake (Microsoft Forms)

A centralized Microsoft Form collects IT service requests.

Form template:  
`forms/it-request-form-template.json`

Includes fields for:

- Name  
- Email  
- Category  
- Issue summary  
- Detailed description  
- Priority  
- Location  
- Contact email  

---

## Automated Ticket Routing (Power Automate)

The flow creates SharePoint tickets automatically and routes them to the correct IT team.

Flow definition:  
`power-automate/ticket-routing-flow.json`

Flow variable documentation:  
`power-automate/flow-variables.md`

Routing logic:

```
IF Category = Hardware  → IT-Hardware
IF Category = Software  → IT-Software
IF Category = Network   → IT-Network
ELSE                    → IT-ServiceDesk
```

Includes:

- Conditional logic  
- HTML email notifications  
- SharePoint integration  
- Identity-driven assignment  

---

# SharePoint Ticket Backend

Ticket list schema:  
`sharepoint/tickets-list-schema.json`

Custom views:  
`sharepoint/custom-views.md`

Permissions model:  
`sharepoint/permissions-model.md`

Features:

- Issue  
- Description  
- Priority  
- Category  
- Status lifecycle  
- Assigned team  
- Requester  
- Location  
- Full audit trail  

---

# Architecture Diagrams

## Architecture Overview
![Architecture Diagram](documentation/architecture-diagram.png)

## Flow Logic Diagram
![Flow Logic Diagram](documentation/flow-logic-diagram.png)

Setup Guide:  
`documentation/setup-guide.md`

---

# Quick Start

1. Deploy hybrid identity (optional)
2. Create Microsoft 365 IT groups
3. Deploy the SharePoint Tickets list
4. Import the Microsoft Form
5. Import or recreate the Power Automate flow
6. Test the full workflow

Full setup instructions:  
`documentation/setup-guide.md`

---

# Repository Structure

```
documentation/
 ├── setup-guide.md
 ├── architecture-diagram.png
 └── flow-logic-diagram.png

power-automate/
 ├── ticket-routing-flow.json
 └── flow-variables.md

sharepoint/
 ├── tickets-list-schema.json
 ├── custom-views.md
 └── permissions-model.md

forms/
 └── it-request-form-template.json

screenshots/
 ├── entra-id-groups.png
 ├── power-automate-flow.png
 └── sharepoint-dashboard.png

README.md
```

---

# Screenshots

## Entra ID – IT Groups
![Entra ID Groups](screenshots/entra-id-groups.png)

## Power Automate – Routing Flow
![Power Automate Flow](screenshots/power-automate-flow.png)

## SharePoint ITSM Dashboard
![SharePoint Dashboard](screenshots/sharepoint-dashboard.png)

---

# Business Value

| Challenge | Solution |
|----------|----------|
| Requests arrive by email | Centralized SharePoint ticketing |
| Manual assignment | Automated routing engine |
| No visibility | SharePoint dashboards & views |
| No audit trail | Versioning + Flow logs |
| Bottlenecks | Group-based assignment |

---

# Technology Stack

| Technology | Purpose |
|-----------|---------|
| Microsoft Entra ID | Identity & RBAC |
| Active Directory | On-prem identity |
| Entra Connect | Sync engine |
| Microsoft Forms | Frontend request intake |
| Power Automate | Workflow automation |
| SharePoint Online | Ticket backend |
| Exchange Online | Notifications |
| Microsoft 365 Groups | Assignment routing |

---

# Skills Demonstrated

- Hybrid identity architecture
- IAM administration
- Power Automate engineering
- SharePoint schema design
- ITSM workflow modeling
- Routing logic automation
- Modern workplace engineering
- Professional documentation

---

# Roadmap

- SLA timers and escalations
- Teams Adaptive Cards notifications
- Power BI dashboards
- AI-based category prediction
- Technician mobile app (Power Apps)

---

# Author

**Vidal Reñao Lopelo**  
Cloud & IT Infrastructure Engineer

Email: vidal-31@hotmail.com  
LinkedIn: https://www.linkedin.com/in/vidalrenao  
GitHub: https://github.com/vidal-renao  
Project Repo: https://github.com/vidal-renao/hybrid-identity-ticket-automation

Last Updated: December 2024  
Version: 1.0  
Status: Production-ready
