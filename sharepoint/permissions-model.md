# SharePoint Permissions Model

## Overview

This document defines the security and permissions structure for the IT Support Hub SharePoint site and the Tickets list. The model follows least-privilege principles while enabling efficient collaboration.

---

## Security Principles

### Core Principles

1. **Least Privilege**: Users receive only the permissions necessary for their role
2. **Group-Based Security**: Permissions assigned to groups, not individuals
3. **Separation of Duties**: Clear distinction between IT staff, admins, and end users
4. **Audit Trail**: All changes tracked through SharePoint version history
5. **Data Privacy**: Tickets may contain sensitive information requiring controlled access

### Compliance Considerations

- **GDPR**: Personal data (requester info) must be protected
- **Data Retention**: Consider implementing retention policies
- **Audit Requirements**: Maintain logs of who accessed/modified tickets
- **Privacy**: End users should only see their own tickets (optional)

---

## Permission Levels Overview

### Standard SharePoint Permission Levels

| Permission Level | Rights | Use In This Project |
|------------------|--------|---------------------|
| **Full Control** | Complete control | IT Admins only |
| **Design** | Create lists/libraries, apply themes | Not used |
| **Edit** | Add, edit, delete items and documents | Not used (too broad) |
| **Contribute** | Add, edit, delete own items | IT Team Groups |
| **Read** | View pages and items | End Users (optional) |
| **Limited Access** | Access shared resources | Automatically granted |
| **View Only** | View items (no download) | Not used |

### Custom Permission Levels (if needed)

**IT Ticket Editor**
- View Items
- Add Items
- Edit Items
- Delete Items
- View Pages
- Create Alerts

**IT Ticket Viewer**
- View Items
- View Pages
- Create Alerts (on own items)

---

## Site-Level Permissions

### IT Support Hub Site

**URL**: `https://yourtenant.sharepoint.com/sites/ITSupport`

#### Site Collection Administrators
```
Role: Full administrative control
Members:
  - IT Manager (primary admin)
  - SharePoint Admin (secondary)
  - Power Platform Service Admin (for flow troubleshooting)

Permissions:
  ✅ Site Settings
  ✅ Add/Remove Users
  ✅ Create Lists/Libraries
  ✅ Manage Permissions
  ✅ Site Collection Features
  ✅ Recycle Bin Management
```

#### Site Owners Group
```
Role: Manage site content and structure
Members:
  - IT Manager
  - Senior IT Staff (2-3 people)

Permissions:
  ✅ Full Control (at site level)
  ✅ Create/delete lists
  ✅ Manage site permissions
  ✅ View all content
```

#### Site Members Group
```
Role: Contribute to site content
Members:
  - All IT Staff Groups:
    • IT-ServiceDesk
    • IT-Hardware
    • IT-Software
    • IT-Network

Permissions:
  ✅ Contribute (inherited to lists by default)
  ⚠️ Override on Tickets list (see below)
```

#### Site Visitors Group
```
Role: Read-only access to site
Members:
  - All Company (optional)
  - Specific end-user groups (if needed)

Permissions:
  ✅ Read
  ❌ No edit capabilities
  ⚠️ Consider security implications before adding
```

---

## List-Level Permissions (Tickets List)

### Permission Inheritance

**Break Inheritance**: YES  
**Reason**: Default site permissions are too broad for sensitive ticket data

### Tickets List Permissions

#### Admins: Full Control
```
Principal: IT Admin Group (or specific admin accounts)
Permission Level: Full Control

What they can do:
  ✅ Manage list settings
  ✅ Create/delete columns
  ✅ Manage views
  ✅ Export all data
  ✅ Delete any item
  ✅ Manage permissions
  ✅ Access version history

Assigned To:
  - IT Manager
  - SharePoint Administrator
```

#### IT Staff Groups: Contribute
```
Principal: 
  - IT-ServiceDesk
  - IT-Hardware
  - IT-Software
  - IT-Network

Permission Level: Contribute (with modifications)

What they can do:
  ✅ View all tickets
  ✅ Add new tickets (if needed)
  ✅ Edit all tickets (update status, add notes)
  ✅ Delete tickets they created
  ✅ Create personal views
  ✅ Set alerts
  ❌ Delete tickets created by automation/others
  ❌ Manage list settings
  ❌ Change permissions

Configuration:
  List Settings → Advanced Settings → Item-level Permissions:
    Read: All items
    Create: Create items
    Edit: All items
    Delete: Own items only
```

#### End Users: Read (Optional)
```
Principal: All Company or Authenticated Users
Permission Level: Read (with restrictions)

What they can do:
  ✅ View tickets (only their own, via filtered view)
  ✅ Create alerts on their tickets
  ❌ Edit any tickets
  ❌ Delete tickets
  ❌ View other users' tickets (enforced by view filters)

Configuration:
  Only enable if business requires users to see their ticket status
  Alternative: Use Power Apps portal or email notifications instead
```

#### Power Automate Service Account: Contribute
```
Principal: Power Automate (automatic)
Permission Level: Contribute

What it does:
  ✅ Create new ticket items
  ✅ Update existing tickets (if flow includes updates)
  ✅ Read ticket data for conditions

Notes:
  - Automatically granted when flow runs
  - Appears as "Created By" on automated tickets
  - Uses app-only authentication
```

---

## Item-Level Security

### Recommended Configuration

**List Settings → Advanced Settings → Item-level Permissions**:

```
Read access:
  ○ Read all items
  ● Read items that were created by the user
  (Choose based on privacy requirements)

Create and Edit access:
  ● Create items and edit items that were created by the user
  ○ Create and edit all items
  (IT staff need "all items", but end users only their own)

Delete access:
  ● None
  ○ Own items
  ○ All items
  (Typically: IT staff = Own items, Admins = All items)
```

### Custom Item-Level Permissions (Advanced)

If stricter security is needed:

**Option A: IT Staff See Only Their Team's Tickets**
- Use SharePoint audience targeting
- Each team sees only tickets assigned to their group
- Requires: Breaking inheritance on each item (complex)
- **Not recommended** for this project (reduces flexibility)

**Option B: Role-Based Views**
- All staff see all tickets (Contribute permission)
- Views filtered by assigned group
- Simpler to manage
- **Recommended approach**

---

## Permission Assignment Process

### Step-by-Step Setup

#### 1. Create Security Groups (Entra ID)

Already created in Phase 1:
- IT-ServiceDesk
- IT-Hardware
- IT-Software
- IT-Network

#### 2. Configure Site Permissions

```powershell
# PowerShell example (PnP)
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/ITSupport" -Interactive

# Add IT Admin to Site Collection Admins
Add-PnPSiteCollectionAdmin -Owners "admin@yourtenant.com"

# Add IT groups to site with Contribute
Add-PnPGroupMember -Group "Site Members" -LoginName "IT-ServiceDesk@yourtenant.com"
Add-PnPGroupMember -Group "Site Members" -LoginName "IT-Hardware@yourtenant.com"
Add-PnPGroupMember -Group "Site Members" -LoginName "IT-Software@yourtenant.com"
Add-PnPGroupMember -Group "Site Members" -LoginName "IT-Network@yourtenant.com"
```

#### 3. Break Inheritance on Tickets List

Via UI:
1. Go to Tickets list
2. Click **Settings** (gear) → **List settings**
3. Click **Permissions for this list**
4. Click **Stop Inheriting Permissions**
5. Confirm

Via PowerShell:
```powershell
Set-PnPList -Identity "Tickets" -BreakRoleInheritance -CopyRoleAssignments
```

#### 4. Assign Permissions to Tickets List

```powershell
# Grant IT groups Contribute on Tickets list
Set-PnPListPermission -Identity "Tickets" -Group "IT-ServiceDesk" -AddRole "Contribute"
Set-PnPListPermission -Identity "Tickets" -Group "IT-Hardware" -AddRole "Contribute"
Set-PnPListPermission -Identity "Tickets" -Group "IT-Software" -AddRole "Contribute"
Set-PnPListPermission -Identity "Tickets" -Group "IT-Network" -AddRole "Contribute"

# Remove default Members group if too broad
Set-PnPListPermission -Identity "Tickets" -Group "Site Members" -RemoveRole "Edit"
```

#### 5. Configure Item-Level Permissions

Via UI:
1. Tickets list → **Settings** → **List settings**
2. **Advanced settings**
3. Under "Item-level Permissions":
   - Read: **Read all items**
   - Create/Edit: **Create and edit all items** (for IT staff)
4. Click **OK**

---

## Special Permissions Scenarios

### Scenario 1: Manager Needs to See All Tickets But Not Edit

**Solution**:
```
Create custom permission level: "IT Viewer"
  - View Items
  - View Pages
  - Create Alerts
  - Export to Excel

Assign to: Managers Group
```

### Scenario 2: Audit/Compliance Team Needs Read-Only Access

**Solution**:
```
Permission Level: Read
Principal: Audit-Team group
Scope: Tickets list only (break inheritance)
Duration: Temporary (if needed)
```

### Scenario 3: External Consultant Needs Limited Access

**Solution**:
```
Create: Guest user in Entra ID
Add to: Specific time-bound security group
Permission: Read on specific view only
Expire: Set Azure AD access review
```

### Scenario 4: User Wants to See Their Own Tickets

**Solution**:
```
Option A: Create personal view with [Me] filter
  - No additional permissions needed
  - User creates their own view

Option B: Create public "My Tickets" view
  - Filter: Requester = [Me]
  - Grant Read permission to all users
  - Ensures they only see their own data
```

---

## Permission Verification

### Testing Checklist

Test with accounts from each role:

#### IT Admin Account
```
□ Can access list settings
□ Can create/edit/delete any ticket
□ Can manage permissions
□ Can export all data
□ Can see all views
```

#### IT Staff Account (e.g., Hardware Team Member)
```
□ Can view all tickets
□ Can edit ticket status and notes
□ Can create new tickets
□ Can delete only tickets they created
□ Cannot access list settings
□ Can create personal views
```

#### End User Account
```
□ Can access form to submit tickets
□ Can view their own submitted tickets (if Read granted)
□ Cannot view other users' tickets
□ Cannot edit any tickets
□ Cannot access list directly (unless Read granted)
```

#### Power Automate
```
□ Flow can create new tickets
□ Flow can update ticket fields
□ Items show "Created by" as service account
□ No permission errors in flow history
```

---

## Troubleshooting Permissions

### Common Issues

#### Issue: IT Staff Can't Edit Tickets

**Cause**: Insufficient permissions or item-level restrictions  
**Solution**:
1. Verify group has Contribute permission
2. Check item-level permissions: Should be "Edit all items"
3. Confirm user is member of IT group in Entra ID

#### Issue: Power Automate Flow Fails with "Access Denied"

**Cause**: Service account lacks permissions  
**Solution**:
1. Ensure flow connection uses correct account
2. Grant that account Contribute on Tickets list
3. Check site collection app catalog permissions
4. Re-save flow to refresh permissions

#### Issue: Users See "You don't have permission to view this list"

**Cause**: No Read permission or broken inheritance  
**Solution**:
1. Grant Read to appropriate group (if intended)
2. Check if inheritance is broken correctly
3. Verify user is authenticated (not anonymous)

#### Issue: Users See All Tickets Instead of Filtered View

**Cause**: View filter not working or permissions too broad  
**Solution**:
1. Verify view filter syntax: `[Requester] = [Me]`
2. Check that view is saved correctly
3. Test with different user account

---

## Auditing & Compliance

### Audit Logging

**Enable**: SharePoint Audit Logs
```
Settings → Site Settings → Site Collection Administration → Audit Settings

Enable:
  ✅ Editing items
  ✅ Deleting items
  ✅ Viewing items
  ✅ Editing content types and columns
  ✅ Searching site content
```

**Review Logs**:
- **Security & Compliance Center** → **Audit log search**
- Filter by: Site, List, User, Date range
- Export to CSV for analysis

### Permission Reviews

**Schedule**: Quarterly

**Review Process**:
1. Export current permissions: `Get-PnPListPermissions -Identity "Tickets"`
2. Compare with documented roles
3. Remove inactive users
4. Verify group memberships in Entra ID
5. Document any exceptions

**Checklist**:
```
□ Site Collection Admins still valid
□ IT group memberships current
□ No individual user permissions (should use groups)
□ No overly permissive access
□ Guest accounts still needed
□ Audit logs enabled and reviewed
```

---

## Best Practices

### Do's ✅

- Use groups for permissions, never individual users
- Follow least privilege principle
- Document all permission exceptions
- Regularly review and audit permissions
- Use descriptive group names
- Test permissions with different accounts
- Enable audit logging
- Break inheritance only when necessary

### Don'ts ❌

- Don't grant Full Control to regular IT staff
- Don't assign permissions to individual users
- Don't forget to test after permission changes
- Don't ignore "Access Denied" errors without investigation
- Don't share sensitive data through public views
- Don't disable audit logging
- Don't use "Everyone" or "All Users" groups casually

---

## Permission Changes Workflow

### Requesting Permission Changes

**Process**:
1. **Request**: Submit ticket or email to IT Admin
2. **Justification**: Explain business need
3. **Approval**: Manager approval required
4. **Implementation**: IT Admin makes change
5. **Documentation**: Update this document
6. **Notification**: Inform requester
7. **Review**: Schedule removal date if temporary

**Form Template**:
```
Permission Change Request

Requested By: [Name]
Date: [Date]
Type: Add / Remove / Modify
Scope: Site / List / Item
Principal: [User or Group]
Permission Level: [Full Control / Contribute / Read / Custom]
Business Justification: [Explain why needed]
Duration: Permanent / Temporary (until [date])
Manager Approval: [Yes/No] - [Manager Name]
```

---



# Other Methods
# SharePoint – Permissions Model (Tickets Management)

This document describes the permission model applied to the **Tickets** SharePoint list
used as the backend for automated IT ticket management.

The model follows **enterprise best practices**:
- Least privilege
- Group-based access
- No individual user permissions
- Clear separation of responsibilities

---

## 1. Design Principles

The permission model is based on the following principles:

- ✅ Access is granted via **Microsoft 365 groups**, not individual users
- ✅ Power Automate operates with explicit permissions
- ✅ End users do not manage tickets directly
- ✅ IT teams have controlled write access
- ✅ The model is auditable and scalable

---

## 2. SharePoint Site Level Permissions

**Site name**: IT Operations  
**Site type**: Team Site (SharePoint Online)

| Role / Group | Permission Level | Description |
|-------------|------------------|-------------|
| IT-Admins | Full Control | Site administration, list configuration, security |
| IT-ServiceDesk | Edit | Ticket triage and coordination |
| IT-Hardware | Edit | Hardware ticket handling |
| IT-Software | Edit | Software ticket handling |
| IT-Network | Edit | Network and VPN ticket handling |
| End Users (optional) | Read | View-only access if required |

> 🔒 In many environments, end users do not have access to the Tickets list at all.
> Ticket interaction is handled exclusively by IT.

---

## 3. Tickets List Level Permissions

### 3.1 Inheritance Strategy

- ✅ **Permissions are inherited** from the site
- ❌ No custom list-level permissions unless required

This simplifies management and reduces configuration errors.

---

### 3.2 Power Automate Permissions

Power Automate creates items in the Tickets list using:

- The **connection owner account**
- Or a **dedicated service account** (recommended in production)

Required permissions:
- **Contribute** on the Tickets list

> ⚠️ The Power Automate account must also have permission to resolve:
> - Users
> - Microsoft 365 groups  
> when populating the **Assigned to** field.

---

## 4. Assignment Model (Critical Concept)

Tickets are **never assigned to individual users**.

Instead:
- The **Assigned to** field allows **Microsoft 365 groups**
- Groups represent **functional IT teams**

### Groups used for assignment

| Group | Purpose |
|-----|--------|
| IT-ServiceDesk | Default intake and triage |
| IT-Hardware | Hardware-related issues |
| IT-Software | Software & licensing |
| IT-Network | Network & connectivity |

### Benefits

- Scalability
- No hardcoded users in flows
- Easy onboarding / offboarding
- Clear responsibility ownership
- Accurate workload distribution

---

## 5. Visibility & Workload Management

### 5.1 Team Views

Each IT group has a dedicated SharePoint view filtered by:
- Assigned to = Group
- Status ≠ Closed

This allows teams to:
- See only relevant tickets
- Avoid noise from other queues

---

### 5.2 Individual Workload

The **"My Active Tickets"** view uses:



## Related Documentation

- [Setup Guide](../documentation/setup-guide.md)
- [SharePoint List Schema](./tickets-list-schema.json)
- [Custom Views](./custom-views.md)

---

## Appendix: Permission Level Definitions

### Full Control
- **Base Permissions**: All permissions
- **Description**: Complete control over the site

### Contribute
- **Base Permissions**: View, Add, Edit, Delete items; View pages
- **Description**: Add, edit, and delete list items and documents

### Read
- **Base Permissions**: View pages and items; Open items and documents
- **Description**: View-only access to content

### Limited Access
- **Base Permissions**: Access specific lists, libraries, or items
- **Description**: Automatically assigned when user needs access to specific resource

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Reviewed By**: IT Manager  
**Next Review Date**: March 2025
