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
