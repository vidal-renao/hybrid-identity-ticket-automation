# SharePoint Custom Views Configuration

## Overview

This document provides detailed instructions for creating and configuring custom views in the Tickets SharePoint list. Each view serves a specific operational purpose for IT teams and management.

---

## View 1: Open Tickets (Default View)

### Purpose
Display all active tickets requiring attention from IT teams.

### Configuration

**View Name**: `Open Tickets`  
**View Type**: Standard View  
**Set as Default**: Yes  
**Personal/Public**: Public View (visible to all)

### Columns to Display (in order)

1. **Edit** (control column) - checkbox for bulk actions
2. **Title** (Issue)
3. **Category**
4. **Priority**
5. **Status**
6. **Assigned To**
7. **Requester**
8. **Created**

### Filter Settings

```
Show items only when the following is true:
  Status is equal to "New"
    OR
  Status is equal to "In Progress"
```

**Filter Formula** (Advanced Mode):
```
[Status] = "New" OR [Status] = "In Progress"
```

### Sort Order

1. **Primary Sort**: Priority (Ascending)
   - This shows High priority first
2. **Secondary Sort**: Created (Descending)
   - Within same priority, newest tickets first

### Additional Settings

- **Item Limit**: 50 items
- **Display**: Show items in batches of 50
- **Mobile View**: Enabled
- **Totals**: None
- **Style**: Default

### Column Formatting (Optional - JSON)

**Priority Column Coloring**:
```json
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
```

---

## View 2: High Priority Dashboard

### Purpose
Quick access to urgent tickets requiring immediate attention.

### Configuration

**View Name**: `High Priority Dashboard`  
**View Type**: Standard View  
**Set as Default**: No  
**Personal/Public**: Public View

### Columns to Display

1. **Edit**
2. **Title**
3. **Category**
4. **Priority**
5. **Status**
6. **Assigned To**
7. **Requester**
8. **Created**

### Filter Settings

```
Show items only when ALL of the following are true:
  Priority is equal to "High"
    AND
  Status is not equal to "Resolved"
    AND
  Status is not equal to "Closed"
```

**Filter Formula**:
```
[Priority] = "High" AND [Status] <> "Resolved" AND [Status] <> "Closed"
```

### Sort Order

1. **Primary Sort**: Created (Descending)
   - Shows most recent urgent tickets first

### Additional Settings

- **Item Limit**: 100 items
- **Display**: Show all items
- **Alert Recommendation**: Set up alert for this view to notify managers of new high-priority tickets

---

## View 3: Hardware Team Tickets

### Purpose
Filtered view for hardware support team members.

### Configuration

**View Name**: `Hardware Team Tickets`  
**View Type**: Standard View  
**Audience**: IT-Hardware group members

### Columns to Display

1. **Edit**
2. **Title**
3. **Description**
4. **Priority**
5. **Status**
6. **Requester**
7. **Location** (important for hardware support)
8. **Created**

### Filter Settings

```
Show items only when the following is true:
  Assigned To is equal to "IT-Hardware"
```

**Filter Formula**:
```
[AssignedTo] = "IT-Hardware"
```

### Sort Order

1. **Primary Sort**: Priority (Ascending)
2. **Secondary Sort**: Created (Descending)

### Additional Settings

- **Item Limit**: 50 items
- **Target Audience**: IT-Hardware Microsoft 365 Group
- **Pin to Top**: Option to pin critical items

---

## View 4: Software Team Tickets

### Configuration

**View Name**: `Software Team Tickets`  
**Audience**: IT-Software group members

### Columns to Display

1. **Edit**
2. **Title**
3. **Description**
4. **Priority**
5. **Status**
6. **Requester**
7. **Created**

### Filter Settings

```
[AssignedTo] = "IT-Software"
```

### Sort Order
Same as Hardware Team view

---

## View 5: Network Team Tickets

### Configuration

**View Name**: `Network Team Tickets`  
**Audience**: IT-Network group members

### Columns to Display

1. **Edit**
2. **Title**
3. **Description**
4. **Priority**
5. **Status**
6. **Requester**
7. **Created**

### Filter Settings

```
[AssignedTo] = "IT-Network"
```

### Sort Order
Same as Hardware Team view

---

## View 6: Service Desk Tickets

### Configuration

**View Name**: `Service Desk Tickets`  
**Audience**: IT-ServiceDesk group members

### Columns to Display

1. **Edit**
2. **Title**
3. **Description**
4. **Category**
5. **Priority**
6. **Status**
7. **Requester**
8. **Created**

### Filter Settings

```
[AssignedTo] = "IT-ServiceDesk"
```

### Sort Order

1. **Primary Sort**: Priority (Ascending)
2. **Secondary Sort**: Created (Descending)

### Special Notes
Service Desk needs to see Category to help with triage and potential reassignment.

---

## View 7: Resolved This Week

### Purpose
Weekly reporting and team performance tracking.

### Configuration

**View Name**: `Resolved This Week`  
**View Type**: Standard View  
**Purpose**: Reporting and metrics

### Columns to Display

1. **Title**
2. **Category**
3. **Assigned To**
4. **Requester**
5. **Created**
6. **Modified**
7. **Modified By**

### Filter Settings

```
Show items only when ALL of the following are true:
  Status is equal to "Resolved"
    AND
  Modified is greater than or equal to [Today]-7
```

**Filter Formula**:
```
[Status] = "Resolved" AND [Modified] >= [Today]-7
```

### Sort Order

1. **Primary Sort**: Modified (Descending)
   - Shows most recently resolved tickets first

### Additional Settings

- **Item Limit**: 100 items
- **Totals**: Count of Title field (shows total resolved tickets)
- **Group By**: Assigned To (optional, to see per-team metrics)

---

## View 8: My Submitted Tickets (Personal View)

### Purpose
Allow end users to see tickets they've submitted.

### Configuration

**View Name**: `My Submitted Tickets`  
**View Type**: Personal View (each user sees their own)  
**Create For**: End users with Read permissions

### Columns to Display

1. **Title**
2. **Category**
3. **Priority**
4. **Status**
5. **Assigned To**
6. **Created**

### Filter Settings

```
Show items only when the following is true:
  Requester is equal to [Me]
```

**Filter Formula**:
```
[Requester] = [Me]
```

### Sort Order

1. **Primary Sort**: Created (Descending)

### Permissions Note
Users need at least Read permission on the list to use this view.

---

## View 9: All Tickets (Admin View)

### Purpose
Complete unfiltered view for administrators and auditing.

### Configuration

**View Name**: `All Tickets`  
**View Type**: Standard View  
**Audience**: IT Admins only

### Columns to Display

1. **Edit**
2. **ID** (ticket number)
3. **Title**
4. **Category**
5. **Priority**
6. **Status**
7. **Assigned To**
8. **Requester**
9. **Created**
10. **Created By**
11. **Modified**
12. **Modified By**

### Filter Settings
None - shows all tickets

### Sort Order

1. **Primary Sort**: ID (Descending)
   - Shows newest tickets first by ID

### Additional Settings

- **Item Limit**: 100 items
- **Display**: Show items in batches
- **Include**: ID column for reference

---

## Advanced View Features

### Conditional Formatting

#### Status Column
```json
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
```

### Group By Configuration

For reporting views, consider grouping:

**Example: Group by Assigned To**
```
Group By: Assigned To
Show groups: Expanded
Sort groups by: Assigned To (Ascending)
```

**Example: Group by Priority**
```
Group By: Priority
Show groups: Collapsed
Sort groups by: Priority (Ascending)
```

---

## Creating Views: Step-by-Step

### Method 1: SharePoint UI

1. Navigate to Tickets list
2. Click **All Items** dropdown → **Create new view**
3. Choose **Standard View**
4. Enter view name
5. Select columns to display (checkboxes)
6. Configure filter in "Filter" section
7. Configure sort in "Sort" section
8. Set item limit in "Item Limit" section
9. Choose audience (Public/Personal) in "Audience" section
10. Click **OK**

### Method 2: PowerShell (For Multiple Views)

```powershell
Connect-PnPOnline -Url "https://yourtenant.sharepoint.com/sites/ITSupport" -Interactive

# Example: Create Hardware Team view
Add-PnPView -List "Tickets" -Title "Hardware Team Tickets" -Fields "Title","Description","Priority","Status","Requester","Location","Created" -Query "<Where><Eq><FieldRef Name='AssignedTo'/><Value Type='Text'>IT-Hardware</Value></Eq></Where><OrderBy><FieldRef Name='Priority' Ascending='TRUE'/><FieldRef Name='Created' Ascending='FALSE'/></OrderBy>"
```

### Method 3: PnP Provisioning Template

```xml
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
```

---

## View Maintenance

### Best Practices

✅ **DO**:
- Create views specific to each team's needs
- Use descriptive view names
- Set appropriate default view for main use case
- Test filters with sample data
- Document view purposes for new team members

❌ **DON'T**:
- Create too many similar views (causes confusion)
- Use complex filters that impact performance
- Grant edit permissions to views intended for read-only use
- Forget to update views when adding new columns

### Performance Optimization

- **Indexed Columns**: Ensure frequently filtered columns are indexed
  - Status, Priority, Created, AssignedTo should all be indexed
- **Item Limits**: Use reasonable limits (50-100 items) for better performance
- **Avoid**: Filtering on calculated fields or lookup columns when possible

### Testing Views

```
Test Checklist:
□ View displays expected tickets
□ Filter logic works correctly
□ Sort order is intuitive
□ Columns are appropriately sized
□ Permissions are correct (public vs. personal)
□ Performance is acceptable (<3 seconds to load)
□ Mobile view renders properly
```

---

## Alerting on Views

### Setting Up Alerts

For high-priority monitoring:

1. Open the **High Priority Dashboard** view
2. Click **Alert me** in the toolbar
3. Configure alert:
   ```
   Alert Title: New High Priority Tickets
   Send to: [Your email]
   Change Type: New items are added
   When to Send: Immediately
   ```

### Recommended Alerts

| View | Alert Name | When | Recipients |
|------|------------|------|------------|
| High Priority Dashboard | Urgent Tickets | New items | IT Manager, Service Desk |
| Open Tickets | Unresolved Old Tickets | Daily summary | IT Manager |
| Hardware Team Tickets | New Hardware Requests | Immediate | IT-Hardware group |
| Software Team Tickets | New Software Requests | Immediate | IT-Software group |

---

## Exporting View Data

### To Excel
1. Open any view
2. Click **Export to Excel** in toolbar
3. Excel file downloads with current filter applied
4. Use for offline analysis or reporting

### To Power BI
1. In Power BI Desktop, click **Get Data** → **SharePoint Online List**
2. Enter site URL
3. Select "Tickets" list
4. Apply view filters in Power Query if needed

---

## Troubleshooting Views

### Issue: View Not Showing Expected Items

**Check**:
- Filter formula syntax is correct
- Column internal names match (case-sensitive)
- User has permissions to view items
- Items actually match filter criteria

### Issue: View Performance is Slow

**Solutions**:
- Reduce item limit
- Index filtered columns
- Simplify filter logic
- Remove unnecessary columns

### Issue: Users Can't See View

**Check**:
- View is set to "Public" not "Personal"
- User has at least Read permission on list
- Audience targeting is correct (if used)

---

## Future View Enhancements

As the system evolves, consider adding:

- **Overdue Tickets View**: Filter by calculated due date
- **SLA Breach Dashboard**: Track tickets exceeding SLA
- **Team Performance View**: Group by Assigned To with metrics
- **Trend Analysis View**: Tickets by created date (last 30/60/90 days)
- **Customer Satisfaction View**: Filter by satisfaction ratings (future field)

---
More  methods

# SharePoint – Tickets List Views

This document describes the recommended views for operating the Tickets list efficiently.

---

## 1. All Tickets (Default View)

**Purpose**: General overview.

- Columns:
  - Issue
  - Request Category
  - Priority
  - Status
  - Assigned to
  - Due Date
  - Created
- Sort:
  - Created (Descending)

---

## 2. Open Tickets

**Purpose**: Active workload.

- Filter:
  - Status is not equal to `Resolved`
  - Status is not equal to `Closed`
- Sort:
  - Priority (High → Low)
  - Created (Descending)

---

## 3. High Priority Tickets

**Purpose**: Incident management.

- Filter:
  - Priority = `High`
  - Status ≠ `Closed`
- Sort:
  - Created (Descending)

---

## 4. Hardware Team Queue

**Purpose**: Dedicated view for IT-Hardware.

- Filter:
  - Assigned to = `IT-Hardware`
  - Status ≠ `Closed`

---

## 5. Software Team Queue

**Purpose**: Dedicated view for IT-Software.

- Filter:
  - Assigned to = `IT-Software`
  - Status ≠ `Closed`

---

## 6. Network Team Queue

**Purpose**: Dedicated view for IT-Network.

- Filter:
  - Assigned to = `IT-Network`
  - Status ≠ `Closed`

---

## 7. My Active Tickets

**Purpose**: Individual workload tracking.

- Filter:
  - Assigned to contains `[Me]`
  - Status ≠ `Closed`

## Related Documentation

- [SharePoint List Schema](./tickets-list-schema.json)
- [Setup Guide](../documentation/setup-guide.md)
- [Permissions Model](./permissions-model.md)

---

**Last Updated**: December 2024  
**Version**: 1.0  
**Maintained By**: [Your Name/Team]
