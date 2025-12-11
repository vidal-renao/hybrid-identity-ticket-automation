# Hybrid Identity & Automated IT Ticket Routing (Microsoft 365)

### 🔙 [Back to Main Repository](../README.md)

---

## Overview

This document describes the design and implementation of a **hybrid identity lab**
and an **automated IT ticket routing system** using Microsoft 365 services.

The solution demonstrates how on-premises and cloud identities can be integrated
with Power Automate to create a structured ITSM-like workflow.

📄 Full documentation index:  
- [Architecture Diagram](architecture-diagram.png)  
- [Flow Logic Diagram](flow-logic-diagram.png)  
- [Power Automate Flow](../power-automate/ticket-routing-flow.json)  
- [SharePoint List Schema](../sharepoint/tickets-list-schema.json)  
- [Permissions Model](../sharepoint/permissions-model.md)

> ⚠️ This is a lab / reference implementation.  
> Tenant-specific values must be adapted before real-world usage.

---

## Solution Goals

- Centralize identity management using Microsoft Entra ID  
- Use Microsoft Forms as a standardized IT request entry point  
- Automatically create tickets in SharePoint Online  
- Route tickets to IT teams using Microsoft 365 groups  
- Avoid manual assignment and person-based routing  

For form template, see:  
➡️ [`forms/it-request-form-template.json`](../forms/it-request-form-template.json)

---

## High-Level Architecture

📌 **Identity Layer**
- On-premises Active Directory (optional)
- Microsoft Entra ID
- Microsoft 365 Groups (IT teams)

📌 **Request Intake**
- Microsoft Forms – [Form Template](../forms/it-request-form-template.json)

📌 **Automation Layer**
- Power Automate cloud flow  
  → Flow export: [`ticket-routing-flow.json`](../power-automate/ticket-routing-flow.json)  
  → Flow variables: [`flow-variables.md`](../power-automate/flow-variables.md)

📌 **Ticket Backend**
- SharePoint Online list: `Tickets`  
  → Schema: [`tickets-list-schema.json`](../sharepoint/tickets-list-schema.json)  
  → Views: [`custom-views.md`](../sharepoint/custom-views.md)

---

## Identity & Group Strategy

### User Sources

This lab environment includes:
- Cloud-only users (created directly in Entra ID)
- Users synchronized from on-premises Active Directory

More details:  
➡️ Groups overview screenshot:  
`../screenshots/entra-id-groups.png`

---

### IT Functional Groups

Tickets are always assigned to **Microsoft 365 Groups**, not individuals.

| Group Name | Purpose |
|------------|----------|
| `IT-ServiceDesk` | Default intake & triage |
| `IT-Hardware` | Hardware-related incidents |
| `IT-Software` | Software & licensing requests |
| `IT-Network` | Network & VPN incidents |

Benefits:
- Clear responsibility ownership  
- Scalability  
- No dependency on individuals  
- Easier auditing and reporting  

Permissions reference:  
➡️ [`permissions-model.md`](../sharepoint/permissions-model.md)

---

## Ticket Lifecycle

1. User submits request via Microsoft Forms  
2. Power Automate evaluates request category  
3. Appropriate IT group is selected  
4. Ticket is created in SharePoint  
5. Notifications are sent to requester + IT group  

Visual flow:  
➡️ `flow-logic-diagram.png`

SharePoint example dashboard:  
➡️ `../screenshots/sharepoint-dashboard.png`

---

## Business Value

- Standardized IT request process  
- Reduced manual workload  
- Improved traceability  
- Enterprise-style IT operations design  
- Strong portfolio project for Microsoft 365 / Cloud roles  

---

## Future Improvements

- SLA calculations  
- Escalation workflows  
- Microsoft Teams notifications  
- Power BI dashboards  
- Approval workflows for sensitive requests  

---

## 📌 Navigation

- 🔝 [Back to Main README](../README.md)  
- 📁 [Open Documentation Folder](../documentation/)  
- ⚙️ [Open Power Automate Files](../power-automate/)  
- 🗂️ [Open SharePoint Configuration](../sharepoint/)  
- 📝 [Open Form Template](../forms/)  
- 📷 [Open Screenshots](../screenshots/)  

---

## 👤 Author

**Vidal Reñao Lopelo**  
Cloud & IT Infrastructure Engineer  

📧 Email: **vidal-31@hotmail.com**  
🔗 LinkedIn: https://www.linkedin.com/in/vidalrenao  
🐙 GitHub Profile: https://github.com/vidal-renao  
📦 Repository Home: https://github.com/vidal-renao/hybrid-identity-ticket-automation  

---

**© 2024 – Documentation for professional and portfolio use.**
