# New Hire Onboarding Process Automation
## Technical Implementation & Project Documentation

> **Classification:** Confidential / Internal Project Documentation  
> **Platform:** Kissflow + External Rex E-Business Onboarding Portal  
> **Current Status:** User Acceptance Testing (UAT)  
> **Environment:** Development / Test  
> **Production Status:** Not yet deployed

---

## Table of Contents

- [1. Project Overview](#1-project-overview)
- [2. Business Problem](#2-business-problem)
- [3. Project Objectives](#3-project-objectives)
- [4. Scope](#4-scope)
- [5. Solution Architecture](#5-solution-architecture)
- [6. End-to-End Process Flow](#6-end-to-end-process-flow)
- [7. Roles and Responsibilities](#7-roles-and-responsibilities)
- [8. Workflow Implementation](#8-workflow-implementation)
- [9. Dynamic Supervisor Assignment](#9-dynamic-supervisor-assignment)
- [10. Integrations](#10-integrations)
- [11. External Onboarding Form](#11-external-onboarding-form)
- [12. Webhook & Data Synchronization](#12-webhook--data-synchronization)
- [13. Email Automation](#13-email-automation)
- [14. Conditional Routing](#14-conditional-routing)
- [15. Document Signing & Generation](#15-document-signing--generation)
- [16. Permissions & Access Control](#16-permissions--access-control)
- [17. Audit & Control Design](#17-audit--control-design)
- [18. User Acceptance Testing](#18-user-acceptance-testing)
- [19. Current UAT Focus Areas](#19-current-uat-focus-areas)
- [20. Exception Handling](#20-exception-handling)
- [21. Production Readiness](#21-production-readiness)
- [22. Current Project Status](#22-current-project-status)
- [23. Repository Structure](#23-repository-structure)
- [24. Security & Confidentiality](#24-security--confidentiality)
- [25. Future Work](#25-future-work)
- [26. Author](#26-author)

---

# 1. Project Overview

The **New Hire Onboarding Process Automation** project is an end-to-end employee onboarding workflow developed primarily within the **Human Capital Management application on Kissflow**.

The solution coordinates the activities required to onboard a new employee, including:

- HR request initiation
- New-hire data collection
- Automated email communication
- External-form submission
- Data synchronization
- HR processing
- Management approval
- Departmental onboarding activities
- Supervisor approval
- IT provisioning
- Conditional location and equipment routing
- Final HR review
- New-hire document signing
- Automated document generation
- Completion tracking
- Buddy communication

The solution integrates the internal Kissflow workflow with an external employee-facing onboarding portal, Microsoft Outlook, JavaScript logic, webhooks, lookup/reference data, and Kissflow document templates.

---

# 2. Business Problem

The employee onboarding process requires coordination between several business functions.

A single onboarding request may involve:

- Human Capital Management
- Management approvers
- Staff Welfare
- BCC
- Facilities
- Technology leadership
- The employee's supervisor
- IT
- The new hire
- Additional operational or approval functions where required

Managing these activities through disconnected emails, manual follow-ups, spreadsheets, and verbal communication creates several risks:

- Tasks may be missed
- Approvals may be delayed
- Responsibilities may be unclear
- Employee information may be duplicated
- Provisioning activities may happen out of sequence
- HR may have limited visibility into onboarding progress
- Audit evidence may be fragmented
- Manual communication may contain incorrect employee information

The project was designed to create a controlled digital workflow where the onboarding request becomes the central record for the employee's onboarding journey.

---

# 3. Project Objectives

The project aims to:

1. Digitize the employee onboarding lifecycle.
2. Create a single onboarding process instance for each new employee.
3. Collect employee information through a structured external form.
4. Synchronize externally submitted information back into Kissflow.
5. Assign tasks automatically to the appropriate business functions.
6. Support role-based approvals and segregation of responsibilities.
7. Allow independent departmental onboarding tasks to run in parallel.
8. Dynamically assign the employee's supervisor based on department.
9. Handle location-dependent and equipment-dependent routing.
10. Automate onboarding email communication.
11. Generate required onboarding documents automatically.
12. Maintain traceable workflow, approval and integration evidence.
13. Provide HR with visibility into onboarding status.
14. Establish a structured UAT and production-readiness process.

---

# 4. Scope

The implemented solution covers the following areas.

### Request Initiation

HR creates a new onboarding request and provides the new hire's email address where required.

### Initial Communication

The system generates a process-specific onboarding link and sends it to the new hire.

### Employee Data Collection

The new hire completes an external multi-step onboarding form.

### Data Synchronization

Submitted information is sent back into the correct Kissflow onboarding instance through a webhook integration.

### Internal HR Processing

HR reviews the synchronized information and supplies internal employee information such as:

- Department
- Unit
- Job title
- Office location
- Resumption date
- Buddy
- Employment type
- Required access controls
- Equipment or other specifications

### Approval & Provisioning

The request is routed through management, operational, supervisor and IT activities.

### Final Review

HR confirms that the required onboarding activities have been completed.

### Document Signing

The new hire reviews and signs the required onboarding documents.

### Completion

Generated documents are retained with the process record and the workflow reaches Completed status.

---

# 5. Solution Architecture

The solution contains several connected components.

| Component | Responsibility |
|---|---|
| Kissflow Human Capital Management App | Hosts the onboarding workflow, tasks, approvals, permissions and process record |
| New Hire Request Page | HR-facing initiation and monitoring interface |
| New Hire Approval / Task Views | Provides role-specific assigned and completed task views |
| External New Hire Portal | Collects onboarding information directly from the employee |
| Kissflow Integrations | Connects workflow events to automated actions |
| JavaScript | Builds dynamic values such as the employee-specific onboarding URL |
| Webhook | Receives employee-submitted information from the external portal |
| Microsoft Outlook | Delivers automated onboarding and Buddy emails |
| Lookup / Reference Data | Supports dynamic assignment and employee-related workflow decisions |
| Kissflow Document Templates | Generates official onboarding documents |
| UAT Workbook | Defines business-readable test scenarios |
| Audit & Control Documentation | Documents controls, evidence and production-readiness considerations |

---

# 6. End-to-End Process Flow

The implemented core workflow can be represented as:

```text
HR Initiates Request
        │
        ▼
Initial New-Hire Email
        │
        ▼
New Hire Opens Unique Link
        │
        ▼
External Onboarding Form
        │
        ▼
Webhook Submission
        │
        ▼
Update Correct Kissflow Instance
        │
        ▼
HR Selects Employment Details
        │
        ▼
Head HCM Review / Approval
        │
        ▼
┌─────────────────────────────────────────────┐
│       Parallel Department Activities        │
│                                             │
│  Welfare   BCC   Facilities   CDIO / Tech   │
└─────────────────────────────────────────────┘
                        │
                        ▼
             Supervisor Review
                        │
                        ▼
                 IT Provisioning
                        │
               ┌────────┴────────┐
               │                 │
         Conditional        Conditional
       Office Routing      Equipment Routing
               │                 │
               └────────┬────────┘
                        ▼
                 Final HR Review
                        │
                        ▼
            New Hire Signs Documents
                        │
                        ▼
            Automated PDF Generation
                        │
                        ▼
                   Completed
                        │
                        ▼
               Buddy Notification
