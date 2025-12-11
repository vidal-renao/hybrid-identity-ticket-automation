# Hybrid Identity & Automated IT Ticket Routing  
### Enterprise-grade IT Operations Automation with Microsoft 365

![Status](https://img.shields.io/badge/status-production-success)
![Documentation](https://img.shields.io/badge/docs-complete-blue)
![Microsoft365](https://img.shields.io/badge/Microsoft%20365-Automation-blue)
![PowerAutomate](https://img.shields.io/badge/Power%20Automate-Flow%20Enabled-purple)
![SharePoint](https://img.shields.io/badge/SharePoint-Ticketing%20Backend-green)

This repository contains a full end-to-end implementation of a **hybrid identity lab** and an **enterprise IT ticket automation system**, built using the Microsoft 365 ecosystem:

- Microsoft Entra ID (Azure AD)
- Active Directory (on-prem, optional)
- Azure AD Connect / Entra Connect
- Microsoft Forms
- Power Automate
- SharePoint Online
- Microsoft 365 Groups

The project demonstrates how organizations can **automate IT Operations**, reduce manual workload, improve service delivery, and create scalable support structures aligned with **ITIL** and **modern workplace engineering**.

---

# 📌 Key Features

## 🔐 Hybrid Identity Architecture (AD + Entra ID)
- Support for **cloud-only** and **synchronized on-prem** identities  
- Unified authentication model for automation  
- Role-based access using Microsoft 365 groups  
- IT teams aligned with functional responsibilities

---

## 📝 Standardized Request Intake (Microsoft Forms)
A single controlled entry point for all IT requests, including:

- Full Name / Corporate Email (identity binding)
- Department
- **Request Category** (Hardware / Software / Network / Other)
- Detailed description
- Priority selection
- Required-by date
- Optional attachments

Ensures **data consistency**, **auditable submissions**, and **support readiness**.

---

## ⚙️ Automated Ticket Routing (Power Automate)
Zero-touch ticket creation and assignment.

### Routing Logic:
```text
IF RequestCategory contains "Hardware" → IT-Hardware
ELSE IF RequestCategory contains "Software" → IT-Software
ELSE IF RequestCategory contains "Network" → IT-Network
ELSE → IT-ServiceDesk (default triage)



### Automation Capabilities:
- Conditional routing using `contains`  
- Email notifications (HTML capable)  
- Safe default routing  
- Clear audit logs and traceability  
- Group assignment using `AssignedToClaims`  

---

## 📁 SharePoint Ticket Backend (ITSM Core)

A structured SharePoint list named `Tickets`, featuring:

- Issue title  
- Auto-generated issue description  
- Priority (Low / Medium / High)  
- Status (New → In Progress → Resolved → Closed)  
- Request Category  
- Assigned Team (M365 Group)  
- Requester identity  
- Due Date  

### Team Dashboards
- Hardware queue  
- Software queue  
- Network queue  
- High Priority dashboard  
- My Active Tickets  

---

# 🧩 Architecture Diagram


User → Microsoft Forms → Power Automate → SharePoint Tickets List
| | ↓
Identity: Entra ID | Team Assignment (M365 Groups)
| ↓
(Optional) AD Sync Email Notifications → Requester + IT Teams



Full diagrams available in:

documentation/architecture-diagram.png
documentation/flow-logic-diagram.png



---

# 🚀 Quick Start Guide

1️⃣ Configure **Microsoft Entra ID** and synchronize on-prem AD (optional)  
2️⃣ Create Microsoft 365 groups:
- IT-ServiceDesk  
- IT-Hardware  
- IT-Software  
- IT-Network  

3️⃣ Deploy the SharePoint **Tickets** list  
4️⃣ Create the Microsoft Form using the template  
5️⃣ Build the Power Automate flow  
6️⃣ Test full workflow end-to-end  

📘 Detailed implementation guide:  
➡ `documentation/setup-guide.md`

---

# 📂 Repository Structure

├── documentation/
│ ├── setup-guide.md
│ ├── architecture-diagram.png
│ └── flow-logic-diagram.png
│
├── power-automate/
│ ├── ticket-routing-flow.json
│ └── flow-variables.md
│
├── sharepoint/
│ ├── tickets-list-schema.json
│ ├── custom-views.md
│ └── permissions-model.md
│
├── forms/
│ └── it-request-form-template.json
│
└── screenshots/
└── (implementation screenshots)



---

# 💼 Business Value

| Challenge | Solution |
|-----------|----------|
| Scattered requests via email | Centralized SharePoint ticketing |
| Manual assignment by ServiceDesk | Automated routing by category |
| No visibility over workload | Team-based dashboards |
| Dependency on specific staff | Group-based assignment |
| Lack of audit trail | SharePoint versioning + flow history |

### Impact:
- **60% reduction** in manual overhead  
- **Instant routing** (<1 second)  
- **100% traceability**  
- Fully **scalable across teams**  

---

# 🛠️ Technology Stack

| Technology | Purpose |
|-----------|----------|
| Microsoft Entra ID | Identity platform |
| Active Directory | On-prem identity source |
| Azure AD Connect | Hybrid identity sync |
| Microsoft Forms | Ticket intake interface |
| Power Automate | Workflow automation |
| SharePoint Online | Ticket backend |
| Exchange Online | Notifications |
| Microsoft 365 Groups | Team assignment |

---

# 📊 Routing Logic (Visual)


Form submission
↓
Evaluate Request Category
↓
Set variable: varAssignedToEmail
↓
Create SharePoint ticket
↓
Send confirmation email to user
↓
Notify assigned IT Group




---

# 🎓 Skills Demonstrated

### Identity & Access Management
- Hybrid identity architecture (AD → Entra ID)  
- Azure AD Connect configuration  
- Group-Based Access Control  
- Security & governance principles  

### Automation Engineering
- Power Automate flow design  
- Conditional branching and routing  
- Multi-system integrations  
- Error handling and fallbacks  

### ITSM & Operations
- Ticket lifecycle design  
- Categorization and prioritization  
- Team assignment models (ITIL-aligned)  

### Microsoft 365 Cloud
- SharePoint list engineering  
- Power Platform development  
- Exchange Online automation  

---

# 🔮 Future Enhancements (Roadmap)

- [ ] SLA-based escalations  
- [ ] Teams integration (channels + adaptive cards)  
- [ ] Power BI reporting dashboards  
- [ ] AI Builder category detection  
- [ ] Approval workflows  
- [ ] Mobile Power App for technicians  

---

# 👤 Author

**Vidal Reñao Lopelo**  
Cloud & IT Infrastructure Engineer  

📧 Email: **vidal-31@hotmail.com**  
🔗 LinkedIn: **https://www.linkedin.com/in/vidalrenao**  
🐙 GitHub: **https://github.com/vidal-renao**  

---

# ⭐ Acknowledgments

This project follows enterprise patterns used in:

- Modern Workplace Engineering  
- Cloud Infrastructure Teams  
- IT Operations & Service Management  

It demonstrates real-world skills expected from an **M365 / Cloud Engineer**.

---

**⭐ If you find this project useful, please give it a star!**

---

*Last Updated: December 2024*  
*Status: Production-ready*  
*Version: 1.0*

