Student Attendance & Leave Automation System
Step 1: Creating a Custom Application Using ServiceNow Studio
Steps Followed (As Implemented)
1.	Logged in to the ServiceNow PDI (Zurich release).
2.	Opened ServiceNow Studio from the Application Navigator.
3.	In Studio, clicked on the Create button and selected App.
4.	Chose the option On your own option to create Appplication
5.	On the Create App screen, entered the application details:
o	Name: Student Attendance And Leave Management
o	Description:
An application to manage student attendance and leave requests efficiently. It helps students submit requests, teachers approve them, and administrators track overall attendance.
o	Scope: Scoped application (default)
6.	Left remaining fields as default and clicked Continue / Create.
7.	After successful creation, ServiceNow Studio opened the new application workspace.

Step 2: Create Roles and Sample Users
2.1 Create Roles
1.	In the Application Navigator, search for Roles.
2.	Navigate to User Administration → Roles.
3.	Click New.
4.	Create the following roles one by one:
a. Student Role
o	Role Name: student_role
o	Purpose: Allows students to apply for leave and view their attendance.
o	Click Submit.
b. Faculty Role
o	Role Name: faculty_role
o	Purpose: Allows faculty to approve leave requests and mark attendance.
o	Click Submit.
c. HOD Role
o	Role Name: hod_role
o	Purpose: Allows HOD to approve escalated leave requests (more than 3 days).
o	Click Submit.
d. Parent Role
o	Role Name: parent_role
o	Purpose: Allows parents to view student attendance details (read-only).
o	Click Submit.
2.2 Create Sample Users (For Testing and Demo)
1.	In the Application Navigator, search for Users.
2.	Navigate to User Administration → Users.
3.	Click New.
4.	Create sample users with the following details:
a. Student User
o	Full Name: Student1
o	User ID: student1
o	Email: student1@gmail.com
o	Password: Set manually (uncheck Password needs reset)
b. Faculty User
o	Full Name: Faculty
o	User ID: faculty
o	Email: faculty@gmail.com
c. HOD User
o	Full Name: HOD
o	User ID: hod
o	Email: hod@gmail.com
d. Parent User
o	Full Name: Parent1
o	User ID: parent1
o	Email: parent1@gmail.com
5.	For each user:
o	Scroll to the Roles related list.
o	Click Edit.
o	Search and add the appropriate role (student_role, faculty_role, hod_role, parent_role).
o	Click Save.
6.	Click Submit to save each user record.

Step 3: Create the Three Important Tables
3.1 Student Table
Implementation Steps
1.	Open ServiceNow Studio.
2.	Make sure the active application is Student Attendance And Leave Management (top-right).
3.	Click Create → File.
4.	Search for Table and select it.
5.	Choose Create a blank table.
6.	Enter the following details:
o	Label: Student
o	Name: (auto-generated)
o	Application: Student Attendance And Leave Management
7.	Click Submit / Create.
8.	Open the created Student table.
9.	Go to the Columns section and add the following fields:
Field Label	Type	Reference / Notes
Student ID	String	Roll / Register number
Student Name	Reference	sys_user
Email	String	Student email
Department	String	CSE / ECE / etc
Year	Integer	Academic year
Section	String	A / B / C
Parent Email	String	Optional
Parent User	Reference	sys_user
Faculty Mentor	Reference	sys_user
10.	Click Save after adding fields.
3.2 Attendance Table
Implementation Steps
1.	In Studio, click Create → File.
2.	Search and select Table.
3.	Choose Create a blank table.
4.	Enter:
o	Label: Attendance
o	Application: Student Attendance And Leave Management
5.	Click Create.
6.	Open the Attendance table.
7.	Add the following fields under Columns:
Field Label	Type	Reference / Notes
Student	Reference	Student table
Date	Date	Attendance date
Status	Choice	Present / Absent / On Leave
Subject	String	Optional
Marked By	Reference	sys_user (Faculty)
Month	Integer	Used for monthly calculation
Year	Integer	Used for yearly calculation
8.	Click Save
3.3 Leave Request Table
Implementation Steps
1.	In Studio, click Create → File.
2.	Search and select Table.
3.	Choose Create a blank table.
4.	Enter:
o	Label: Leave Request
o	Application: Student Attendance And Leave Management
5.	Click Create.
6.	Open the Leave Request table.
7.	Add the following fields:
Field Label	Type	Reference / Notes
Student	Reference	Student table
Start Date	Date	Leave start date
End Date	Date	Leave end date
Number of Days	Integer	Manual / calculated
Type	Choice	Sick / Casual / OD / Other
Reason	String	Leave reason
Status	Choice	Requested / Faculty Approved / HOD Approved / Rejected
Current Approver	Reference	sys_user
8.	Click Save.

Step 4: Create Student Records & Link Them to Users (Using Studio Preview)
Step 4.1: Open the application in Studio
•	Go to ServiceNow Studio
•	Open your scoped application:
Student Attendance And Leave Management
Step 4.2: Open the Student table file
•	In Studio, go to Application Explorer
•	Expand Data
•	Click on Student table (the table you created using Create File → Table → Blank table)
Step 4.3: Preview the Student table
•	On the top-right corner of Studio, click Preview
•	The system opens the Student table list view in a new tab
•	Existing student records (if any) will be visible
Step 4.4: Create a new Student record
•	Click New
•	Fill in the student details:
o	Student ID
o	Email
o	Department
o	Year
o	Section
o	Parent Email
o	Parent User (select from sys_user)
o	Student Name / User (reference to sys_user)
o	Faculty Mentor (reference to sys_user)
Step 4.5: Save the record
•	Click Submit
•	The student record is now created and stored inside your scoped application table
Step 4.6: Repeat if needed
•	You can add multiple student records using the same Preview → New → Submit process
•	For demo purpose, even one student record is sufficient

Step 5: Create “Apply for Leave” Record Producer (Student Leave Form)
Step 5.1: Create Record Producer
•	Go to ServiceNow Studio
•	Open your application: Student Attendance And Leave Management
•	In the left navigation (or filter), search Record Producers
•	Navigate to:
Service Catalog → Catalog Definitions → Record Producers
•	Click New
Step 5.2: Fill Basic Record Producer Details
Fill the following fields:
Field	Value
Name	Apply for Leave
Table	Leave Request (custom table)
Catalog	Service Catalog
•	Click Submit
•	Open the record producer again for further configuration
Step 5.3: Add Variables (Student Leave Form Fields)
Scroll down to Variables section → Click New
Add variables one by one as below:
Variable Label	Type
Student	Reference (Student table)
Start Date	Date
End Date	Date
Number of Days	Single Line Text
Type	Select Box (Sick / Casual / OD / Other)
Reason	Multi-line Text
•	After adding each variable → click Submit
•	Then click New to add the next variable
 This creates the leave application form that the student fills.
Step 5.4: Map Variables to Leave Request Table Fields (Using Script)
Since you used script mapping, scroll to the Record Producer Script section and add:
current.u_start_date = Start_date;
current.u_end_date = End_date;
current.u_number_of_days = Number_of_Days;
current.u_type = Type;
current.u_reason = Reason;
•	This script transfers the entered catalog values into the Leave Request table
•	Click Update
Step 5.5: Restrict Access to Students Only
•	Scroll to Available For
•	Add role: student_role
•	Save / Update the record producer
Result of Step 5
•	Students can now apply for leave through Service Catalog
•	They don’t need to open the Leave Request table manually
•	A Leave Request record is automatically created after submission

Step 6: Build Flow Designer – Automatic Leave Approval Workflow
6.1 Open Flow Designer
1.	In the Application Navigator, search for Flow Designer.
2.	Click on Flow Designer.
3.	At the top-right corner, make sure the Application is set to:
Student Attendance And Leave Management
(Change it if required.)
6.2 Create a New Flow
1.	Click New → select Flow.
2.	Fill the details:
o	Name: Student Leave Approval Flow
o	Description: Handles leave request approval process
3.	Click Create / Submit.
6.3 Add Trigger – Leave Request Created
1.	Click Add Trigger.
2.	Select Record.
3.	Configure:
o	Table: Leave Request (custom table)
o	When: Record is created
4.	Click Done.
6.4 Add Faculty Approval Action
1.	Click Add Action.
2.	Search for Ask for Approval.
3.	Configure the action:
o	Record: Current
o	Approvers:
Select from data panel →
Leave Request → Student → Faculty Mentor
o	Approval Type: First to Respond
4.	Rename this step as Faculty Approval.
6.5 Check Faculty Approval Result
1.	Below the Faculty Approval action, click Add Action.
2.	Select If condition.
3.	Set condition:
o	Faculty Approval → State is Approved
6.6 Check Leave Duration (Number of Days)
1.	Inside the Approved IF block, click Add Action.
2.	Select If.
3.	Set condition:
o	Current → Number of Days
o	is greater than
o	3
6.7 Case A: Leave Days Greater Than 3 (HOD Approval)
If condition is TRUE (Days > 3):
1.	Add Ask for Approval action.
2.	Configure:
o	Approver: HOD user (fixed user or reference)
3.	Rename step as HOD Approval.
If HOD Approves:
1.	Add Update Record.
2.	Set:
o	Status = HOD Approved
If HOD Rejects:
1.	In Else block, add Update Record.
2.	Set:
o	Status = Rejected
6.8 Case B: Leave Days Less Than or Equal to 3
1.	In the Else block of days condition:
2.	Add Update Record.
3.	Set:
o	Status = Faculty Approved
6.9 Faculty Rejection Handling
1.	In the Else block of the Faculty Approval condition:
2.	Add Update Record.
3.	Set:
o	Status = Rejecte
6.10 Activate the Flow
1.	Click Save.
2.	Click Activate.
3.	Ensure the flow status shows Active.

Step 7: Attendance Marking (Faculty Side)
Step 7.1: Allow Faculty to Manually Add Attendance Records
1.	Open ServiceNow Studio.
2.	Open your application: Student Attendance And Leave Management.
3.	In Application Explorer, expand Data.
4.	Click on the Attendance table (the custom table created earlier).
5.	Click Preview from the top-right corner.
6.	The Attendance table list view opens in a new tab.
7.	Click New to create a new attendance record.
8.	Fill the following fields:
o	Student: Select the student record
o	Date: Select the current date
o	Status: Choose Present / Absent / On Leave
o	Marked by: Select the faculty user (example: faculty1)
o	Month: Enter the month number (example: 12)
o	Year: Enter the year (example: 2025)
9.	Click Submit to save the record.
Result
•	A new attendance record is successfully created.
•	Attendance entries are linked to:
o	Student
o	Faculty user
o	Date and status
•	This confirms that the Attendance table is functioning correctly.

Step 8: Create Reports & Dashboard (Using Data Visualization Method)
Step 8.1: Create Faculty & HOD Dashboard
1.	In the Application Navigator, search for Dashboards.
2.	Navigate to Self-Service → Dashboards.
3.	Click Create New Dashboard.
4.	Enter:
o	Name: Faculty & HOD Dashboard
5.	Click Submit.
6.	Open the newly created dashboard.
7.	Click Add New Element.
8.	Select Data Visualization.
9.	Click New Visualization.
All reports are created inside the dashboard itself using visualization configuration.
Step 8.2: Leave Requests by Status (Pie Chart)
1.	Inside New Visualization, configure:
o	Visualization Type: Pie Chart
o	Data Source: Leave Request table
2.	Set:
o	Group By: Status
3.	Leave filters empty to include all records.
4.	Click Save.
5.	Add the visualization to the dashboard.
Result
•	Displays leave requests grouped as:
o	Requested
o	Faculty Approved
o	HOD Approved
o	Rejected
Step 8.3: Attendance Summary per Student (Bar Chart)
Implementation Steps
1.	Click Add New Element → Data Visualization → New Visualization.
2.	Configure:
o	Visualization Type: Bar Chart
o	Data Source: Attendance table
3.	Set:
o	Group By: Student
o	Aggregation: Count
4.	Click Save and add it to the dashboard.
Step 8.4: Low Attendance Students (List View)
Implementation Steps
1.	Click Add New Element → Data Visualization → New Visualization.
2.	Configure:
o	Visualization Type: List
o	Data Source: Attendance table
3.	Add filter:
o	Status is Absent
4.	Group by:
o	Student
5.	Click Save and add it to the dashboard.
Result
•	Displays students with absent records.
•	Useful for identifying low attendance cases.
Step 8.5: Arrange Dashboard
1.	Drag and resize visualizations:
o	Pie chart (Leave status)
o	Bar chart (Attendance summary)
o	List view (Low attendance)
2.	Click Save Dashboard.



