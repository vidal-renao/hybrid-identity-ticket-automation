# SharePoint Permissions Model

> 🔗 **Navigation**
> - 🏠 [Back to main README](../README.md)
> - 📘 [Setup Guide](../documentation/setup-guide.md)
> - 📑 [Tickets List Schema](./tickets-list-schema.json)
> - 👁️ [Custom Views](./custom-views.md)
> - ⚙️ [Ticket Routing Flow (Power Automate)](../power-automate/ticket-routing-flow.json)
> - 🧮 [Flow Variables](../power-automate/flow-variables.md)
> - 📝 [IT Request Form Template](../forms/it-request-form-template.json)

---

## Overview

This document defines the **security and permissions structure** for the **IT Support Hub** SharePoint site and the **Tickets** list.  
The model follows **least-privilege principles** while enabling efficient collaboration between IT teams and end users.

This permissions model works together with:

- The list structure: 👉 [Tickets List Schema](./tickets-list-schema.json)  
- The reporting and queues: 👉 [Custom Views](./custom-views.md)  
- The automation: 👉 [Ticket Routing Flow](../power-automate/ticket-routing-flow.json)

---

## Security Principles

### Core Principles

1. **Least Privilege** – Users receive only the permissions necessary for their role.  
2. **Group-Based Security** – Permissions assigned to Microsoft 365 / Entra ID groups, not individuals.  
3. **Separation of Duties** – Clear distinction between IT staff, admins, and end users.  
4. **Audit Trail** – All changes tracked through SharePoint version history and audit logs.  
5. **Data Privacy** – Tickets may contain sensitive information and must be access-controlled.

### Compliance Considerations

- **GDPR**: Personal data (requester identity, contact details, location) must be protected.  
- **Data Retention**: Consider retention / deletion policies for older tickets.  
- **Audit Requirements**: Track who accessed / modified tickets.  
- **Privacy**: Optionally restrict end users so they only see their **own** tickets.

---

## Permission Levels Overview

### Standard SharePoint Permission Levels

| Permission Level   | Rights                                 | Use In This Project                  |
|--------------------|-----------------------------------------|--------------------------------------|
| **Full Control**   | Complete control                       | IT Admins only                       |
| **Design**         | Create lists/libraries, apply themes   | Not used                             |
| **Edit**           | Add/edit/delete items and documents    | Not used (too broad)                 |
| **Contribute**     | Add/edit/delete items                  | IT Team Groups / Flow service acct   |
| **Read**           | View pages and items                   | End users (optional)                 |
| **Limited Access** | Access shared resources                | Automatic                            |
| **View Only**      | View items (no download)               | Not used                             |

### Optional Custom Permission Levels

**IT Ticket Editor**  
- View Items  
- Add Items  
- Edit Items  
- Delete Items (own or all – configurable)  
- View Pages  
- Create Alerts  

**IT Ticket Viewer**  
- View Items  
- View Pages  
- Create Alerts (own items / views)

---

## Site-Level Permissions

### IT Support Hub Site

**URL**: `https://yourtenant.sharepoint.com/sites/ITSupport`  
**Type**: Team Site (SharePoint Online)

#### Site Collection Administrators

**Role**: Full administrative control over the site collection.

**Typical Members**:
- IT Manager (primary admin)  
- SharePoint Admin  
- Power Platform Service Admin (for flow troubleshooting)  

**Capabilities**:
- ✅ Site Settings  
- ✅ Add/Remove Users  
- ✅ Create Lists/Libraries  
- ✅ Manage Permissions  
- ✅ Manage Site Collection Features  
- ✅ Recycle Bin Management  

---

#### Site Owners Group

**Role**: Manage site content and structure.

**Members**:
- IT Manager  
- Senior IT Staff (2–3 people)  

**Permissions**:
- ✅ Full Control (site level)  
- ✅ Create/delete lists  
- ✅ Manage site permissions  
- ✅ View all content  

---

#### Site Members Group

**Role**: Contribute to site content.

**Members**:
- All IT Staff Groups:
  - IT-ServiceDesk  
  - IT-Hardware  
  - IT-Software  
  - IT-Network  

**Permissions**:
- ✅ Contribute (by default)  
- ⚠️ Overridden on **Tickets** list (see below).

---

#### Site Visitors Group

**Role**: Read-only access (if needed).

**Members**:
- All Company (optional)  
- Specific end-user groups  

**Permissions**:
- ✅ Read  
- ❌ No edit capabilities  

> ⚠️ If the Tickets list contains sensitive info, do **not** expose it to everyone via the site Visitors group. Control at list level instead.

---

## List-Level Permissions (Tickets List)

### Permission Inheritance

- **Inheritance**: ❌ **Broken** (Stop inheriting from site)  
- **Reason**: Default site permissions are often too broad for sensitive ticket data.

---

### Tickets List – Roles & Permissions

#### Admins: Full Control

**Principal**: IT Admin Group (or specific admin accounts)  
**Permission Level**: Full Control  

**Capabilities**:
- ✅ Manage list settings  
- ✅ Create/delete columns  
- ✅ Manage views  
- ✅ Export all data  
- ✅ Delete any item  
- ✅ Manage permissions  
- ✅ Access version history  

Assigned to:
- IT Manager  
- SharePoint Administrator  

---

#### IT Staff Groups: Contribute

**Principal**:
- IT-ServiceDesk  
- IT-Hardware  
- IT-Software  
- IT-Network  

**Permission Level**: Contribute (with item-level configuration)

**Capabilities**:
- ✅ View all tickets  
- ✅ Add new tickets (if needed)  
- ✅ Edit tickets (update status, notes, fields)  
- ✅ Delete tickets they created  
- ✅ Create personal views  
- ✅ Set alerts  
- ❌ Manage list settings  
- ❌ Change permissions  

**Recommended Item-Level Settings** (Advanced Settings on list):

```text
Read access:
  Read all items

Create and Edit access:
  Create and edit all items

Delete access:
  Delete items that were created by the user


End Users: Read (Optional)

Principal: All Company / specific department groups
Permission Level: Read (if enabled)

Capabilities:

✅ View their own tickets via filtered view (e.g., “My Submitted Tickets”)

✅ Create alerts on their own tickets

❌ Edit tickets

❌ Delete tickets

❌ View other users’ tickets (enforced via view filters + design)

💡 Alternative:
Don’t give direct access to the list. Users interact only via Forms + email notifications, or via a Power Apps front-end.

Power Automate Service Account: Contribute

Principal: Flow connection owner or dedicated service account
Permission Level: Contribute

What it does:

✅ Create new ticket items

✅ Update ticket fields (if flow includes updates)

✅ Read ticket data for conditions & routing

Notes:

This account will appear in Created By / Modified By columns for automated actions.

Make sure it has at least Contribute on the Tickets list.

Item-Level Security
Recommended Configuration

From List Settings → Advanced Settings → Item-level Permissions:

Read access:
  Option A (IT-only visibility): Read all items
  Option B (privacy per user):   Read items that were created by the user

Create and Edit access:
  IT Staff:    Create and edit all items
  End Users:   [No direct access or Read only]

Delete access:
  IT Staff:    Own items only (Admins can delete all)
  End Users:   None


✅ For this lab/project, the recommended model is:

IT teams: See and edit all items

End users: No direct access OR view only their own tickets via filtered view

Advanced Options (Not Required for Lab)

Option A – Team-Isolated Tickets

Break permission inheritance per item.

Grant access only to the AssignedGroup (IT-Hardware, IT-Software, etc.).

Pros: Maximum isolation.

Cons: Very complex to maintain, not recommended for most ITSM-style setups.

Option B – Role-Based Views (Recommended)

All IT staff see all tickets.

Use views filtered by Assigned To / AssignedGroup.

Simple, transparent, easy to manage.

Permission Assignment Process
1. Create Security Groups (Entra ID)

Already covered conceptually in the architecture:

IT-ServiceDesk

IT-Hardware

IT-Software

IT-Network

(These are used by:

Automation → routing logic

SharePoint → Assigned To & permissions

Teams/Outlook → notifications)

2. Configure Site Permissions (PnP Example)
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/ITSupport" -Interactive

# Add IT Admin as Site Collection Admin
Add-PnPSiteCollectionAdmin -Owners "admin@yourtenant.com"

# Add IT groups to Site Members
Add-PnPGroupMember -Group "IT Support Hub Members" -LoginName "IT-ServiceDesk@yourtenant.com"
Add-PnPGroupMember -Group "IT Support Hub Members" -LoginName "IT-Hardware@yourtenant.com"
Add-PnPGroupMember -Group "IT Support Hub Members" -LoginName "IT-Software@yourtenant.com"
Add-PnPGroupMember -Group "IT Support Hub Members" -LoginName "IT-Network@yourtenant.com"


(Group names may differ based on your tenant configuration.)

3. Break Inheritance on Tickets List

Via UI:

Go to Tickets list

Click Settings (gear) → List settings

Click Permissions for this list

Click Stop Inheriting Permissions

Confirm

Via PowerShell:

Set-PnPList -Identity "Tickets" -BreakRoleInheritance -CopyRoleAssignments

4. Assign Permissions on Tickets List
# Grant IT groups Contribute on Tickets list
Set-PnPListPermission -Identity "Tickets" -Group "IT-ServiceDesk" -AddRole "Contribute"
Set-PnPListPermission -Identity "Tickets" -Group "IT-Hardware"  -AddRole "Contribute"
Set-PnPListPermission -Identity "Tickets" -Group "IT-Software"  -AddRole "Contribute"
Set-PnPListPermission -Identity "Tickets" -Group "IT-Network"   -AddRole "Contribute"

# Optional: Remove broad permissions (e.g., Site Members if too wide)
Set-PnPListPermission -Identity "Tickets" -Group "IT Support Hub Members" -RemoveRole "Edit"

5. Configure Item-Level Permissions

Via UI:

Tickets list → Settings → List settings

Click Advanced settings

Under Item-level Permissions:

Read access: Read all items (for IT teams)

Create and Edit: Create and edit all items (for IT teams)

Click OK

If end users are allowed to access the list directly, combine this with views filtered by [Requester] = [Me].

Special Permissions Scenarios
Scenario 1 – Manager Needs Read-Only Access to All Tickets

Solution:

Create group: IT-Managers-ViewOnly

Assign Read permission on Tickets list.

Optional: Add a dedicated view with aggregated metrics.

Scenario 2 – Audit / Compliance Team

Solution:

Create group: IT-Audit

Grant Read on Tickets list.

Use Audit logs and Export to Excel / Power BI for review.

Scenario 3 – External Consultant (Temporary Access)

Solution:

Create Guest user account in Entra ID.

Add to a dedicated group with Read on Tickets.

Set Access Review / expiry for that account.

Scenario 4 – End Users Viewing Their Own Tickets

Option A – Views Only

Give end users Read on Tickets list.

Create view: My Submitted Tickets with filter: [Requester] = [Me].

Users cannot edit tickets but can monitor status.

Option B – No Direct Access (Recommended for strict setups)

No list access for end users.

Users are informed via:

Email notifications (from Power Automate)

Optional Power Apps front-end

Permission Verification
Test Matrix
IT Admin
□ Can access list settings
□ Can manage columns and views
□ Can create/edit/delete any ticket
□ Can manage permissions on list
□ Can export tickets (Excel / Power BI)

IT Staff (e.g., Hardware Team)
□ Can view all tickets
□ Can edit ticket fields (status, notes, etc.)
□ Can create new tickets (if allowed)
□ Can delete only own tickets
□ Cannot change list settings
□ Cannot manage permissions

End User
□ Can submit tickets via Microsoft Forms
□ Can view only their own tickets (if Read access is enabled)
□ Cannot edit or delete tickets
□ Cannot see other users' data

Power Automate
□ Flow can create new tickets
□ Flow can update ticket fields (if designed to do so)
□ No "Access Denied" errors in flow history

Troubleshooting Permissions
IT Staff Can’t Edit Tickets

Possible Causes:

Incorrect permission level (e.g., Read instead of Contribute)

Item-level permissions too restrictive

Fix:

Verify list permissions for IT group.

Check Advanced Settings → Item-level Permissions.

Power Automate Fails with “Access Denied”

Possible Causes:

Flow connection uses an account without permissions.

Fix:

Open flow → check connections.

Ensure connection owner has Contribute on Tickets list.

Re-authenticate connection if needed.

End Users See “You Don’t Have Permission”

Possible Causes:

No Read access, or list-level inheritance incorrect.

Fix:

Confirm if they should have access.

If yes → grant Read to appropriate group.

If no → Forms + email-only model is correct.

Users See All Tickets Instead of Just Their Own

Possible Causes:

View filter not configured properly.

Fix:

Ensure filter is: [Requester] = [Me].

Confirm view is saved and users are using that view.

Auditing & Compliance
Audit Logging

Enable audit logging at site / tenant level:

Track:

View item

Edit item

Delete item

Permission changes

Use Microsoft Purview / Compliance Center to query audit logs.

Periodic Permission Review

Frequency: Quarterly

Checklist:

□ Site Collection Admins still valid
□ Group memberships reviewed (IT-ServiceDesk, IT-Hardware, etc.)
□ No direct permissions to individual users on Tickets list
□ Guest users still required (or removed)
□ Audit logs enabled and reviewed
□ No overly broad groups with high privileges

Best Practices
Do ✅

Use groups, not individual accounts.

Follow least privilege at all levels.

Document exceptions and temporary access.

Test permissions with test accounts.

Combine permissions with Custom Views for usability.

Don’t ❌

Don’t grant Full Control to regular staff.

Don’t use Everyone / All Users casually.

Don’t ignore “Access Denied” errors in flows.

Don’t leave guest / external accounts with permanent access.

Related Documentation

📘 Setup Guide

📑 Tickets List Schema

👁️ Custom Views

⚙️ Ticket Routing Flow (Power Automate)

🧮 Flow Variables

📝 IT Request Form Template

Document Version: 1.0
Last Updated: December 2024
Maintained By: Vidal Reñao Lopelo
