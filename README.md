# Hybrid Identity & Automated IT Ticket Routing  
### Enterprise-grade IT Operations Automation with Microsoft 365

![Status](https://img.shields.io/badge/status-production-success)
![Documentation](https://img.shields.io/badge/docs-complete-blue)
![Microsoft365](https://img.shields.io/badge/Microsoft%20365-Automation-blue)
![PowerAutomate](https://img.shields.io/badge/Power%20Automate-Flow%20Enabled-purple)
![SharePoint](https://img.shields.io/badge/SharePoint-Ticketing%20Backend-green)

This repository contains a full end-to-end implementation of a **Hybrid Identity lab** and an **enterprise IT ticket automation system**, built using Microsoft 365:

- Microsoft Entra ID (Azure AD)  
- Active Directory (optional, hybrid identity)  
- Azure AD Connect / Entra Connect  
- Microsoft Forms  
- Power Automate  
- SharePoint Online  
- Microsoft 365 Groups  

The project demonstrates how organizations can **automate IT Operations**, reduce manual workload, and create scalable support structures aligned with **ITIL** and **modern workplace engineering**.

---

# 📌 Key Features

## 🔐 Hybrid Identity Architecture (AD + Entra ID)
- Supports **cloud-only** + **synchronized on-prem AD**  
- Unified authentication for automation workflows  
- Group-based team assignment  
- Identity lifecycle aligned with enterprise IAM  

---

## 📝 Standardized Request Intake (Microsoft Forms)
Centralized IT request intake using a Microsoft Form.

➡️ **Form Template:**  
📄 [it-request-form-template.json](forms/it-request-form-template.json)

Form inputs include:

- Full Name  
- Corporate Email  
- Department  
- Device Type / Category  
- Reason for Request  
- Priority  
- Required-by date  
- Attachments  

---

## ⚙️ Automated Ticket Routing (Power Automate)

Zero-touch ticket creation and assignment using dynamic routing.

➡️ **Power Automate Flow:**  
📄 [ticket-routing-flow.json](power-automate/ticket-routing-flow.json)

➡️ **Flow Variable Docs:**  
📄 [flow-variables.md](power-automate/flow-variables.md)

### Logic:
```text
IF Category contains "Hardware" → IT-Hardware  
IF Category contains "Software" → IT-Software  
IF Category contains "Network" → IT-Network  
ELSE → IT-ServiceDesk (default)

Includes:

Conditional branching

HTML email notifications

SharePoint integration

Identity-driven assignment

Default fail-safe routing

📁 SharePoint Ticket Backend (ITSM Core)

➡️ Ticket List Schema:
📄 tickets-list-schema.json

➡️ Custom Views:
📄 custom-views.md

➡️ Permissions Model:
📄 permissions-model.md

Features:

Issue title

Description generated from Form

Status (New → In Progress → Completed → Closed)

Priority

Request Category

Assigned Team

Due date

Complete audit trail

🧩 Architecture Diagrams
Architecture Overview

🖼 architecture-diagram.png

Flow Logic Diagram

🖼 flow-logic-diagram.png

Setup Guide

📘 setup-guide.md

🚀 Quick Start Guide

1️⃣ Prepare Hybrid Identity (Optional AD sync)
2️⃣ Create Microsoft 365 Groups (IT teams)
3️⃣ Deploy SharePoint Tickets list
4️⃣ Import Microsoft Form
5️⃣ Import Power Automate Flow
6️⃣ Validate full workflow end-to-end

📘 Full instructions:
➡️ setup-guide.md

📂 Repository Structure (Clickable)
📁 documentation/

📘 setup-guide.md

🖼 architecture-diagram.png

🖼 flow-logic-diagram.png

📁 power-automate/

🔄 ticket-routing-flow.json

🧩 flow-variables.md

📁 sharepoint/

📄 tickets-list-schema.json

📄 custom-views.md

🔐 permissions-model.md

📁 forms/

📄 it-request-form-template.json

📁 screenshots/

🖼 entra-id-groups.png

🖼 power-automate-flow.png

🖼 sharepoint-dashboard.png

💼 Business Value
Challenge	Solution
Requests arrive by email	Centralized SharePoint Ticketing
Manual assignment	Automated routing engine
No visibility	Dashboards + Views
Ticket bottlenecks	Group-based assignment
No audit trail	SharePoint versioning + flow logs
🛠️ Technology Stack
Technology	Purpose
Microsoft Entra ID	Identity & RBAC
Active Directory	On-prem identity
Entra Connect	Sync engine
Microsoft Forms	Intake frontend
Power Automate	Automation
SharePoint Online	Ticket backend
Exchange Online	Notifications
Microsoft 365 Groups	Team routing
🎓 Skills Demonstrated

Identity & Access Management

Cloud & Hybrid Identity Architecture

Automation with Power Automate

SharePoint engineering

ITSM workflow design

Routing engines

Modern Workplace deployment

Documentation & engineering best practices

🔮 Roadmap

SLA escalations

Adaptive Card notifications (Teams)

Power BI dashboards

AI category detection

Technician mobile app (Power Apps)

👤 Author

Vidal Reñao Lopelo
Cloud & IT Infrastructure Engineer

📧 Email: mailto:vidal-31@hotmail.com
🔗 LinkedIn: https://www.linkedin.com/in/vidalrenao

🐙 GitHub Profile: https://github.com/vidal-renao

📦 Project Repo: https://github.com/vidal-renao/hybrid-identity-ticket-automation

⭐ Acknowledgments

This project follows engineering principles used in:

Modern Workplace Engineering

Cloud Infrastructure Teams

IT Operations & ITSM

If you find this project useful, please give it a star ⭐

Last Updated: December 2024
Status: Production-ready
Version: 1.0
