# ServiceNow Problem Management – POC

A Proof of Concept (POC) application built on the ServiceNow platform to enhance the standard Problem Management process. The application uses a custom table extending the out-of-the-box Problem table, with custom numbering, form behaviour, business rules, and automated Problem Task creation.

## Project Overview

- **Platform:** ServiceNow
- **Application:** POC ITSM
- **Custom Table:** `u_poc_problem_table`
- **Update Set:** `POC_PRB_TABLE`
- **Number Prefix:** `POCPRB`
- **Prerequisite:** Problem Management plugin (`com.snc.problem`)

## Features

| Area | Implementation |
|---|---|
| Custom Table | POC Problem Table extending the standard Problem table |
| Auto-numbering | Custom number prefix `POCPRB` with 7-digit numbering |
| Custom Field | Initial Assignment Group (`u_poc_initial_assignment_group`) |
| Business Rule | Captures the initial assignment group during record creation and updates |
| Business Rule | Automatically creates a Problem Task when the problem reaches the Assess stage |
| UI Policy | Makes Assignment Group mandatory during Assess |
| UI Policy | Makes Configuration Item mandatory during Root Cause Analysis |
| UI Policy | Displays Description after Short Description is filled |
| Client Script | Displays help icons on Impact and Urgency fields |
| UI Action | Save & Exit button to save the record and return to the list |
| Navigation | Application menu and module for accessing POC Problem Tables |
| Form Layout | Custom form sections, including a Notes section |

## Problem Lifecycle

The project uses the following problem lifecycle stages:

`New → Assess → Root Cause Analysis → ...`

Relevant state values:
- **Assess:** `102`
- **Root Cause Analysis:** `103`

## Repository Structure

```text
ServiceNow-Problem-Management-POC/
├── update-set/
│   └── POC_PRB_TABLE_update_set.xml
├── scripts/
│   ├── business_rule_initial_assignment_group.js
│   ├── business_rule_problem_task_automation.js
│   ├── client_script_help_icon_prb.js
│   └── ui_policy_make_ci_mandatory_in_rca.js
└── README.md
```

## Installation Guide

1. Log in to your ServiceNow instance.
2. Navigate to **System Update Sets → Retrieved Update Sets**.
3. Import the update set XML file.
4. Open the imported update set named `POC_PRB_TABLE`.
5. Click **Preview Update Set** and resolve any conflicts or errors.
6. Click **Commit Update Set**.
7. Open **POC Problem Table → POC Problem Tables** from the Application Navigator.

**Prerequisite:** Ensure that the Problem Management plugin (`com.snc.problem`) is available and activated in the target instance.

## Testing the Application

### 1. Create a Problem Record
Create a new record from POC Problem Tables and verify that the number follows the `POCPRB` prefix.

### 2. Verify Initial Assignment Group
Set an Assignment Group and save the record. Verify that the Initial Assignment Group field is populated and retains the original group when the record is reassigned.

### 3. Verify Problem Task Automation
Change the problem state to **Assess** and save the record. Verify that a Problem Task is created automatically with the Configuration Item, Short Description, and Description copied from the problem.

### 4. Verify UI Policies
- During **Assess**, Assignment Group should be mandatory.
- During **Root Cause Analysis**, Configuration Item should be mandatory.
- Description should appear after entering a Short Description.

### 5. Verify Client Script and UI Action
- Confirm that help icons appear on Impact and Urgency.
- Test the Save & Exit button to verify that the record is saved and the user returns to the list.

## Future Enhancements

- Prevent duplicate Problem Task creation when an existing problem is updated multiple times in the Assess state.
- Implement assignment-change notifications using Flow Designer.
- Introduce a Problem → Known Error → Knowledge Article workflow.
- Create reports and dashboards for problems by state, priority, and assignment group.

## Technologies Used

- ServiceNow ITSM – Problem Management
- Business Rules
- Client Scripts
- UI Policies
- UI Actions
- GlideRecord
- JavaScript
- Update Sets

## Learning Outcomes

- Created a custom table extending a standard ServiceNow table.
- Configured custom auto-numbering.
- Implemented business rules for data handling and automation.
- Configured UI Policies for dynamic form behaviour.
- Developed a client script for field-level assistance.
- Used update sets to package and migrate ServiceNow customizations.

---

**Project Type:** ServiceNow ITSM POC  
**Focus Areas:** Problem Management, Automation, Form Customization, and Application Development
