# SharePoint Custom Views Configuration

> 🔗 **Navigation**
> - 🏠 [Back to main README](../README.md)
> - 📘 [Setup Guide](../documentation/setup-guide.md)
> - 📑 [Tickets List Schema](./tickets-list-schema.json)
> - 🔐 [Permissions Model](./permissions-model.md)
> - ⚙️ [Power Automate – Ticket Routing Flow](../power-automate/ticket-routing-flow.json)
> - 🧮 [Power Automate – Flow Variables](../power-automate/flow-variables.md)
> - 📝 [IT Request Form Template](../forms/it-request-form-template.json)

---

## Overview

This document provides detailed instructions for creating and configuring custom views in the **Tickets** SharePoint list. Each view serves a specific operational purpose for IT teams and management.

These views assume the list structure defined in:  
👉 [Tickets List Schema](./tickets-list-schema.json)  
and the routing logic implemented in:  
👉 [Power Automate – Ticket Routing Flow](../power-automate/ticket-routing-flow.json)

---

## View 1: Open Tickets (Default View)

### Purpose
Display all active tickets requiring attention from IT teams.

### Configuration

- **View Name**: `Open Tickets`  
- **View Type**: Standard View  
- **Set as Default**: Yes  
- **Personal/Public**: Public View (visible to all)

### Columns to Display (in order)

1. **Edit** (control column)
2. **Title** (Issue)
3. **Category**
4. **Priority**
5. **Status**
6. **Assigned To**
7. **Requester**
8. **Created**

### Filter Settings

```text
Show items only when the following is true:
  Status is equal to "New"
    OR
  Status is equal to "In Progress"

Filter Formula (Advanced Mode):

[Status] = "New" OR [Status] = "In Progress"


Sort Order

Primary Sort: Priority (Ascending)

Secondary Sort: Created (Descending)

Additional Settings

Item Limit: 50 items

Display: Show items in batches of 50

Mobile View: Enabled

Totals: None

Style: Default

Column Formatting (Optional – JSON)

Priority Column Coloring:

{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
  "elmType": "div",
  "style": {
    "padding": "4px 8px",
    "border-radius": "4px",
    "background-color": {
      "operator": "?",
      "operands": [
        {
          "operator": "==",
          "operands": [
            "@currentField",
            "High"
          ]
        },
        "#fde7e9",
        {
          "operator": "?",
          "operands": [
            {
              "operator": "==",
              "operands": [
                "@currentField",
                "Medium"
              ]
            },
            "#fff4ce",
            "#dff6dd"
          ]
        }
      ]
    },
    "color": {
      "operator": "?",
      "operands": [
        {
          "operator": "==",
          "operands": [
            "@currentField",
            "High"
          ]
        },
        "#d13438",
        {
          "operator": "?",
          "operands": [
            {
              "operator": "==",
              "operands": [
                "@currentField",
                "Medium"
              ]
            },
            "#ca5010",
            "#0b6a0b"
          ]
        }
      ]
    },
    "font-weight": "600"
  },
  "txtContent": "@currentField"
}


🔗 Related:

Tickets List Schema

Routing Logic – Power Automate

View 2: High Priority Dashboard
Purpose

Quick access to urgent tickets requiring immediate attention.

Configuration

View Name: High Priority Dashboard

View Type: Standard View

Set as Default: No

Personal/Public: Public View

Columns to Display

Edit

Title

Category

Priority

Status

Assigned To

Requester

Created

Filter Settings
Show items only when ALL of the following are true:
  Priority is equal to "High"
    AND
  Status is not equal to "Resolved"
    AND
  Status is not equal to "Closed"


Filter Formula:

[Priority] = "High" AND [Status] <> "Resolved" AND [Status] <> "Closed"

Sort Order

Primary Sort: Created (Descending)

Additional Settings

Item Limit: 100 items

Display: Show all items

Recommendation: Set up alerts for this view for IT managers.

View 3: Hardware Team Tickets
Purpose

Filtered view for the hardware support team.

Configuration

View Name: Hardware Team Tickets

View Type: Standard View

Audience: IT-Hardware group members

Columns to Display

Edit

Title

Description

Priority

Status

Requester

Location (important for hardware support)

Created

Filter Settings
Show items only when the following is true:
  Assigned To is equal to "IT-Hardware"


Filter Formula:

[AssignedTo] = "IT-Hardware"

Sort Order

Primary Sort: Priority (Ascending)

Secondary Sort: Created (Descending)

Additional Settings

Item Limit: 50 items

Target Audience: IT-Hardware Microsoft 365 Group

🔗 Related:

Permissions Model

Routing Logic

View 4: Software Team Tickets
Purpose

Dedicated view for IT-Software.

Configuration

View Name: Software Team Tickets

View Type: Standard View

Audience: IT-Software group members

Columns to Display

Edit

Title

Description

Priority

Status

Requester

Created

Filter Settings
[AssignedTo] = "IT-Software"

Sort Order

Same as Hardware Team Tickets:

Priority (Ascending)

Created (Descending)

View 5: Network Team Tickets
Purpose

Dedicated view for IT-Network.

Configuration

View Name: Network Team Tickets

View Type: Standard View

Audience: IT-Network group members

Columns to Display

Edit

Title

Description

Priority

Status

Requester

Created

Filter Settings
[AssignedTo] = "IT-Network"

Sort Order

Same as Hardware Team Tickets

View 6: Service Desk Tickets
Purpose

Service Desk triage and overview.

Configuration

View Name: Service Desk Tickets

View Type: Standard View

Audience: IT-ServiceDesk group members

Columns to Display

Edit

Title

Description

Category

Priority

Status

Requester

Created

Filter Settings
[AssignedTo] = "IT-ServiceDesk"

Sort Order

Primary Sort: Priority (Ascending)

Secondary Sort: Created (Descending)

📝 Service Desk should see Category to help with triage and potential reassignment.

View 7: Resolved This Week
Purpose

Weekly reporting and performance tracking.

Configuration

View Name: Resolved This Week

View Type: Standard View

Purpose: Reporting / metrics

Columns to Display

Title

Category

Assigned To

Requester

Created

Modified

Modified By

Filter Settings
Show items only when ALL of the following are true:
  Status is equal to "Resolved"
    AND
  Modified is greater than or equal to [Today]-7


Filter Formula:

[Status] = "Resolved" AND [Modified] >= [Today]-7

Sort Order

Primary Sort: Modified (Descending)

Additional Settings

Item Limit: 100 items

Totals: Count on Title (total resolved tickets)

Optional: Group by Assigned To for per-team metrics.

View 8: My Submitted Tickets (Personal View)
Purpose

Allow end users to see the tickets they have submitted.

Configuration

View Name: My Submitted Tickets

View Type: Personal View

Audience: End users with Read permissions

Columns to Display

Title

Category

Priority

Status

Assigned To

Created

Filter Settings
Show items only when the following is true:
  Requester is equal to [Me]


Filter Formula:

[Requester] = [Me]

Sort Order

Primary Sort: Created (Descending)

Permissions Note

Users need at least Read permission on the list to use this view.

View 9: All Tickets (Admin View)
Purpose

Complete unfiltered view for admins and auditors.

Configuration

View Name: All Tickets

View Type: Standard View

Audience: IT Admins only

Columns to Display

Edit

ID (ticket number)

Title

Category

Priority

Status

Assigned To

Requester

Created

Created By

Modified

Modified By

Filter Settings

None (shows all tickets)

Sort Order

Primary Sort: ID (Descending)

Additional Settings

Item Limit: 100 items

Display: Show in batches

Advanced View Features
Conditional Formatting – Status Column
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/v2/column-formatting.schema.json",
  "elmType": "div",
  "attributes": {
    "class": "sp-field-severity--good"
  },
  "children": [
    {
      "elmType": "span",
      "style": {
        "padding": "4px 12px",
        "border-radius": "12px",
        "display": "inline-block",
        "font-weight": "500",
        "background-color": "=if(@currentField == 'New', '#e1dfdd', if(@currentField == 'In Progress', '#fff4ce', if(@currentField == 'Resolved', '#dff6dd', '#e1dfdd')))",
        "color": "=if(@currentField == 'New', '#323130', if(@currentField == 'In Progress', '#a4262c', if(@currentField == 'Resolved', '#0b6a0b', '#605e5c')))"
      },
      "txtContent": "@currentField"
    }
  ]
}

Group By Configuration

Example: Group by Assigned To

Group By: Assigned To
Show groups: Expanded
Sort groups by: Assigned To (Ascending)


Example: Group by Priority

Group By: Priority
Show groups: Collapsed
Sort groups by: Priority (Ascending)

Creating Views: Step-by-Step
Method 1: SharePoint UI

Navigate to the Tickets list.

Click All Items dropdown → Create new view.

Choose Standard View.

Enter view name.

Select columns to display.

Configure Filter.

Configure Sort.

Set Item Limit.

Choose Audience (Public/Personal).

Click OK.

Method 2: PowerShell (PnP)
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/ITSupport" -Interactive

# Example: Create Hardware Team view
Add-PnPView -List "Tickets" -Title "Hardware Team Tickets" `
  -Fields "Title","Description","Priority","Status","Requester","Location","Created" `
  -Query "<Where><Eq><FieldRef Name='AssignedTo'/><Value Type='Text'>IT-Hardware</Value></Eq></Where><OrderBy><FieldRef Name='Priority' Ascending='TRUE'/><FieldRef Name='Created' Ascending='FALSE'/></OrderBy>"

Method 3: PnP Provisioning Template
<pnp:View DisplayName="Open Tickets" DefaultView="TRUE">
  <pnp:ViewFields>
    <pnp:FieldRef Name="Title" />
    <pnp:FieldRef Name="Category" />
    <pnp:FieldRef Name="Priority" />
    <pnp:FieldRef Name="Status" />
    <pnp:FieldRef Name="AssignedTo" />
    <pnp:FieldRef Name="Requester" />
    <pnp:FieldRef Name="Created" />
  </pnp:ViewFields>
  <pnp:Query>
    <Where>
      <Or>
        <Eq><FieldRef Name="Status"/><Value Type="Choice">New</Value></Eq>
        <Eq><FieldRef Name="Status"/><Value Type="Choice">In Progress</Value></Eq>
      </Or>
    </Where>
    <OrderBy>
      <FieldRef Name="Priority" Ascending="TRUE"/>
      <FieldRef Name="Created" Ascending="FALSE"/>
    </OrderBy>
  </pnp:Query>
  <pnp:RowLimit>50</pnp:RowLimit>
</pnp:View>

View Maintenance
Best Practices

✅ DO

Create views specific to each team's needs.

Use descriptive view names.

Set an appropriate default view.

Test filters with real data.

Document view purposes for new team members.

❌ DON'T

Create too many similar views.

Use overly complex filters that hurt performance.

Let everyone modify shared views.

Forget to update views when adding new columns.

Performance Optimization

Indexed Columns:
Index frequently filtered columns: Status, Priority, Created, AssignedTo.

Item Limits:
Use 50–100 items per page.

Avoid complex filters on calculated or lookup columns when possible.

Testing Views – Checklist
□ View displays expected tickets
□ Filter logic works correctly
□ Sort order makes sense
□ Columns are readable and sized well
□ Permissions are correct (Public / Personal)
□ Performance is acceptable (<3 seconds)
□ Mobile view renders properly

Alerting on Views
Setting Up Alerts

Example for High Priority Dashboard:

Open the High Priority Dashboard view.

Click Alert me in the toolbar.

Configure alert:

Alert Title: New High Priority Tickets

Send To: IT Manager / Service Desk leads

Change Type: New items are added

When to Send: Immediately

Recommended Alerts
View	Alert Name	When	Recipients
High Priority Dashboard	Urgent Tickets	New items	IT Manager, Service Desk
Open Tickets	Daily Open Summary	Daily summary	IT Manager
Hardware Team Tickets	New Hardware Requests	New items	IT-Hardware group
Software Team Tickets	New Software Requests	New items	IT-Software group
Exporting View Data
To Excel

Open any view.

Click Export to Excel in the toolbar.

Excel file downloads with current filter applied.

To Power BI

In Power BI Desktop, select Get Data → SharePoint Online List.

Enter the site URL.

Choose the Tickets list.

Apply view-like filters in Power Query if needed.

Troubleshooting Views
Issue: View Not Showing Expected Items

Check filter formula syntax.

Confirm internal column names.

Verify user permissions.

Ensure items actually match the filter criteria.

Issue: View Performance is Slow

Reduce item limit.

Index filtered columns.

Simplify filter logic.

Remove unnecessary columns from the view.

Issue: Users Can't See View

Confirm view is Public, not Personal.

Verify users have Read permission on the list.

Check any audience targeting settings.

Quick Reference: Core Operational Views

This is a condensed map of the most important operational views:

All Tickets (Admin) – Full overview (Admin use).

Open Tickets – Active workload (default for IT).

High Priority Tickets / Dashboard – Incident management.

Hardware / Software / Network Team Queues – Dedicated team queues based on AssignedTo.

My Active / My Submitted Tickets – Personal views for technicians or end users.

Related Documentation

📘 Setup Guide

📑 Tickets List Schema

🔐 Permissions Model

⚙️ Ticket Routing Flow (Power Automate)

🧮 Flow Variables

📝 IT Request Form Template

Last Updated: December 2024
Version: 1.0
Maintained By: Vidal Reñao Lopelo

