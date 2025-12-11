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

Initialize variable:
Name: varAssignedToEmail
Type: String
Value: ""



---

## 2. Purpose

`varAssignedToEmail` stores the **Microsoft 365 Group email address** that will receive the ticket and notification.

| IT Team | Variable Value |
|---------|----------------|
| IT Service Desk | it-servicedesk@VidalCloudSolutions.onmicrosoft.com |
| IT Hardware | it-hardware@VidalCloudSolutions.onmicrosoft.com |
| IT Software | it-software@VidalCloudSolutions.onmicrosoft.com |
| IT Network | it-network@VidalCloudSolutions.onmicrosoft.com |

This variable is used in:

- **SharePoint Create Item** → Assigned To  
- **Email notification to team** → To  
- **User confirmation email** → Body info  

---

# ⚙️ 3. Assignment Logic (Routing)

Routing is performed using **Conditions**, not Switch blocks.

### Logic summary:

```pseudo
IF RequestCategory contains "Hardware"
      varAssignedToEmail = 'it-hardware@VidalCloudSolutions.onmicrosoft.com'

ELSE IF RequestCategory contains "Software"
      varAssignedToEmail = 'it-software@VidalCloudSolutions.onmicrosoft.com'

ELSE IF RequestCategory contains "Network"
      varAssignedToEmail = 'it-network@VidalCloudSolutions.onmicrosoft.com'

ELSE
      varAssignedToEmail = 'it-servicedesk@VidalCloudSolutions.onmicrosoft.com'


Full flow logic diagram:
🖼 flow-logic-diagram.png

📥 4. Dynamic Content (From Form Trigger)

These are not variables, but are essential to the routing logic.

From Microsoft Forms Trigger
Dynamic Content	Purpose
Response Id	Passed to Get Response Details
Submission Time	Processing timestamp
From Get Response Details
Form Field	Used For	Used In
Full Name	Requester identity	SharePoint, emails
Work Email	Contact + user confirmation	Emails
Department	Reporting	SharePoint
Request Category	Routing conditions	AssignedGroup / varAssignedToEmail
Issue Summary	Ticket title	SharePoint & email subject
Detailed Description	Ticket body	SharePoint & notifications
Priority	Ticket metadata	SharePoint priority
Required by Date	Due Date	SP field
Attachment	Supporting info	SP file

Form structure reference:
📄 it-request-form-template.json

🧱 5. Variable Naming Conventions
Current convention:

PascalCase

Clear descriptive names

No ambiguous abbreviations

Recommended patterns:
Pattern	Example	Purpose
{Entity}{Action}	TicketID	Identifiers
{Action}{Entity}	AssignedGroup	Assignment logic
Is{Condition}	IsHighPriority	Boolean flags
{Entity}Date	DueDate	Time properties
🔮 6. Future Variables (Planned Enhancements)
SLA Variables
DueDate
Type: DateTime
Purpose: Used for SLA calculation

IsOverdue
Type: Boolean
Logic: Now() > DueDate AND Status != "Resolved"

EscalationLevel
Type: Integer
0 = None
1 = Supervisor
2 = Manager

Advanced Routing
RequiresApproval
Type: Boolean
Logic: Priority = High AND request contains "access" or "purchase"

OriginalGroup
Stores the first assignment for audit tracking

🧪 7. Testing Checklist

Before finalizing the flow:

 varAssignedToEmail never empty

 Correct team assigned for each category

 Default routing → ServiceDesk

 SharePoint ticket correctly created

 Group emails receive notification

 User confirmation includes assigned group

 Flow behaves correctly with attachments

🛠 8. Debugging Variables
View variable value:

Power Automate → My flows → Run history
Expand:

Initialize variable

Set variable

Create item

Common issues:
Problem	Cause	Fix
Variable empty	Wrong condition order	Check Contains logic
Wrong team	Typo in group email	Verify values
Not accessible	Scope issue	Ensure global variable
Email not sent	Invalid email address	Check group exists
🗂 9. Related Documentation

📘 Setup Guide

🖼 Flow Logic Diagram

📄 SharePoint Tickets Schema

📄 Permissions Model

🔄 Flow Export File

📅 Change Log
Version	Date	Change	Author
1.0	Dec 2024	Initial documentation	Vidal Reñao Lopelo
1.1	Dec 2024	Added navigation & cross-references	Vidal Reñao Lopelo
1.2	Planned	Add SLA and escalation variables	—
👤 Author

Vidal Reñao Lopelo
Cloud & IT Infrastructure Engineer

📧 Email: mailto:vidal-31@hotmail.com
🔗 LinkedIn: https://www.linkedin.com/in/vidalrenao

🐙 GitHub: https://github.com/vidal-renao

Last Updated: December 2024
Flow Version: 1.0
