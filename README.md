# Hybrid Identity & Automated IT Ticket Routing
Enterprise IT ticketing automation with hybrid identity management and intelligent routing using Microsoft 365

Enterprise-grade IT ticketing automation integrating hybrid identity management with intelligent routing using Microsoft 365 services.

![Status](https://img.shields.io/badge/status-production-success)
![Documentation](https://img.shields.io/badge/docs-complete-blue)

---

## 📋 Overview

This project automates IT support ticket creation and routing by integrating on-premises Active Directory with Microsoft Entra ID, using Power Automate to intelligently assign requests to specialized IT teams based on category.

**Key Features:**
- Hybrid identity architecture (on-prem AD + Entra ID)
- Automated ticket routing (Hardware, Software, Network, General)
- Group-based assignment following ITSM principles
- Zero-touch ticket creation and notifications
- Complete audit trail via SharePoint

---

## 🏗️ Architecture

```
On-Premises AD → Microsoft Entra ID → Power Automate → SharePoint
                                              ↓
                                    Email Notifications
```

**Components:**
- **Identity Layer**: Active Directory + Microsoft Entra ID + Azure AD Connect
- **Automation**: Power Automate with conditional routing logic
- **Frontend**: Microsoft Forms for user submissions
- **Backend**: SharePoint Online (ticket repository)
- **Notifications**: Exchange Online (automated emails)

---

## 🚀 Quick Start

1. **Prerequisites**: Microsoft 365 Business Premium/Enterprise, Power Automate license
2. **Follow setup guide**: [documentation/setup-guide.md](documentation/setup-guide.md)
3. **Implementation time**: ~1 business day

**Key Steps:**
1. Configure hybrid identity (Entra ID + AD sync)
2. Create IT support groups (ServiceDesk, Hardware, Software, Network)
3. Build SharePoint Tickets list
4. Create Microsoft Form
5. Deploy Power Automate flow
6. Test and validate

---

## 📂 Repository Structure

```
├── documentation/
│   ├── setup-guide.md              # Complete implementation guide
│   ├── architecture-diagram.png    # Visual architecture
│   └── flow-logic-diagram.png      # Power Automate flowchart
│
├── power-automate/
│   ├── ticket-routing-flow.json    # Flow template/reference
│   └── flow-variables.md           # Variable documentation
│
├── sharepoint/
│   ├── tickets-list-schema.json    # SharePoint list structure
│   ├── custom-views.md             # View configurations
│   └── permissions-model.md        # Security model
│
├── forms/
│   └── it-request-form-template.json  # Form structure
│
└── screenshots/
    └── (implementation screenshots)
```

---

## 💼 Business Value

| Challenge | Solution |
|-----------|----------|
| Scattered email requests | Centralized SharePoint repository |
| Manual ticket assignment | Automated routing by category |
| No visibility on status | Real-time SharePoint dashboard |
| Personnel dependency | Group-based team assignment |
| No audit trail | Complete SharePoint versioning |

**Impact:**
- 60% reduction in administrative overhead
- Sub-minute ticket routing
- 100% traceability
- Scalable to unlimited team members

---

## 🛠️ Technology Stack

- **Microsoft Entra ID** (Azure AD) - Identity platform
- **Active Directory** (on-premises) - Source directory
- **Azure AD Connect** - Identity synchronization
- **Power Automate** - Workflow automation
- **SharePoint Online** - Ticket repository
- **Microsoft Forms** - User interface
- **Exchange Online** - Email notifications
- **Microsoft 365 Groups** - Team collaboration

---

## 📊 Routing Logic

```
User submits form → Power Automate evaluates category

IF category contains "Hardware" → IT-Hardware team
ELSE IF category contains "Software" → IT-Software team
ELSE IF category contains "Network" → IT-Network team
ELSE → IT-ServiceDesk (default/triage)

→ Create SharePoint ticket
→ Notify user (confirmation)
→ Notify assigned team
```

---

## 📖 Documentation

| Document | Description |
|----------|-------------|
| [Setup Guide](documentation/setup-guide.md) | Step-by-step implementation (60+ pages) |
| [Flow Variables](power-automate/flow-variables.md) | Power Automate variable documentation |
| [SharePoint Schema](sharepoint/tickets-list-schema.json) | Complete list structure |
| [Custom Views](sharepoint/custom-views.md) | SharePoint view configurations |
| [Permissions Model](sharepoint/permissions-model.md) | Security and access control |
| [Forms Template](forms/it-request-form-template.json) | Microsoft Forms structure |

---

## 🎓 Skills Demonstrated

**Identity & Access Management:**
- Hybrid identity architecture (AD + Entra ID)
- Azure AD Connect configuration
- Group-based access control (GBAC)
- Microsoft 365 Groups management

**Automation & Integration:**
- Power Automate flow design
- Conditional logic and routing
- Multi-system integration
- Error handling and notifications

**IT Service Management:**
- ITSM principles (ITIL-aligned)
- Ticket lifecycle management
- SLA-ready architecture
- Categorization and prioritization

**Cloud Platform:**
- Microsoft 365 administration
- SharePoint Online configuration
- Exchange Online automation
- Power Platform development

---

## 🔮 Future Enhancements

- [ ] SLA monitoring with automatic escalation
- [ ] Microsoft Teams channel integration
- [ ] Power BI reporting dashboard
- [ ] AI-powered category detection (AI Builder)
- [ ] Approval workflows for high-priority requests
- [ ] Knowledge base integration
- [ ] Mobile app for technicians (Power Apps)

---

## 📸 Screenshots

### Microsoft Entra ID Groups
*IT support groups configured with mail-enabled collaboration*

### Power Automate Flow
*Automated routing logic with conditional branching*

### SharePoint Dashboard
*Real-time ticket tracking and team views*

---

## 🤝 Use Cases

- **Corporate IT Departments**: Internal help desk automation
- **Managed Service Providers**: Multi-client ticket routing
- **Educational Institutions**: Campus IT support
- **Healthcare**: IT support with compliance tracking
- **Government**: Structured incident management

---

## 📄 License

This project documentation is provided for educational and professional portfolio purposes.

---

## 👤 Author

Vidal Reñao Lopelo
- LinkedIn:https://www.linkedin.com/in/vidalrenao
- Email: mailto:vidal-31@hotmail.com
- Portfolio: [Your Website]
- GitHub: https://github.com/vidal-renao

---

## 🌟 Acknowledgments

This project demonstrates real-world enterprise architecture patterns used in IT Operations and Cloud Engineering teams. All configurations follow Microsoft best practices and ITIL service management principles.

---

**⭐ If you find this project helpful, please consider giving it a star!**

---

*Last Updated: December 2024*  
*Status: Production-ready*  
*Version: 1.0*
