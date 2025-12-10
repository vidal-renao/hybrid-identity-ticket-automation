# Implementation Setup Guide

## Prerequisites

Before beginning the implementation, ensure you have:

### Required Licenses & Subscriptions
- [ ] Microsoft 365 Business Premium or Enterprise (E3/E5)
- [ ] Power Automate per-user license or premium connector access
- [ ] SharePoint Online (included in M365)
- [ ] Microsoft Forms (included in M365)
- [ ] Exchange Online (included in M365)

### Required Permissions
- [ ] **Microsoft Entra ID Admin** (to create and manage groups)
- [ ] **SharePoint Admin** (to create site and lists)
- [ ] **Power Automate Environment Maker** (to create flows)
- [ ] **Forms Admin** (to create forms)

### Optional (for Hybrid Identity)
- [ ] On-premises Active Directory Domain Services
- [ ] Azure AD Connect installed and configured
- [ ] Hybrid Identity Administrator role

### Technical Requirements
- [ ] Modern web browser (Edge, Chrome, Firefox)
- [ ] Access to Microsoft 365 Admin Center
- [ ] Basic understanding of:
  - Microsoft Entra ID (Azure AD)
  - Power Automate
  - SharePoint Online
  - JSON (for exports)

---

## Implementation Phases

### Estimated Time
- **Phase 1** (Identity): 1-2 hours
- **Phase 2** (SharePoint): 1 hour
- **Phase 3** (Forms): 30 minutes
- **Phase 4** (Power Automate): 2-3 hours
- **Phase 5** (Testing): 2 hours

**Total**: Approximately 1 business day

---

## Phase 1: Identity Setup (Microsoft Entra ID)

### Step 1.1: Configure Hybrid Identity (Optional - Skip if Cloud-Only)

If you have on-premises Active Directory:

1. **Install Azure AD Connect** on a domain-joined server
   ```
   - Download from: https://www.microsoft.com/en-us/download/details.aspx?id=47594
   - Choose "Express Settings" for standard setup
   - Use a dedicated service account with appropriate permissions
   ```

2. **Configure Synchronization**
   - Select OUs to sync
   - Enable password hash synchronization
   - Configure filtering if needed
   - Run initial sync cycle

3. **Verify Synchronization**
   - Navigate to: **Entra ID Admin Center** → **Users**
   - Confirm on-prem users appear with "Synced from Windows Server AD" status
   - Check sync status: **Entra ID Connect** → **Sync Status**

### Step 1.2: Create Microsoft 365 Groups

1. Navigate to **Microsoft Entra ID Admin Center**
   - URL: `https://entra.microsoft.com`

2. Go to **Groups** → **All groups** → **New group**

3. Create each functional group with these settings:

#### IT-ServiceDesk Group
```
Group type: Microsoft 365
Group name: IT-ServiceDesk
Group email: IT-ServiceDesk@yourdomain.com
Description: General IT support and triage team
Privacy: Private
Owners: [Your IT Manager(s)]
Members: [Add Service Desk team members]
```

#### IT-Hardware Group
```
Group type: Microsoft 365
Group name: IT-Hardware
Group email: IT-Hardware@yourdomain.com
Description: Hardware support team (laptops, desktops, peripherals)
Privacy: Private
Owners: [Your IT Manager(s)]
Members: [Add Hardware team members]
```

#### IT-Software Group
```
Group type: Microsoft 365
Group name: IT-Software
Group email: IT-Software@yourdomain.com
Description: Software and application support team
Privacy: Private
Owners: [Your IT Manager(s)]
Members: [Add Software team members]
```

#### IT-Network Group
```
Group type: Microsoft 365
Group name: IT-Network
Group email: IT-Network@yourdomain.com
Description: Network and connectivity support team
Privacy: Private
Owners: [Your IT Manager(s)]
Members: [Add Network team members]
```

4. **Verify Group Creation**
   - Check each group appears in **Groups** list
   - Confirm email addresses are active (may take 5-10 minutes)
   - Send test email to each group address

### Step 1.3: Configure Group Settings

For each group:
1. Open group → **Settings**
2. Enable: **Let group members outside your organization email the group**
3. Enable: **Allow external senders to email this group** (if needed)
4. Set: **Who can view group conversations** → Members only

---

## Phase 2: SharePoint Backend Setup

### Step 2.1: Create SharePoint Site

1. Navigate to **SharePoint Admin Center**
   - URL: `https://admin.microsoft.com/sharepoint`

2. **Create new site**:
   ```
   Template: Team site
   Site name: IT Support Hub
   Site address: /sites/ITSupport
   Primary admin: [Your admin account]
   Language: English
   Time zone: [Your timezone]
   ```

3. **Configure site permissions**:
   - IT Admin Group: Full Control
   - IT Staff Groups: Contribute
   - End Users: Read (if they need to see their tickets)

### Step 2.2: Create Tickets List

1. In your new site, go to **Site contents** → **New** → **List**

2. Choose **Blank list**:
   ```
   Name: Tickets
   Description: IT Support Ticket Repository
   Show in site navigation: Yes
   ```

3. **Add custom columns** (in this order):

#### Column 1: Issue (Title - already exists, rename if needed)
```
Type: Single line of text
Required: Yes
```

#### Column 2: Description
```
Type: Multiple lines of text
Number of lines: 6
Text type: Plain text
Required: Yes
```

#### Column 3: Category
```
Type: Choice
Choices:
  - Hardware
  - Software
  - Network
  - Other
Default: Other
Required: Yes
Display choices using: Drop-Down Menu
```

#### Column 4: Priority
```
Type: Choice
Choices:
  - High
  - Medium
  - Low
Default: Medium
Required: Yes
Display choices using: Drop-Down Menu
```

#### Column 5: Status
```
Type: Choice
Choices:
  - New
  - In Progress
  - Resolved
  - Closed
Default: New
Required: Yes
Display choices using: Drop-Down Menu
```

#### Column 6: Assigned To
```
Type: Single line of text
Description: IT Group assigned to this ticket
Required: No
```

#### Column 7: Requester
```
Type: Person or Group
Allow multiple selections: No
Show field: Name with presence
Required: Yes
```

### Step 2.3: Create Custom Views

#### View 1: Open Tickets
```
View name: Open Tickets
Columns to display: Issue, Category, Priority, Status, Assigned To, Created
Filter: Status is equal to "New" OR Status is equal to "In Progress"
Sort: Priority (descending), then Created (newest first)
Set as default view: Yes
```

#### View 2: My Team Tickets (Example for IT-Hardware)
```
View name: Hardware Team Tickets
Columns to display: Issue, Description, Priority, Status, Requester, Created
Filter: Assigned To is equal to "IT-Hardware"
Sort: Priority (descending), Created (newest first)
Create audience: IT-Hardware group
```

Repeat for other teams.

#### View 3: High Priority
```
View name: High Priority Dashboard
Columns to display: Issue, Category, Priority, Status, Assigned To, Created
Filter: Priority is equal to "High" AND Status is not equal to "Resolved"
Sort: Created (newest first)
Alert: Set up alert to notify admins when new items match
```

#### View 4: Resolved This Week
```
View name: Resolved This Week
Columns to display: Issue, Category, Assigned To, Resolved By, Modified
Filter: Status is equal to "Resolved" AND Modified is greater than [Today]-7
Sort: Modified (newest first)
```

### Step 2.4: Configure List Settings

1. **Versioning settings**:
   - Enable version history: Yes
   - Require content approval: No (optional for your needs)

2. **Advanced settings**:
   - Allow management of content types: No
   - Item-level permissions: Read all items, Create and edit items that were created by the user

3. **Validation settings** (optional):
   Add formula to prevent status going backwards if needed

---

## Phase 3: Microsoft Forms Setup

### Step 3.1: Create IT Request Form

1. Navigate to **Microsoft Forms**
   - URL: `https://forms.office.com`

2. Click **New Form**

3. **Form Settings**:
   ```
   Title: IT Support Request
   Description: Submit your IT support needs using this form. Our team will respond as quickly as possible.
   ```

4. **Add Questions**:

#### Question 1: Request Type
```
Type: Choice (Required)
Question: What type of support do you need?
Options:
  • Hardware (laptops, monitors, keyboards, etc.)
  • Software (applications, licenses, installation)
  • Network (VPN, WiFi, connectivity issues)
  • Other (general IT support)
```

#### Question 2: Issue Summary
```
Type: Text (Required)
Question: Brief summary of your issue
Subtitle: Provide a short title (e.g., "Laptop won't turn on")
Long answer: No
```

#### Question 3: Detailed Description
```
Type: Text (Required)
Question: Detailed description
Subtitle: Please describe your issue in detail, including any error messages
Long answer: Yes
```

#### Question 4: Priority
```
Type: Choice (Required)
Question: How urgent is this request?
Options:
  • High - Work is blocked, urgent assistance needed
  • Medium - Issue is impacting work but manageable
  • Low - Minor inconvenience, not blocking work
```

#### Question 5: Contact Information
```
Type: Text (Required)
Question: Best contact email
Subtitle: We'll use this to follow up (your Microsoft account email will also be recorded)
```

#### Question 6: Location (Optional)
```
Type: Text (Optional)
Question: Your office location or desk number
Subtitle: Helps our team find you if hardware support is needed
```

### Step 3.2: Configure Form Settings

1. Click **Settings** (gear icon)
2. Configure:
   ```
   • Who can fill out this form: Only people in my organization
   • Record name: Required
   • One response per person: No
   • Shuffle questions: No
   • Customized thank you message: "Thank you! Your ticket has been submitted. You'll receive a confirmation email shortly."
   ```

3. **Share the form**:
   - Click **Share**
   - Copy link for distribution
   - Optionally embed in SharePoint site or Teams

### Step 3.3: Test Form Submission

1. Submit a test request through the form
2. Verify you receive confirmation
3. Check that response appears in Forms **Responses** tab

---

## Phase 4: Power Automate Flow Setup

### Step 4.1: Create New Flow

1. Navigate to **Power Automate**
   - URL: `https://make.powerautomate.com`

2. Select your environment (default or specific)

3. Click **Create** → **Automated cloud flow**

4. **Flow name**: `IT Ticket Routing - Automated`

5. **Trigger**: Search for "Microsoft Forms" → **When a new response is submitted**
   - Select your IT Support Request form

### Step 4.2: Get Form Response Details

1. **Add action**: Forms - **Get response details**
   - Form Id: (auto-populated from trigger)
   - Response Id: (select from dynamic content)

### Step 4.3: Initialize Variable for Group Assignment

1. **Add action**: Variables - **Initialize variable**
   ```
   Name: AssignedGroup
   Type: String
   Value: (leave empty for now)
   ```

### Step 4.4: Category Detection Logic

1. **Add action**: Control - **Condition**

#### Condition 1: Check for Hardware
```
Condition: Request Type (from form) | contains | Hardware
```

**If yes**:
- Action: Variables - **Set variable**
- Name: AssignedGroup
- Value: `IT-Hardware`

**If no**: Move to next condition

2. **Add action** (inside "If no"): Control - **Condition**

#### Condition 2: Check for Software
```
Condition: Request Type (from form) | contains | Software
```

**If yes**:
- Action: Variables - **Set variable**
- Name: AssignedGroup
- Value: `IT-Software`

**If no**: Move to next condition

3. **Add action** (inside "If no"): Control - **Condition**

#### Condition 3: Check for Network
```
Condition: Request Type (from form) | contains | Network
```

**If yes**:
- Action: Variables - **Set variable**
- Name: AssignedGroup
- Value: `IT-Network`

**If no**:
- Action: Variables - **Set variable**
- Name: AssignedGroup
- Value: `IT-ServiceDesk` (default)

### Step 4.5: Create SharePoint Item

1. **Add action** (after all conditions): SharePoint - **Create item**
   ```
   Site Address: [Select your IT Support Hub site]
   List Name: Tickets
   ```

2. **Map fields**:
   ```
   Issue: [Issue Summary from form]
   Description: [Detailed Description from form]
   Category: [Request Type from form]
   Priority: [Priority from form]
   Status: New
   Assigned To: [AssignedGroup variable]
   Requester: [Responder's Email from form]
   ```

### Step 4.6: Send Confirmation Email to User

1. **Add action**: Outlook - **Send an email (V2)**
   ```
   To: [Best contact email from form]
   Subject: Ticket Submitted - [Issue Summary]
   Body:
   ```

```html
<p>Hello,</p>

<p>Your IT support request has been received and assigned to our <strong>[AssignedGroup]</strong> team.</p>

<p><strong>Ticket Details:</strong></p>
<ul>
  <li><strong>Issue:</strong> [Issue Summary]</li>
  <li><strong>Category:</strong> [Request Type]</li>
  <li><strong>Priority:</strong> [Priority]</li>
  <li><strong>Assigned to:</strong> [AssignedGroup]</li>
</ul>

<p>Our team will review your request and respond as soon as possible.</p>

<p>Best regards,<br>IT Support Team</p>
```

### Step 4.7: Notify Assigned Group

1. **Add action**: Outlook - **Send an email (V2)**
   ```
   To: [AssignedGroup]@yourdomain.com
   Subject: New Ticket Assigned - [Issue Summary]
   Body:
   ```

```html
<p>A new support ticket has been assigned to your team.</p>

<p><strong>Ticket Information:</strong></p>
<ul>
  <li><strong>Issue:</strong> [Issue Summary]</li>
  <li><strong>Description:</strong> [Detailed Description]</li>
  <li><strong>Priority:</strong> [Priority]</li>
  <li><strong>Requester:</strong> [Responder's Email]</li>
  <li><strong>Location:</strong> [Location from form]</li>
</ul>

<p><a href="[Link to SharePoint item]">View ticket in SharePoint</a></p>

<p>Please review and update the status as you work on this request.</p>
```

### Step 4.8: Error Handling (Optional but Recommended)

1. For each action, click **...** → **Settings**
2. Configure **Run after**:
   - ✅ is successful
   - ✅ has failed (for critical actions)
   - ✅ is skipped
   - ✅ has timed out

3. Add parallel branch with error notification if needed

### Step 4.9: Save and Test Flow

1. Click **Save** (top right)
2. Click **Test** → **Manually**
3. Submit test form
4. Monitor flow run in real-time
5. Verify:
   - SharePoint item created
   - User received confirmation
   - Group received notification
   - All fields populated correctly

---

## Phase 5: Testing & Validation

### Test Scenarios

#### Test 1: Hardware Request
```
Form Input:
  - Request Type: Hardware
  - Issue: Laptop screen flickering
  - Priority: High

Expected Result:
  ✅ Assigned to IT-Hardware
  ✅ SharePoint item created
  ✅ User confirmation sent
  ✅ IT-Hardware team notified
```

#### Test 2: Software Request
```
Form Input:
  - Request Type: Software
  - Issue: Need Adobe license
  - Priority: Medium

Expected Result:
  ✅ Assigned to IT-Software
  ✅ All notifications sent
```

#### Test 3: Network Request
```
Form Input:
  - Request Type: Network
  - Issue: Cannot connect to VPN
  - Priority: High

Expected Result:
  ✅ Assigned to IT-Network
  ✅ All notifications sent
```

#### Test 4: General/Other Request
```
Form Input:
  - Request Type: Other
  - Issue: Password reset
  - Priority: Low

Expected Result:
  ✅ Assigned to IT-ServiceDesk (default)
  ✅ All notifications sent
```

### Validation Checklist

- [ ] All 4 test scenarios passed
- [ ] SharePoint list populates correctly
- [ ] Email notifications arrive within 1 minute
- [ ] Group assignments are accurate
- [ ] Form data maps to SharePoint correctly
- [ ] No errors in flow run history
- [ ] Users can view their submitted tickets (if permissions allow)
- [ ] IT teams can filter by their assigned tickets

---

## Phase 6: User Communication & Rollout

### Step 6.1: Create User Documentation

Provide users with:
1. **Form link** (from SharePoint or direct URL)
2. **Instructions**:
   - When to use the form
   - How to describe issues effectively
   - Expected response times by priority
3. **FAQ**:
   - How do I check my ticket status?
   - Who will respond to my ticket?
   - What if it's urgent?

### Step 6.2: Train IT Teams

1. **SharePoint access**: Ensure all IT staff can access Tickets list
2. **View training**: Show each team their filtered view
3. **Status updates**: Train on updating ticket status
4. **Escalation process**: Define when to reassign or escalate

### Step 6.3: Announce Go-Live

Send company-wide announcement:
```
Subject: New IT Support Request System - Now Live

We're excited to announce our new IT support ticketing system!

🔹 Submit requests via our new online form: [link]
🔹 Receive instant confirmation and ticket number
🔹 Track your request status in real-time
🔹 Faster response times with automated routing

For urgent issues, continue to call the IT help desk at [phone].

Thank you for your patience during this transition.
```

---

## Troubleshooting

### Issue: Flow fails to create SharePoint item

**Solution:**
- Verify Power Automate has permissions to the SharePoint site
- Check column names match exactly (case-sensitive)
- Ensure required fields are populated
- Review flow run history for specific error

### Issue: Group emails not being received

**Solution:**
- Verify group email addresses are correct
- Check if groups are mail-enabled (may take 10-15 minutes after creation)
- Test sending direct email to group address
- Check group spam/junk folders

### Issue: Users can't access form

**Solution:**
- Confirm form settings: "Only people in my organization"
- Check user permissions in Forms
- Verify users are signed into Microsoft 365
- Try incognito/private browsing mode

### Issue: AssignedGroup variable is blank

**Solution:**
- Check condition logic uses "contains" not "equals"
- Verify form field name matches exactly
- Add default assignment in final "else" branch
- Test with different Request Type options

---

## Maintenance & Updates

### Daily
- Monitor Power Automate run history for errors
- Check new tickets assigned correctly

### Weekly
- Review ticket metrics (volume by category, resolution time)
- Update group memberships as staff changes
- Clean up resolved/closed tickets (archive if needed)

### Monthly
- Review and optimize flow performance
- Gather user feedback
- Plan enhancements (SLA, Teams integration, etc.)

### Quarterly
- Audit group memberships
- Review SharePoint permissions
- Update documentation
- Train new IT staff

---

## Next Steps: Advanced Features

Once the basic system is operational, consider:

1. **SLA Monitoring**: Add due dates and escalation logic
2. **Microsoft Teams Integration**: Post tickets to Teams channels
3. **Power BI Reporting**: Create executive dashboards
4. **Approval Workflows**: For high-priority or expensive requests
5. **Knowledge Base**: Link common issues to solution articles
6. **Mobile Access**: Configure Power Apps for field technicians

---

## Support & Resources

### Microsoft Documentation
- [Power Automate Documentation](https://learn.microsoft.com/en-us/power-automate/)
- [SharePoint Online Documentation](https://learn.microsoft.com/en-us/sharepoint/sharepoint-online)
- [Microsoft Entra ID Documentation](https://learn.microsoft.com/en-us/entra/identity/)

### Community Resources
- [Power Automate Community](https://powerusers.microsoft.com/t5/Power-Automate-Community/ct-p/MPACommunity)
- [SharePoint Community](https://techcommunity.microsoft.com/t5/sharepoint/ct-p/SharePoint)
- [Microsoft 365 Tech Community](https://techcommunity.microsoft.com/t5/microsoft-365/ct-p/microsoft365)

---

**Document Version**: 1.0  
**Last Updated**: December 2024  
**Maintained By**: [Your Name/Team]
