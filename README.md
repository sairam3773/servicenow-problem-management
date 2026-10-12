ServiceNow Problem Management – POC

A proof-of-concept Problem Management application on the ServiceNow platform. It adds a custom table that extends the out-of-the-box problem table, with its own numbering, role-based access, form behaviour, and automation that creates a Problem Task when a problem reaches the Assess stage.

Scope: POC ITSM (scoped app) | Update set: POC_PRB_TABLE | Custom table: u_poc_problem_table

Features
Area	Implementation
Custom table	POC Problem Table (u_poc_problem_table) extends Problem → inherits Task fields, state model, CI, impact/urgency
Auto-numbering	Custom number prefix POCPRB (7 digits)
Custom field	Initial Assignment Group (u_poc_initial_assignment_group)
Business rule – Initial assignment	Before insert/update: stores the first assignment group so reassignment history isn't lost
Business rule – Problem task automation	After update when State = Assess: auto-creates a problem_task copying CI, short description and description
UI Policy	Assignment group becomes mandatory in Assess
UI Policy	Configuration Item becomes mandatory in Root Cause Analysis
UI Policy	Description shown once Short description is filled
Client script	onLoad help icons on Impact and Urgency
UI Action	Save & Exit form button (saves and returns to the list)
Security	Dedicated role u_poc_problem_table_user; ACLs for create / read / write / delete
Navigation	Application menu POC Problem Table → module POC Problem Tables
Form layout	Custom form sections, including a Notes section
Problem lifecycle used

New → Assess → Root Cause Analysis → … (state values: Assess = 102, RCA = 103)

Repository structure
├── update-set/
│   └── POC_PRB_TABLE_update_set.xml     # Importable ServiceNow update set
├── scripts/                              # Readable copies of the custom scripts
│   ├── business_rule_initial_assignment_group.js
│   ├── business_rule_problem_task_automation.js
│   ├── client_script_help_icon_prb.js
│   └── ui_policy_make_ci_mandatory_in_rca.js
└── README.md
How to install
In your ServiceNow instance go to System Update Sets → Retrieve Update Sets → Import Update Set from XML.
Upload update-set/POC_PRB_TABLE_update_set.xml.
Open the imported set (POC_PRB_TABLE) → Preview Update Set → resolve any issues → Commit Update Set.
Assign the role u_poc_problem_table_user to your test user.
Open POC Problem Table → POC Problem Tables from the application navigator.

Requires the Problem Management plugin (com.snc.problem) on the target instance.

Testing the flow
Create a new record in POC Problem Tables → note the POCPRB… number.
Set an Assignment group → check Initial Assignment Group is populated and doesn't change on reassignment.
Move State to Assess → Assignment group is mandatory; save → a Problem Task is created automatically.
Move State to Root Cause Analysis → Configuration Item becomes mandatory.
Possible improvements
Guard the problem-task business rule so it doesn't create duplicates when an Assess record is updated more than once (e.g. condition state CHANGES TO Assess).
Add a Flow Designer flow / notifications for assignment changes.
Add a Problem → Known Error → Knowledge article workflow.
Add reporting (dashboard of problems by state, priority, assignment group).
Tech

ServiceNow (ITSM – Problem Management) · Business Rules · Client Scripts · UI Policies · ACLs · GlideRecord · JavaScript
