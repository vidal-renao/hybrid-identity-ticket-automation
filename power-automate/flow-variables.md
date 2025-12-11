# Power Automate Flow Variables Documentation

## Overview

This document describes all variables used in the IT Ticket Routing automation flow, their purpose, scope, and usage.

---

## Variable List

### 1. AssignedGroup

**Type**: String  
**Scope**: Flow-level  
**Purpose**: Stores the name of the IT group that will be assigned to the ticket based on request category  
**Initialized**: At the beginning of the flow (after form response retrieval)  
**Modified**: Within conditional logic blocks based on category detection

#### Usage Pattern
```
Initialize Variable
├─ Name: AssignedGroup
├─ Type: String
└─ Value: "" (empty initially)

Set Variable (in conditions)
├─ Name: AssignedGroup
└─ Value: "IT-Hardware" | "IT-Software" | "IT-Network" | "IT-ServiceDesk"
```

#### Possible Values
| Value | Condition | Description |
|-------|-----------|-------------|
| `IT-Hardware` | Request Type contains "Hardware" | Hardware support team |
| `IT-Software` | Request Type contains "Software" | Software support team |
| `IT-Network` | Request Type contains "Network" | Network support team |
| `IT-ServiceDesk` | Default (no match) | General support/triage team |

#### Example Flow Logic
```
IF Request_Type contains "Hardware"
    THEN AssignedGroup = "IT-Hardware"
ELSE IF Request_Type contains "Software"
    THEN AssignedGroup = "IT-Software"
ELSE IF Request_Type contains "Network"
    THEN AssignedGroup = "IT-Network"
ELSE
    AssignedGroup = "IT-ServiceDesk"
```

#### Where Used
- SharePoint "Create item" action → Assigned To field
- Email notification to assigned group → To field
- User confirmation email → Body (informational)

---

## Dynamic Content from Trigger

While not variables, these dynamic content values are crucial to the flow:

### From "When a new response is submitted" (Trigger)

| Dynamic Content | Source | Type | Usage |
|-----------------|--------|------|-------|
| **Response Id** | Forms trigger | String | Passed to "Get response details" |
| **Submission time** | Forms trigger | DateTime | Reference for timing |

### From "Get response details" (Forms)

| Dynamic Content | Form Field | Type | Description | Used In |
|-----------------|------------|------|-------------|---------|
| **Request Type** | Question 1 | Choice | Hardware/Software/Network/Other | Condition logic, SharePoint Category |
| **Issue Summary** | Question 2 | Text | Brief issue title | SharePoint Issue field, email subjects |
| **Detailed Description** | Question 3 | Text (long) | Full issue description | SharePoint Description, team notification |
| **Priority** | Question 4 | Choice | High/Medium/Low | SharePoint Priority field, conditional formatting |
| **Best contact email** | Question 5 | Text | User's email | User confirmation "To" field |
| **Location** | Question 6 | Text | Office/desk location | Team notification (optional field) |
| **Responder's Email** | Auto-captured | Email | Microsoft account email | SharePoint Requester field |

---

## Variable Naming Conventions

### Current Convention
- **PascalCase** for all variables (e.g., `AssignedGroup`)
- Descriptive names that indicate purpose
- No abbreviations unless universally understood

### Recommended for Future Variables

| Pattern | Example | Use Case |
|---------|---------|----------|
| `{Entity}{Action}` | `TicketID` | Storing identifiers |
| `{Action}{Entity}` | `AssignedGroup` | Assignment operations |
| `Is{Condition}` | `IsHighPriority` | Boolean flags |
| `{Entity}Count` | `TicketCount` | Counters |
| `{Entity}Date` | `DueDate` | Date/time values |

---

## Future Variable Additions

### For SLA Implementation

#### DueDate
```
Type: DateTime
Purpose: Calculate ticket due date based on priority
Logic: 
  - High: Created + 4 hours
  - Medium: Created + 24 hours
  - Low: Created + 72 hours
```

#### IsOverdue
```
Type: Boolean
Purpose: Flag tickets exceeding SLA
Logic: Current DateTime > DueDate AND Status != "Resolved"
```

#### EscalationLevel
```
Type: Integer
Purpose: Track escalation tier (0=none, 1=manager, 2=director)
Default: 0
```

### For Advanced Routing

#### RequiresApproval
```
Type: Boolean
Purpose: Flag high-cost or sensitive requests
Logic: Priority = "High" AND (Request contains "purchase" OR Request contains "access")
```

#### OriginalGroup
```
Type: String
Purpose: Store initial assignment for audit trail
Logic: Set when AssignedGroup is first determined
```

#### ReassignmentReason
```
Type: String
Purpose: Capture why ticket was reassigned
Default: "" (empty)
```

### For Metrics & Reporting

#### TicketAge
```
Type: Integer (hours)
Purpose: Calculate time since ticket creation
Logic: HOURS(Now() - Created)
```

#### ResolutionTime
```
Type: Integer (hours)
Purpose: Time from creation to resolution
Logic: HOURS(ResolvedDate - Created)
```

---

## Variable Best Practices

### Initialization
✅ **DO**: Initialize all variables at the beginning of the flow  
✅ **DO**: Use empty strings ("") or null for optional values  
✅ **DO**: Document default values in flow comments  

❌ **DON'T**: Initialize variables inside conditional blocks  
❌ **DON'T**: Use undefined variables in expressions  

### Naming
✅ **DO**: Use clear, descriptive names  
✅ **DO**: Follow consistent naming convention  
✅ **DO**: Avoid single-letter variables except in loops  

❌ **DON'T**: Use abbreviations like "AG" instead of "AssignedGroup"  
❌ **DON'T**: Mix naming conventions (camelCase and PascalCase)  

### Scope
✅ **DO**: Use flow-level variables for values needed across actions  
✅ **DO**: Use local variables (in Apply to each) when possible  
✅ **DO**: Minimize number of variables for performance  

❌ **DON'T**: Create unnecessary variables for one-time use  
❌ **DON'T**: Reuse variables for different purposes  

### Performance
✅ **DO**: Use variables to avoid repeated API calls  
✅ **DO**: Store complex expressions results in variables  
✅ **DO**: Clear variables when no longer needed (if large objects)  

❌ **DON'T**: Store entire objects when only one property is needed  
❌ **DON'T**: Create variables inside loops (use Apply to each scope)  

---

## Error Handling with Variables

### ErrorMessage Variable (Future Enhancement)

```yaml
Type: String
Purpose: Capture error details for logging/notification
Initialized: At start with ""
Set on Error: 
  - In "Configure run after" settings
  - Captures: action name, error code, error message
Usage: Send to admin if flow fails
```

### Example Error Handling Pattern
```
Try:
  Create SharePoint Item

Configure Run After (Create SharePoint Item):
  ☑ has failed
  Action: Set Variable
    Name: ErrorMessage
    Value: "Failed to create SharePoint item: @{outputs('Create_item')?['error']?['message']}"

After All Actions:
  Condition: ErrorMessage is not empty
    If yes:
      Send email to admin with ErrorMessage
```

---

## Variable Lifecycle in Flow

```
Flow Start
    ↓
Initialize AssignedGroup = ""
    ↓
Get Form Response Details
    ↓
[Conditional Logic Block]
    ├─ Condition 1: Hardware?
    │   └─ Yes → Set AssignedGroup = "IT-Hardware"
    │   └─ No → Continue
    │
    ├─ Condition 2: Software?
    │   └─ Yes → Set AssignedGroup = "IT-Software"
    │   └─ No → Continue
    │
    ├─ Condition 3: Network?
    │   └─ Yes → Set AssignedGroup = "IT-Network"
    │   └─ No → Set AssignedGroup = "IT-ServiceDesk"
    ↓
[AssignedGroup now contains team name]
    ↓
Use AssignedGroup in:
    ├─ SharePoint Create Item → "Assigned To" field
    ├─ Team Notification Email → "To" field
    └─ User Confirmation Email → Body text
    ↓
Flow End
```

---

## Variable Testing Checklist

When testing the flow, verify:

- [ ] AssignedGroup is never null or empty when creating SharePoint item
- [ ] AssignedGroup matches expected value for each category
- [ ] Default assignment (IT-ServiceDesk) triggers correctly for "Other"
- [ ] Variable is accessible in all actions where needed
- [ ] Email recipients resolve correctly using variable
- [ ] SharePoint "Assigned To" field populates with group name
- [ ] No variable conflicts or scope issues
- [ ] Variable works with non-English characters (if applicable)

---

## Debugging Variables

### View Variable Values During Flow Run

1. Navigate to **Power Automate** → **My flows**
2. Select your flow → **28-day run history**
3. Click on a specific run
4. Expand **Initialize variable** action to see initial value
5. Expand **Set variable** actions to see updated values
6. Check **Create item** action → **Inputs** to see final variable value used

### Common Variable Issues

| Symptom | Likely Cause | Solution |
|---------|--------------|----------|
| Variable is null | Not initialized before use | Add Initialize Variable at flow start |
| Wrong group assigned | Condition logic incorrect | Check "contains" operator and spelling |
| Variable not updating | Set Variable in wrong branch | Verify condition branches |
| "Variable not found" error | Scope issue or typo | Check variable name spelling exactly |

---

## Variable Expressions Reference

### Common Expressions with Variables

#### Concatenate variable with text
```
concat('Assigned to: ', variables('AssignedGroup'))
```

#### Check if variable is empty
```
empty(variables('AssignedGroup'))
```

#### Variable in conditional expression
```
if(equals(variables('AssignedGroup'), 'IT-Hardware'), 'Hardware Team', 'Other Team')
```

#### Append to group email
```
concat(variables('AssignedGroup'), '@yourdomain.com')
```

---




# Power Automate – Flow Variables

This document describes the variables used in the **IT Ticket Routing** flow and how they support automated assignment of tickets to Microsoft 365 groups.

---

## 1. Overview

The flow is designed to:

- Leer una respuesta de **Microsoft Forms**
- Decidir qué equipo de IT debe gestionar el ticket
- Guardar esa decisión en una **variable**
- Usar la variable en:
  - La creación del ticket en **SharePoint**
  - Las notificaciones por **correo**

La variable clave de este flujo es:

- `varAssignedToEmail` → contiene el **email del grupo M365** que recibirá el ticket.

---

## 2. Variable: `varAssignedToEmail`

### 2.1 Definición

- **Nombre**: `varAssignedToEmail`  
- **Tipo**: `String`  
- **Ámbito**: Global (toda la ejecución del flow)  
- **Inicialización**: Justo después de **Get response details**  
- **Valor inicial**: Cadena vacía (`""`)

Ejemplo de acción:

> **Initialize variable**  
> Name: `varAssignedToEmail`  
> Type: `String`  
> Value: *(empty)*

---




### 2.2 Objetivo

`varAssignedToEmail` se usa para almacenar el **correo electrónico** del grupo de Microsoft 365 al que se asignará el ticket.

Ejemplos de valores:

| Equipo IT        | Valor de `varAssignedToEmail`                                   |
|------------------|-----------------------------------------------------------------| 
| IT Service Desk  | `it-servicedesk@VidalCloudSolutions.onmicrosoft.com`           |
| IT Hardware      | `it-hardware@VidalCloudSolutions.onmicrosoft.com`              |
| IT Software      | `it-software@VidalCloudSolutions.onmicrosoft.com`              |
| IT Network       | `it-network@VidalCloudSolutions.onmicrosoft.com`               |

---

### 2.3 Lógica de asignación

La variable se establece usando **Conditions** (no Switch), basadas en los datos del formulario.

Ejemplo de lógica típica (simplificada):

```pseudo
IF RequestType contains 'Hardware'
    varAssignedToEmail = 'it-hardware@VidalCloudSolutions.onmicrosoft.com'
ELSE IF RequestType contains 'Software'
    varAssignedToEmail = 'it-software@VidalCloudSolutions.onmicrosoft.com'
ELSE IF RequestType contains 'Network'
    varAssignedToEmail = 'it-network@VidalCloudSolutions.onmicrosoft.com'
ELSE
    varAssignedToEmail = 'it-servicedesk@VidalCloudSolutions.onmicrosoft.com'






## Change Log

| Version | Date | Change | Author |
|---------|------|--------|--------|
| 1.0 | Dec 2024 | Initial documentation | [Your Name] |
| - | - | Future: Add SLA variables | - |
| - | - | Future: Add error handling variables | - |

---

## Related Documentation

- [Setup Guide](../documentation/setup-guide.md) - Full implementation instructions
- [Flow Logic Diagram](../documentation/flow-logic-diagram.png) - Visual representation
- [SharePoint Schema](../sharepoint/tickets-list-schema.json) - Field mappings

---

**Last Updated**: December 2024  
**Flow Version**: 1.0  
**Maintained By**: [Your Name/Team]
