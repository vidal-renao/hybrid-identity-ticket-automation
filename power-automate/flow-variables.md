# Power Automate – Flow Variables Documentation  
### 🔙 [Back to Main Repository](../README.md)

---

# 📘 Overview

This document explains all variables used in the **IT Ticket Routing** automation flow, their purpose, behavior, and where they are used inside the workflow.

This flow processes Microsoft Forms submissions, determines the appropriate IT team, assigns the request, and creates a structured ticket in SharePoint.

📄 Related documentation:  
- **Flow Logic Diagram** → [flow-logic-diagram.png](../documentation/flow-logic-diagram.png)  
- **Setup Guide** → [setup-guide.md](../documentation/setup-guide.md)  
- **SharePoint Schema** → [tickets-list-schema.json](../sharepoint/tickets-list-schema.json)  
- **Flow Export File** → [ticket-routing-flow.json](ticket-routing-flow.json)

---

# 🧩 Core Variable: `varAssignedToEmail`

This is the **primary routing variable** used by the system.

## 1. Definition

| Property | Value |
|---------|--------|
| **Name** | `varAssignedToEmail` |
| **Type** | String |
| **Scope** | Global (accessible across entire flow) |
| **Initialized** | Immediately after “Get response details” |
| **Default Value** | `""` (empty string) |

Example initialization block:

