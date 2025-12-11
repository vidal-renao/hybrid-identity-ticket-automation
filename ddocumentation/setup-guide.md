# Hybrid Identity & Automated IT Ticket Routing (Microsoft 365)

## Overview

This document describes the design and implementation of a **hybrid identity lab**
and an **automated IT ticket routing system** using Microsoft 365 services.

The solution demonstrates how on-premises and cloud identities can be integrated
with Power Automate to create a structured ITSM-like workflow.

> ⚠️ This is a lab / reference implementation.  
> Tenant-specific values must be adapted before real-world usage.

---

## Solution Goals

- Centralize identity management using Microsoft Entra ID
- Use Microsoft Forms as a standardized IT request entry point
- Automatically create tickets in SharePoint Online
- Route tickets to IT teams using Microsoft 365 groups
- Avoid manual assignment and person-based routing

---

## High-Level Architecture

**Identity Layer**
- On-premises Active Directory (optional)
- Microsoft Entra ID
- Microsoft 365 Groups (IT teams)

**Request Intake**
- Microsoft Forms (IT Request Form)

**Automation Layer**
- Power Automate cloud flow
- Conditional routing logic

**Ticket Backend**
- SharePoint Online list: `Tickets`

---

## Identity & Group Strategy

### User Sources

The lab environment includes:
- Cloud-only users created directly in Entra ID
- Users synchronized from on-premises Active Directory

Both user types are treated identically by Power Automate and SharePoint.

---

### IT Functional Groups

Tickets are assigned to **groups**, never to individual users.

| Group Name | Purpose |
|----------|--------|
| IT-ServiceDesk | Default intake & triage |
| IT-Hardware | Hardware-related incidents |
| IT-Software | Software & licensing requests |
| IT-Network | Network & VPN incidents |

Benefits of this approach:
- Clear responsibility ownership
- Scalability
- No dependency on specific individuals
- Easier auditing and reporting

---

## Ticket Lifecycle

1. User submits request via Microsoft Forms
2. Power Automate evaluates request category
3. Appropriate IT group is selected
4. Ticket is created in SharePoint
5. Notifications are sent to:
   - Requester
   - Assigned IT group

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

**Author**: Vidal Reñao Lopelo  
**Version**: 1.0  
**Last updated**: December 2024
