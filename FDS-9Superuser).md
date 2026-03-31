# FDS - Latest**Executive Summary**

The Behavioral Incident & Counseling Management module in our App streamlines the process of documenting, reviewing, and managing behavioral incidents and counseling sessions with the purpose making them billable to MediCal in California per CYBHI guidelines. It empowers classified staff to submit incident reports with AI assistance compliant with CYBHI guidelines, provides counselors with tools to manage sessions and review incidents, and gives administrators oversight through reporting and analytics.

## **Introduction**

This Functional Design Specification (FDS) outlines the core features, user flows, and technical requirements for the Behavioral Incident & Counseling Management App. The application is designed to be fully compliant with FERPA, HIPAA, and CYBHI guidelines.

**App Name**

Behavioral Incident & Counseling Management App

**Purpose:**

- Enable classified staff to document and refine incident reports with AI assistance compliant with CYBHI guidelines.
- Empower counselors to manage sessions, view incidents, document notes, and schedule follow-ups.
- Provide admins with oversight of incidents and sessions across users.

**Target Users:** Classified staff, Certified Health Workers, Counselors, and administrative personnel in school behavioral health settings.

# **GLOSSARY**

The below table contains terminology used in the FDS and their related definitions.

| **Term** | **Definition** |
| --- | --- |
| **Admin Module  <br>** | The administrative interface that allows authorized personnel to manage user accounts, oversee incidents, and monitor counseling sessions. Provides access to reports, user activity, and compliance management tools. |
| --- | --- |
| **AI-Assisted Reporting** | Automated support using language models to guide users through incident documentation, ensuring accuracy, CYBHI compliance, and structured output. |
| --- | --- |
| **Firebase Auth** | The identity provider used for secure authentication and authorization, implementing OAuth2, OIDC, and PKCE protocols for login management. |
| --- | --- |
| **Behavioral Incident** | Any event involving student behavior that requires documentation, review, or counseling intervention. Examples include conflicts, policy violations, or concerning emotional displays. |
| --- | --- |
| **BIRP Format** | A structured clinical note format with four parts: **Behavior** (observable actions, statements, mood), **Intervention** (what the provider did), **Response** (how the client reacted), and **Plan** (next steps, homework, follow‑up). |
| --- | --- |
| **Burger Menu  <br>** | The navigation menu (typically a three-line icon) that provides users access to app settings and additional options. |
| --- | --- |
| **CHW (Certified Health Worker)** | A certified staff member authorized to handle health-related behavioral reports, coaching documentation, and follow-up interventions. Can bill sessions. |
| --- | --- |
| **Classified Staff** | Non-certified school employees (e.g., aides, security personnel, supervisors) who can report behavioral incidents and initiate coaching or counseling workflows. Cannot bill sessions. |
| --- | --- |
| **Coaching/Counseling Draft** | A preliminary counseling or coaching record automatically generated for each participant in an incident to support individualized follow-ups before final submission. |
| --- | --- |
| **Counselor** | A Counselor is authorized to handle health-related behavioral reports, counseling documentation, and follow-up interventions. Can bill sessions. |
| --- | --- |
| **CYBHI** | California Youth Behavioral Health Initiative; governs documentation, redaction, and reporting compliance requirements for student behavioral and counseling data. |
| --- | --- |
| **Dashboard  <br>** | The main user interface shows report summaries, pending incidents, counseling sessions, and quick access to key features. |
| --- | --- |
| **Draft Report  <br>** | A saved but unsubmitted version of an incident or counseling report that can be edited or deleted prior to submission. |
| --- | --- |
| **FERPA  <br>** | Family Educational Rights and Privacy Act; ensures protection and confidentiality of student education records and identifiable data. |
| --- | --- |
| **HIPAA  <br>** | Health Insurance Portability and Accountability Act; governs confidentiality and security standards for health-related data in the system. |
| --- | --- |
| **Incident Report** | A formal record of a behavioral event submitted by classified staff, reviewed by counselors or administrators for analysis, intervention, or follow-up. |
| --- | --- |
| **LLM (Large Language Model)  <br>** | An AI component used to generate structured reports, summaries, or recommendations from transcribed and redacted incident data. |
| --- | --- |
| **MongoDB Atlas** | The secure, cloud-hosted database storing anonymized and encrypted incident, counseling, and user records with role-based access control. |
| --- | --- |
| **Multifactor Authentication (MFA)** | A security measure requiring multiple verification factors (such as password and token) for admin-level access. |
| --- | --- |
| **PII Redaction** | The automated process of detecting and anonymizing personally identifiable information (names, emails, student IDs) using tools such as Microsoft Presidio or Google DLP. |
| --- | --- |
| **RAG (Retrieval-Augmented Generation)  <br>** | AI pipeline that enriches generated reports with context from past records, ensuring consistency and accuracy in report production. |
| --- | --- |
| **RBAC (Role-Based Access Control)** | Access control system ensures that users can only view or modify data appropriate to their roles (e.g., Staff, Counselor, Admin). |
| --- | --- |
| **SOAP Format  <br>** | Structured reporting format including **Subjective**, **Objective**, **Assessment**, and **Plan** elements, used for both incident and counseling documentation. |
| --- | --- |
| **Speech-to-Text  <br>** | AI function that converts recorded audio into text for further processing and redaction. |
| --- | --- |
| **User Statuses** | **Approve:** Admin approves the account and system access is activated.<br><br>**Reject:** Admin rejects the account request and access is not granted.<br><br>**Pending:** The account request is under review and awaiting admin action. |
| --- | --- |
| **Touch ID/Face ID** | Biometric authentication methods supported for secure user login in android and iOS devices respectively. |
| --- | --- |
| **VPC (Virtual Private Cloud)** | A secure, isolated cloud network hosting the application backend and database with no direct public access. |
| --- | --- |
| **Whisper** | A self-hosted speech-to-text engine for converting user audio recordings into text within a private cloud environment. |
| --- | --- |

# **User Personas**

The system is designed to support multiple user roles, each with clearly defined responsibilities and access levels. These personas ensure role-based workflows, data security, and compliance with operational and regulatory requirements. The following user personas represent the primary stakeholders who interact with the system.

**Classified Staff  
**Classified Staff are non-certified school personnel responsible for reporting incidents observed during daily activities, such as Campus Safety & Security, Instructional Support, Operations & Facilities, and Office & Administrative Support Personnel. Their access is limited to incident reporting through the Incident Bot, with reports generated in SOAP format and submitted to the Admin for review.

**Certified Classified Staff / Community Health Worker (CHW)  
**Certified Classified Staff or CHWs are authorized personnel who can report incidents and provide follow-up coaching. In addition to incident reporting, they can conduct coaching sessions within the same workflow, generate BIRP format coaching reports with billing codes, and submit them to the Admin for approval.

**Counselor  
**Counselors are licensed or certified professionals responsible for conducting counseling sessions and managing assigned or self-created incidents. They can initiate counseling from existing incidents or new incident reports, generate counseling documentation in BIRP format with billing codes, and respond to revision requests from the Admin.

**Admin  
**Admins oversee the overall system operations, including incident, coaching, and counseling workflows. They are responsible for reviewing and approving submissions, requesting revisions, assigning coaching or counseling sessions, managing users, and controlling system access and security keys to ensure compliance and governance.

## **1.1. Actor:** Classified Staff

**Overview**

This workflow describes the complete process for Classified Staff to sign into the system, report an incident using the Incident Bot, generate a standardized SOAP (Date, Time, Subjective, Objective, Assessment, Plan, Participants) format incident report, and submit the report to the Admin for review and action, handling Admin revision requests, and resubmitting reports for approval. Classified Staff users are limited to incident reporting only and do not initiate coaching or counseling sessions.

**Preconditions**

- Classified Staff users are registered and authorized in the system.
- The user has valid login credentials.
- Incident Bot service is available and operational.
- Admin approval workflow is active.

**Trigger**

Classified Staff user signs into the system.

**Primary Workflow**

**Step 1:** Sign In

1.  Classified Staff users sign into the system using valid credentials.

**Step 2:** Report Incident

1.  After successful login, the user initiates incident reporting through the Incident Bot.
2.  Incident Bot prompts the user to provide incident details (Date, Time, Location, Subjective, Objective, Assessment, Plan, and Participants.).
3.  The system validates the entered incident information for completeness (SOAP Format report) and correctness.

**Step 3:** SOAP Report Generation

1.  System generates the incident report in **SOAP format**, including:
    - Date
    - Time
    - Location
    - Subjective
    - Objective
    - Assessment
    - Plan
    - Presumed Participants based on audio/written input

**Step 4:** Submit Incident

1.  User selects the correct student ID from the provided list of students that was preloaded into the app. Users will see the name(s) of any participants as relayed via audio/text transmission, however the report cannot be processed until the user clicks each participant, goes to the subsequent screen, and adds the student ID for each participant.
2.  Classified Staff reviews and edits as necessary the generated SOAP incident report.
3.  Classified Staff submits the incident report to the Admin.

**Postconditions**

- The incident report is successfully stored in the system.
- Incident report status is set to **Submitted**.
- Incident report is available to the Admin for review, or can be sent back to Classified Staff for updates.
- Audit logs are generated for the submission.

**Business Rules**

- Classified Staff can only create and submit incident reports.
- Incident reports must follow **SOAP format**.
- Classified Staff cannot edit the report after submission, unless specified by Admin and reassigned.
- Future-dated incidents are not allowed.
- Submission is blocked until all mandatory incident fields are completed.

**Status Lifecycle**

Draft → Submitted → Reviewed (Admin) > Completed/Reassigned

**Assumptions**

- Classified Staff users are trained to accurately report incidents.
- Classified Staff does not participate in counseling or coaching workflows.  
    

## **1.2. Actor:** Certified Classified Staff / Community Health Worker (CHW)

**Overview**

This workflow describes the complete lifecycle for Certified Classified Staff or CHW users, including signing in, reporting incidents through the Incident Bot, generating SOAP reports, initiating coaching within the same chat or creating coaching sessions for later, selecting or generating coaching drafts, producing structured coaching reports with billing codes, handling Admin revision requests, and resubmitting reports for approval. Coaching sessions set in the draft stage should be visible to the Admin.

**Preconditions**

- Certified Classified Staff / CHW is registered and authorized.
- User has valid login credentials.
- Incident Bot and Coaching Bot services are available.
- Role based assignments are made based on the user certification and capabilities. Users are certified and permitted to generate coaching reports and billing codes.
- Admin approval workflow is active.

**Trigger**

Certified Classified Staff / CHW signs into the system.

**Primary Workflow**

**Step 1:** Sign In

1.  Certified Classified Staff / CHW signs into the system.

**Step 2:** Report Incident

1.  User initiates incident reporting through the Incident Bot.
2.  Incident Bot collects incident details (_Section 2.3.2.3)_
3.  System generates the incident report in **SOAP format (**_Section 1.1.3_)
4.  Incident report is submitted to the Admin.

**Step 3:** Coaching Starts in Same Chat

1.  After incident reporting, coaching details are requested from user from within the same chat. These requests focus on completing the coaching notes in the correct BIRP format (Behavior, Intervention, Response, Plan) and coaching session details (Date, Time, Length) and automatically starts in the same chat session.

**Step 4:** Coaching Draft Generation

1.  System generates coaching drafts:
    - Pre-populated draft - prepopulated when sufficient information was provided during initial incident reporting.
    - Empty draft - created regardless if coaching was provided or not, however no details on the coaching were included in initial submission. The number of empty drafts depends on the number of students involved.
2.  Coaching drafts can be generated manually as well by CHW and are not just system generated. One student can have multiple sessions.
3.  User can click and select any draft to proceed.

**Step 5:** Start Coaching

1.  Coaching session starts based on the selected draft.
2.  Coaching Bot guides the coaching interaction as per _section 2.4.2_.

**Step 6:** Coaching Report Generation

1.  Coaching Bot generates a coaching report in a predefined BIRP format, including:
    - Coaching notes
    - Required structure BIRP (Behavior, Intervention, Response, Plan)
    - Applicable CYBHI billing code based on the session length, participants and service provider.

**Step 7:** Submit Coaching Report

1.  The user reviews the coaching report draft and edits as necessary.
2.  The coaching report is submitted to the Admin for approval.

**Revision Workflow (Admin → CHW Loop)**

**Step 8:** Admin Requests Revision

1.  Admin reviews the coaching report.
2.  Admin requests a revision.
3.  Coaching session status is updated to **Revision Requested**.

**Step 9:** View Revision-Requested Coaching Session

1.  Certified Classified Staff / CHW signs in.
2.  User views coaching sessions marked as **Revision Requested**.
3.  User opens the revision-requested coaching session.

**Step 10:** Update Coaching Session

1.  User reviews Admin feedback.
2.  User updates coaching details:
    - Via same chat session from earlier coaching bot chat.
    - Or updates manually via text input
3.  If required, user updates related incident details through the Incident Bot.
4.  System regenerates the incident **SOAP report** if incident data is modified.

**Step 11:** Regenerate Coaching Report

1.  Coaching resumes in the same chat originally used to generate coaching.
2.  Coaching Bot regenerates the coaching report with updated content and billing code.

**Step 12:** Resubmit for Approval

1.  User reviews the updated coaching report.
2.  User submits the updated coaching report to the Admin for approval.

**Postconditions**

- Coaching report status is updated to **Pending Admin Approval**.
- Previous versions are retained for audit and compliance.
- Admin is notified of resubmission.
- Audit logs are generated for all actions.

**Business Rules**

- Incident reports must follow **SOAP format**.
- Coaching reports must follow the predefined **BIRP** coaching template.
- Billing codes can be generated only by certified users.
- Once submitted, only reports with status **Revision Requested** are editable.
- Approved reports are locked from further edits.
- Future-dated incidents or coaching sessions are not allowed.
- Submission is blocked until all mandatory fields are completed.

**Status Lifecycle**

Draft → Submitted → Revision Requested → Resubmitted → Approved / Rejected

**Assumptions**

- Certified Classified Staff / CHWs are trained to conduct coaching sessions.
- Admin provides clear revision feedback.
- Users address revisions without creating duplicate records.

## **1.3. Actor:** Counselor

**Overview**

This workflow describes the complete lifecycle for Counselor, including signing in, reporting incidents through the Incident Bot, generating SOAP reports, initiating counseling sessions within the same chat or creating sessions for later, selecting or generating counseling session drafts, producing structured counseling reports with billing codes, handling Admin revision requests, and resubmitting reports for approval. Sessions set in draft stage should be visible to the Admin.

**Preconditions**

- Counselor is registered and authorized in the system.
- Counselor has valid login credentials.
- Incident Bot and Counseling Bot services are available.
- Admin approval workflow is active.

**Trigger**

Counselor signs into the system.

**Primary Workflow (Full Diagram Flow)**

**Step 1:** Sign In

1.  Counselor signs into the system using valid credentials.

**Step 2:** View Incidents

1.  After successful login, Counselor can view incidents:
    - Incidents allocated by Admin
    - Incidents created by the Counselor

**Step 3A:** Start Counseling from Existing Incident

1.  Counselor selects an existing incident.
2.  Counselor starts a counseling session from the selected incident.
3.  Counseling session begins in the system.

**Step 3B:** Report New Incident (Alternate Path)

1.  Counselor reports a new incident through the Incident Bot.
2.  Incident Bot collects incident details.
3.  System generates the incident report in **SOAP format**.
4.  Incident report is submitted to Admin.
5.  Admin can send the report for a revision as in the coaching role.

**Step 4:** Counseling in Same Chat

1.  After incident reporting, counseling starts automatically in the same chat session. However, Counseling can be initiated at any point during the chat session. Counseling-related details may be shared in any message and are not restricted to starting only after the incident report is completed.
2.  Counseling Bot guides the reporting of the counseling session.

**Step 5:** Counseling Report Generation

1.  Counseling Bot generates a counseling report in **BIRP format**, including:
    - Behaviour
    - Intervention
    - Response
    - Plan
    - Applicable billing code

**Step 6:** Submit Counseling Report

1.  Counselor reviews the generated counseling report and updates as necessary.
2.  Counselor submits the counseling report to Admin for approval.

**Revision Workflow (Admin → Counselor Loop)**

**Step 7:** Admin Requests Revision

1.  Admin reviews the counseling report.
2.  Admin requests a revision.
3.  Counseling session status is updated to Revision Requested.

**Step 8:** View Revision-Requested Session

1.  Counselor signs in.
2.  Counselor views counseling sessions marked as Revision Requested.
3.  Counselor opens the revision-requested counseling session.

**Step 9:** Update Counseling Session

1.  Counselor reviews Admin feedback.
2.  Counselor updates counseling details:
    - Via same chat session OR
    - Via text input
3.  If required, Counselor updates or reports incident details through the Incident Bot.
4.  System regenerates the incident SOAP report (if modified).

**Step 10:** Regenerate Counseling Report

1.  Counseling resumes in the same chat earlier used to generate original counselling reports.
2.  Counseling Bot regenerates the counseling report in **BIRP format** with billing code.

**Step 11:** Resubmit for Approval

1.  Counselor reviews the updated counseling report.
2.  Counselor resubmits the updated report to Admin for approval.

**Postconditions**

- Counseling report status is updated to **Pending Admin Approval**.
- Previous versions are retained for audit purposes.
- Admin is notified of resubmission.
- Audit logs are recorded for all actions.

**Business Rules**

- Incident reports must follow **SOAP format**.
- Counseling reports must follow **BIRP format**.
- Only reports with status **Revision Requested** are editable.
- Approved reports are locked from further editing.
- Billing codes are generated only by authorized Counselors.
- Future-dated incidents or counseling sessions are not allowed.
- One student can have multiple sessions.

**Status Lifecycle**

Draft → Submitted → Revision Requested → Resubmitted → Approved / Rejected

**Assumptions**

- Counselors are certified and trained to conduct counseling.
- Admin provides clear revision feedback.
- Counselor addresses revisions without creating duplicate records.

## Counseling drafts are automatically created if the user reports counseling was provided. Detailed counseling details in BIRP format will be added in the draft if the user provides sufficient detail on both the incident and the counseling. Alternatively, if only the incident report is created, and the user mentions counseling was provided but does not provide details, a blank counseling draft will be created as a placeholder. The placeholder draft can then be completed in a separate chat. **1.4. Actor:** Admin

**Overview**

This workflow defines the complete administrative responsibilities within the system. The Admin signs in to oversee all submitted incidents, coaching reports, and counseling sessions. The Admin reviews, approves, rejects, or requests revisions, assigns coaching or counseling sessions based on incidents, and manages system users and security access. The Admin also has the ability to generate reports for all users under their purview and for the institution at large, including data on Incident Reporting, Coaching/Counseling, Financial & Billing, School-Level, Outcomes & Impacts, Staffing & Performance, and Compliance & Audit-Ready, System Health & Operational Monitoring/

**Preconditions**

- Admin user is registered and authorized.
- Admin has valid credentials.
- Incident, Coaching, Counseling, and User Management modules are active.
- Reports and/or users exist in the system.

**Trigger**

Admin signs into the system.

**Complete Functional Flow**

**Step 1:** Sign In

1.  Evolo AI assigns Admin role based on institution roles.
2.  Admin signs into the system using valid credentials.

**Step 2:** Incident Oversight

1.  Admin navigates to the **Incidents** module.
2.  Admin views all submitted incident reports.
3.  Admin reviews incident details and SOAP-formatted reports.
4.  Admin may:
    - View incidents
    - Assign a new coaching session from an incident
    - Assign a new counseling session from an incident

**Step 3:** Coaching Approval Workflow

1.  Admin navigates to the **Coaching** module.
2.  Admin views submitted coaching reports.
3.  Admin reviews coaching documentation and billing information.
4.  Admin may:
    - Approve the coaching report
    - Reject the coaching report
    - Request a revision and must provide a reason in the notes.
5.  If revision is requested, the coaching report status is updated to **Revision Requested** and returned to the originating user with the notes from the admin.

**Step 4:** Counseling Session Approval Workflow

1.  Admin navigates to the **Counseling Sessions** module.
2.  Admin views submitted counseling session reports.
3.  Admin reviews counseling documentation and billing codes.
4.  Admin may:
    - Approve the counseling session
    - Reject the counseling session
    - Request a revision
5.  If revision is requested, the counseling session status is updated to **Revision Requested** and returned to the Counselor.

**Step 5:** Revision Review Loop

1.  Admin monitors resubmitted coaching and counseling reports.
2.  Admin reviews updated submissions.
3.  Admin takes final action:
    - Approve
    - Reject

**Step 6:** User Management

1.  Admin navigates to the **User Management** module.
2.  Admin uploads user lists, including:
    - Student lists get uploaded every quarter. In cases where a student not included in the system is involved in an incident or receives coaching or counseling, the admin can add them manually.
    - Additionally, whenever an admin needs to upload a student list, a **secure, one-time clickable link** will be sent to their **registered email address**. This link will be **valid for 15 minutes only** and will **expire immediately after a single use**. The student list can be uploaded only through this secure link.
    - Classified Staff
    - Counselors
3.  Admin can add or remove users.
4.  Admin provides security keys to Classified Staff and Counselors for system access.

**Postconditions**

- Incident, coaching, and counseling reports have final statuses applied.
- Approved reports are locked and finalized.
- Revision-requested reports are routed back to users.
- Assigned coaching or counseling sessions are visible to relevant roles.
- User records and security keys are updated if the user changes their security key.
- All Admin actions are logged for audit and compliance.

**Business Rules**

- Only Admin users can approve, reject, or request revisions.
- Approved reports cannot be edited further.
- Billing data becomes final only after Admin approval.
- Security keys must be unique and securely generated.
- All Admin actions must be auditable.

**Status Lifecycle**

Draft → Submitted → Revision Requested → Resubmitted → Approved / Rejected

**Assumptions**

- Admin is responsible for compliance, governance, and quality assurance.
- Users respond to revision requests before resubmission.
- Admin approval decisions are final.
- A school can have multiple admins and this needs to be role based.

# **2\. Functional Flows**

### 2.1. Splash & Onboarding

- Users can view splash and onboarding screens introducing the app and its capabilities.
- The first screen displays a clean branded title welcoming users to the Incident Report module.
- The second screen provides a brief overview emphasizing the app’s role in supporting incident tracking and behavioral health coordination.
- The third screen highlights ease of recording incidents with integrated AI prompts ensuring CYBHI guideline compliance.

### 2.2. User Account Management

#### 2.2.1. Sign Up

All users can create an account with:

- - Institution Email address
    - Password creation and confirmation
    - Full name
    - Employee ID
    - Role assignment which will be validated from Administrator listing.
    - The system verifies email and sends a confirmation link.
    - Upon confirmation, the account is activated.

**Description:**These screens guide users through account creation, legal agreements acceptance, and administrative approval processes.

The first screen displays the sign-up form, where users enter personal details including name, institutional email, employee ID, role, and password to register for access.

The second screen presents the Terms of Service and Privacy Policy, outlining the data processing consent and platform usage agreement, with users required to read and agree before continuing.

The final screen provides confirmation that the account request has been submitted for administrative approval, instructing users to contact their administrator to complete activation and begin using the app.

 

#### 

#### 2.2.2. Sign In

- Staff can log in with email and password.
- Password reset supported via verified email.
- The IOS users can login using Face ID
- Android Users can login using Touch ID
- Sign-in with Face ID will bypass the Security Key requirement after the user’s initial verified login.
- Multi-factor authentication optional in future phases.

**Description:**These screens guide the user through the login interface, offering options such as “Remember Password” and biometric authentication (Face ID / Touch ID) for secure and convenient access. Biometric login enhances usability by reducing repeated credential entry while maintaining security standards.

The first screen is for Android users and prompts users to enter their email and password to access the platform, with additional options to remember their password, reset forgotten credentials, or sign in using Touch ID for added convenience.

The second screen for IOS users demonstrates the login process in action, showing the fields filled and the “Remember password” option selected, while providing quick access to sign-up for those without an account.

**First-Time Login & Security Key Setup Flow**

**Classified Staff & Counselor**

All users must complete a one-time security key setup during their first login:

- Email and password sign-in
- One-time security key (provided by School Admin)
- Creation of personal security key
- Confirmation of personal security key

The system validates the one-time security key and allows the user to create their own security key for future access.

**Description:**

These screens guide users through secure first-time access and security key setup.  
After successful login with email and password, new users are prompted to enter a one-time security key issued by the School Admin to verify authorization. Once validated, the user is directed to a screen where they create and confirm their own personal security key. This personal key replaces the one-time key and is required for all future logins. After successful setup, the user gains full access to the dashboard and the one-time key is permanently disabled.

**Update Security Key Setup**

Users can update their security key at any time through Settings:

- Access via Burger Menu
- Navigate to Settings
- Select Update Security Key
- Enter current security key
- Create and confirm a new security key

The system validates the existing key before allowing the update.

**Description:**

These screens allow users to update their security key after initial account setup.  
From the Burger Menu, users can access the Settings screen and select the Update Security Key option. The system prompts the user to enter their current security key for verification, followed by entering and confirming a new security key of their choice. Upon successful validation, the new security key replaces the old one and a confirmation message is displayed. This ensures users can securely manage and update their access credentials at any time.

### 

**First-Time Login & Security Key Setup Flow**

**Admin**

Admin must complete a one-time security key setup during their first login:

- Email and password sign-in
- Creation of main security key
- Confirmation of main security key

The system validates both security keys.

**Description:**

These screens guide users through setting up their main security key. After successfully logging in with their email and password, new users are prompted to create a one-time personal key and confirm it. This key will be used for future logins. Once the setup is complete, users gain full access to the dashboard.

**First-Time Login & Backup Key Setup Flow**

**Admin**

Admin must complete back up key setup during their first login:

- The admin logs in using their registered email and password.
- The admin creates a one-time personal key for future logins.
- The system verifies the main key.
- Backup Key Setup (Optional but Recommended) The admin is prompted to create a backup key to ensure account recovery if the main key is lost.
- The system validates the backup key.
- Once both keys are set up (or the backup key is skipped), the admin gains full access to the dashboard.

**Description:**

These screens guide admins through the first-time login security setup. After signing in with their email and password, admins create and confirm a main security key, which will be used for all future logins. Immediately afterward, they are prompted to optionally create a backup key to protect their account. Once the setup is complete, the admin can access the dashboard with full functionality.

**Update Security Main Key & Backup Key Setup**

Users can update their security key at any time through Settings:

- Access via Burger Menu
- Navigate to Settings
- Select Update Security Key/backup key
- Enter current security key
- Create and confirm a new security key
- You can create up to 3 backup keys.

**Forgot Main Key:**

- If a user forgets their main security key, they can select “Forgot Key”.
- They can then use a backup key from a dropdown list to log in.
- After logging in with a backup key, the system prompts: “Do you want to reset your main key?”
- Users can choose to reset the key immediately or skip for now.

**Description:**

These screens allow users to update or reset their main security key and backup keys. Users can access this feature through Settings in the Burger Menu or directly from their Profile. For updates, the current security key must be verified before creating and confirming a new key. Users can create up to three backup keys to ensure account recovery and secure access.

If the main key is forgotten, users can log in with a backup key and optionally reset the main key immediately or skip the reset. This ensures continuous secure access while providing a recovery option.

### 2.2. Dashboard Screens

#### 2.2.1 Classified Staff

- Classified staff are redirected to the dashboard after successful sign-in.
- The dashboard displays total incidents reports submitted and total students helped.
- Students Helped and Total Incidents metric cards are clickable.
- A “+” button allows classified staff to create a new incident report.
- Past incident reports are displayed below the metric cards.
- Incident reports are listed in chronological order.
- Each incident card displays the incident title.
- Each incident card shows the date, day, and time of the incident.
- Each incident card displays the report status (Draft, Generated, Submitted).
- Each incident card includes a Participants (count) section.
- Clicking the Participants section opens a pop-up with the list of participant names.
- An info (i) icon is available on each incident card.
- Clicking the info (i) icon opens a pop-up showing incident details including title, student names, grades, and student IDs.

**Description:** After successful sign-in, classified staff are redirected to the main dashboard where they can view a summary of their incident-related activities. The dashboard displays key metrics, including the total number of reports submitted and the total number of students helped. The Students Helped and Total Incidents cards are clickable. From this screen, staff can initiate a new incident report by selecting the “+” button, which opens the incident creation interface. Below the metric cards, the dashboard displays a list of past incident reports in chronological order. Each incident card shows the incident title, date, day, and time, followed by a Participants (count) section. Each card also displays the report status (Draft, Generated, Submitted). An info (i) icon is available on the right side of each incident card; clicking it opens a pop-up displaying incident details such as incident title, involved students, their grades, and student IDs. Clicking on the Participants section opens a pop-up displaying a list of participant names.

#### 2.2.2. Certified Health Workers (CHW)

After successful sign-in, Certified Health Workers (CHWs) are directed to the main dashboard where they can view key metrics, including the total number of coaching sessions, the total number of incident reports, and the number of students helped. The dashboard also displays the total hours of sessions provided so far. Incident reports are shown in chronological order, with each report displaying the number of participants and number of coaching sessions. The "+" button allows CHWs to easily create a new incident report, enabling them to quickly document incidents as they occur.

CHW can:

- See the total number of Submitted coaching sessions and total number of Submitted incident reports.
- View the number of students helped, indicating the total Submitted Coachings provided to the students.
- View the total hours of sessions given, indicating the total time spent for the Submitted Coaching sessions.
- See Incident reports in chronological order with the number of participants and coaching sessions.
- Upon clicking Participants, they can view the list of total participants involved in that Incident.
- Upon clicking the Coaching button, they can view the list of Coaching sessions provided associated with the incident.
- Clicking over the info icon, the user can view the details such as Incident ID, and Incident Title.

#### 2.2.3. Counselor

After successful sign-in, counselors are redirected to the main dashboard where they can view key metrics, such as the total number of counseling sessions in a specific time frame (semester, day, week, month, year) through a filter, incident reports, the total number of students helped, and total hours. The dashboard displays a list of upcoming sessions. Users can open any incident to view full details. The dashboard also provides options to cancel, or reschedule sessions. Sessions can be filtered based on criteria such as status, billed sessions, assigned by, date, and favorites, offering counselors a streamlined way to manage and track.

Counselor can:

- View the number of students helped, indicating the total Submitted Counseling sessions provided to the students.
- View the total hours of sessions given, indicating the total time spent for the Submitted Counseling sessions.
- View the total number of submitted Sessions and Incidents.
- Navigate to counseling sessions and incident reports.
- View list of sessions with their status (Upcoming, Submitted, Cancelled, Past).
- View incident details by opening any incident.
- Cancel or reschedule sessions. Please note that users cannot cancel sessions that are assigned by the Admin.
- Add/Remove the Sessions to Favourites by clicking on the star icon. Selecting sessions as Favorites pins them to the top of the list.
- Clicking over the info icon, the user can view the details such as Incident ID, Incident title, participant details and Assigned by.

#### 2.2.4 Admin

After successful sign-in, admins are redirected to the main dashboard where they can view key metrics, including the total number of sessions, incidents, the total number of students helped, and total hours. The dashboard displays a list of reports in chronological order, showing their status as Draft, Submitted, or Completed. Admins can view detailed reports and see the session count along with the incident details. The "+" icon on the dashboard is used to assign and schedule new sessions to counselors, streamlining the task assignment process.

Admin can:

- View the number of students helped, indicating the total Approved Counseling and Coaching sessions provided to the students across all classified staff, coaches and counselors.
- View the total hours of sessions given, indicating the total time spent for the Approved Counseling and Coaching sessions.
- View the total number of sessions and reported incidents on the dashboard.
- View a list of Incident reports in chronological order based on the date and status (Draft, and Submitted).
- Upon clicking Participants, they can view the list of total participants involved in that Incident.
- Upon clicking the Counseling session button, they can view the list of Counselings associated with the incident.
- Assign and schedule sessions to counselors from the "+" icon on the dashboard.

### 2.3. Incident Reporting

The Incident Reporting Flow defines how authorized users report incidents through the Incident Bot, generate standardized incident documentation, and submit reports for administrative review. The flow ensures consistent data capture, structured reporting, and controlled submission for compliance and audit purposes.

#### 2.3.1. Actors

- Classified Staff
- Certified Classified Staff / CHW
- Counselor
- _(Admin participates only in reviewing the Incident Reports)_

#### 2.3.2. Smart Incident Reporting

All users can view and create incident reports from the dashboard:

- “+” button allows creation of a new incident report
- Smart Incident Reporter supports voice recording and written input
- Guided assistance helps capture required incident details
- Incident information is prepared for SOAP-format report generation

**Description:**

After successful sign-in, Users are redirected to the main dashboard. From this screen, users can initiate a new incident report by selecting the “+” button. Upon selecting the “+” button, the Smart Incident Reporter is launched.

**Help Screen:** Before collecting the incident input, the system provides guidance on the required information, including date and time, location, parties involved, incident description, resolution, and counseling details if applicable.

**Smart Incident Reporter Bot Workflow:** This feature allows users to describe an incident either by speaking through voice recording or by typing the details. The system captures the provided information and prepares it for structured incident documentation and SOAP-format report generation.

**Step 1:** Initiate Incident Reporting

1.  _User Sign-In:_  
    The user signs into the system, using valid credentials (Classified Staff, Certified Classified Staff, or Counselor).
2.  _Incident Reporting Triggered:_  
    The user selects the option to report an incident through the Smart Incident Reporter Bot.

**Step 2:** Incident Data Collection

1.  Bot Prompts for Incident Details:  
    The Incident Bot asks the user to provide essential information for the incident report, including:
    - Incident Date: The date the incident occurred.
    - Incident Time: The time the incident occurred.
    - Location: Where the incident took place.
    - Description: A brief narrative of the incident (e.g., who was involved, what happened, the cause of the incident, and any immediate actions taken) and whatever details are necessary to generate a valid SOAP report.  
        
2.  Data Validation:  
    The system validates the entered data to ensure it meets the following criteria:
    - Mandatory fields (date, time, location, and description) must not be empty.
    - Valid date and time (no future dates or incorrect formats).
    - Valid location (the system may check against predefined locations if necessary).
    - Clear and complete description (brief and concise).

**Step 3:** Report Generation

1.  SOAP Format Generation:  
    Once the data is validated, the system automatically generates the incident report in SOAP format, which includes the following sections:
    - Subjective: The user's description of the incident.
    - Objective: Factual observations (e.g., witnesses, condition of involved persons, environment details).
    - Assessment: Interpretation of the incident's impact (e.g., severity, potential consequences).
    - Plan: Next steps, actions taken, or recommended follow-up (e.g., referral to counseling, follow-up check).  
        
2.  Incident Report Preview:  
    The user is presented with a preview of the SOAP-format report before submission. The user can review the content for accuracy and is prompted to select participants from student list

**Step 4:** Report Submission

1.  User Confirmation:  
    After reviewing the report, the user confirms the details and submits the incident report.  
    
2.  Incident Submission to Admin:  
    The incident report is securely stored in the system and sent to the Admin for further review. The report is assigned a "Submitted" status and becomes available for the Admin to view.

**Alternate Flows**

**AF-1:** Missing Mandatory Information

If the user fails to provide required information (e.g., date, description), the bot will prompt the user to enter the missing details before proceeding to the next step.

**AF-2:** Invalid Date or Time Format

If the user enters an invalid date (e.g., future dates or incorrect formats), the system will display an error message, prompting the user to correct the date and try again.

**AF-3:** Incident Description Length

If the incident description is too short or unclear, the system may prompt the user to provide more detailed information.

### 2.4. Coaching Sessions

#### 2.4.1. Actors

- Certified Classified Staff / CHW
- _(Admin participates can only in Review, Revise, Accept, or Reject the Coaching Reports)_

#### 2.4.2. Smart Coaching Bot

The Smart Coaching Bot Workflow allows users to report coaching sessions using the BIRP format (Behavior, Intervention, Response, Plan). The bot automatically generates a billing code based on session data, which the user can edit with a valid reason. A key feature is that users can click on draft reports to return to the exact point in the coaching chat where they left off, ensuring continuity and a smooth experience. The workflow covers all steps from initiating the session, collecting details, generating the report, and submitting it for Admin approval.  
Additionally, when a coaching session is initiated from an incident, the Student Name and Student ID automatically populate in the coaching session report, eliminating manual re-entry and reducing user error.

**Preconditions**

- The user is registered and authorized in the system.
- Coaching Bot service is active and operational.
- The user has signed in to the system.
- The user has either submitted an incident report or been assigned a coaching session by the Admin.

**Step 1:** Coaching Session Initiation

1.  User Sign-In:  
    The user (Certified Classified Staff / CHW, Counselor) signs into the system using valid credentials.
2.  Coaching Session Start:  
    After submitting an incident report or receiving an assignment from the Admin, the Smart Coaching Bot automatically initiates the coaching session within the same chat, or the user manually starts a new coaching session.

**Step 2:** Collecting Coaching Session Details

1.  BIRP Report Structure:  
    The Smart Coaching Bot prompts the user to enter information for each section of the BIRP format:
    - **Behavior:** The situation or behavior that triggered the coaching session (e.g., "Student displayed frustration during group work").
    - **Intervention:** The specific actions taken by the coach during the session (e.g., "Redirected the student and provided coping strategies").
    - **Response:** The participant's response to the intervention (e.g., "Student acknowledged frustration and agreed to try coping strategies").
    - **Plan:** The agreed-upon plan for follow-up actions (e.g., "Student will meet with a counselor next week for further guidance").
    - After collecting this information, Bot should not be asking for any further information.
2.  Billing Code Auto-Generation:  
    Based on predefined rules (session type, duration, user role), the Coaching Bot automatically generates a billing code based on the CYBHI billing codes. The system selects the correct code based on the session data, service provider, session length and number of participants.

**Step 3:** Editable Billing Code

1.  Option to Edit Billing Code:  
    After the billing code is auto-generated, the Coaching Bot presents the code to the user. The user can:
    - Edit the billing code if necessary (e.g., due to a different session type or duration).
    - Provide a reason for the edit (e.g., "Changed due to longer session time" or "Billing code adjusted for a different intervention").
2.  Billing Code Validation:  
    If the user edits the billing code, the system validates the new code to ensure it is correct. The user must enter a reason for the edit, which is logged for audit and compliance purposes.

**Step 4:** Report Generation

1.  BIRP Report Creation:  
    Once the Behavior, Intervention, Response, Plan, and Billing Code sections are completed, the Coaching Bot automatically generates the BIRP-format coaching report, including:
    - Student Name (Auto-Populated)
    - Student ID (Auto-Populated)
    - **Behavior:** Description of the behavior or incident.
    - **Intervention:** The actions and coaching techniques applied, including the billing code.
    - **Response:** The participant's feedback or reaction.
    - **Plan:** The next steps, such as follow-up actions or additional coaching.
    - **Billing Code:** The auto-generated or edited billing code, with the reason for any changes (if applicable).
2.  Report Preview:  
    The Coaching Bot presents a preview of the completed BIRP-format coaching report to the user for review. The user can check the accuracy of the report and billing code.

**Step 5:** Report Submission

1.  User Review:  
    The user reviews the final BIRP-format report for completeness and accuracy. The report includes all necessary details (Behavior, Intervention, Response, Plan, and Billing Code).
2.  Submit Report:  
    Once the user is satisfied with the report, they submit it to the Admin for review. The system stores the report securely and marks it as Pending Admin Approval.

**Step 6:** Post-Submission Handling

1.  Report Storage:  
    The completed coaching report, including the Billing Code, is securely stored in the system’s database.
2.  Admin Notification:  
    The Admin is notified of the new coaching report submission and can begin the review process.

**Step 7:** Draft Resumption Feature

1.  Access Draft Reports:  
    If the user needs to leave the session, they can save the draft. Clicking on the draft report at any time will automatically take the user back to the point in the coaching chat where they left off, allowing them to continue from the last entered field.
2.  Seamless Continuity:  
    This feature ensures that users do not lose any progress, allowing them to seamlessly return to the same chat interface to complete their report.

**Alternate Flows**

**AF-1:** Missing Data or Field

- If any mandatory fields (e.g., Behavior, Intervention, or Billing Code) are missing, the Coaching Bot will prompt the user to complete the missing information before proceeding to report submission.

**AF-2:** Invalid Billing Code

- Instead of restricting the user to predefined billing codes, allow them to select any billing code they want. The AI can suggest a code initially, but the Admin has the final say (Human in the Loop).

**AF-3:** Session Interruption

- If the session is interrupted (e.g., due to system errors or user inactivity), the system will automatically save the user’s progress. The user can resume entering data once logged back in.

**Postconditions**

- The BIRP report is submitted, including the Billing Code (either auto-generated or edited with a reason), and marked as Pending Admin Review.
- The report is securely stored in the system.
- Audit logs are created for the coaching session, including user ID, timestamps, and the reason for any changes to the billing code.

**Business Rules**

- Coaching reports must include Behavior, Intervention, Response, Plan, and Billing Code.
- Billing Code can be auto-generated or edited by the user. If edited, a valid reason must be provided, which will be logged for compliance and auditing.
- Coaching reports cannot be submitted without all required fields.
- Billing codes are automatically generated or validated by the system to ensure compliance with predefined rules.
- Only authorized users (Certified Classified Staff / CHW, Counselors) can submit coaching reports.

**Assumptions**

- Users are familiar with the coaching process and the required details for each BIRP section.
- The Coaching Bot helps guide users in selecting or editing the appropriate billing code based on session data.
- Admin reviews and approves the submitted reports and may request revisions if necessary.

### 2.5. Counseling Sessions

#### 2.5.1. Actors

- Counselor
- _(Admin participates can only in Review, Revise, Accept, or Reject the Counseling Reports)_

#### 2.5.2. Smart Counseling Bot Reporter

The Counseling Bot Workflow describes the process by which the Counseling Bot interacts with users (Counselors, Certified Classified Staff, and CHWs) to create and submit reports for counseling sessions using the BIRP format (Behavior, Intervention, Response, Plan). The system automatically generates a billing code for each session, which the user can edit with a provided reason. A draft resumption feature allows users to return to the chat interface from the point they left off when they click on draft reports.  
Additionally, when a counseling session is initiated from an incident, the Student Name and Student ID automatically populate in the counseling session report, eliminating manual re-entry and reducing user error.

**Preconditions**

- The user is registered and authorized in the system.
- The Counseling Bot service is active and operational.
- The user has signed in to the system.
- The user has either submitted an incident report or been assigned a counseling session by the Admin.

**Trigger**

The Counseling Bot is triggered after an incident report is submitted or when the user manually starts a new counseling session.

**Step 1:** Counseling Session Initiation

1.  User Sign-In:  
    The user (Counselor, Certified Classified Staff / CHW) signs into the system using valid credentials.
2.  Counseling Session Start:  
    After submitting an incident report or being assigned a counseling session by the Admin, the Counseling Bot automatically initiates the counseling session within the same chat, or the user can manually start a new counseling session based on the assigned incident.

**Step 2:** Collecting Counseling Session Details

1.  BIRP Report Structure:  
    The Counseling Bot prompts the user to enter details for each of the following sections of the BIRP format:  
    - **Behavior**: Describes the behavior or situation that led to the counseling session (e.g., "Student exhibited disruptive behavior in class").
    - **Intervention**: Describes the specific actions taken by the counselor during the session (e.g., "Counselor discussed coping strategies and set expectations for behavior").
    - **Response**: Describes the participant's response to the intervention (e.g., "Student agreed to follow strategies and showed willingness to improve").
    - **Plan**: Describes the agreed-upon follow-up actions or future steps (e.g., "Student will attend weekly sessions to work on behavioral improvements").
    - After collecting this information, Bot should not be asking for any further information.
2.  Billing Code Auto-Generation:  
    Based on predefined rules (session type, user role, session duration), the Counseling Bot automatically generates a billing code (e.g., H2014, 96156). The correct billing code is selected by the system based on the session data provided by the user.

**Step 3:** Editable Billing Code

1.  Option to Edit Billing Code:  
    After the billing code is auto-generated, the Counseling Bot presents the code to the user. The user can:
    - Edit the billing code if necessary (e.g., if the session duration changes or a different session type applies).
    - Provide a reason for the edit (e.g., "Changed billing code due to extended session time," "Adjusted code for different intervention type").
2.  Billing Code Validation:  
    If the user edits the billing code, the system validates the new code to ensure it is correct and within acceptable parameters. The user must also enter a reason for the change, which will be logged for audit and compliance purposes.

**Step 4:** Report Generation

1.  BIRP Report Creation:  
    Once the Behavior, Intervention, Response, Plan, and Billing Code sections are completed, the Counseling Bot automatically generates the BIRP-format counseling report, including:
    - **Behavior**: Description of the behavior or situation.
    - **Intervention**: The actions and counseling techniques applied, including the billing code.
    - **Response**: The participant's feedback or reaction.
    - **Plan**: The next steps, including follow-up actions or additional counseling.
    - **Billing Code**: The auto-generated or edited billing code, along with the reason for any changes (if applicable).
2.  Report Preview:  
    The Counseling Bot presents the user with a preview of the completed BIRP-format coaching report for review. The user can ensure the accuracy of the report and billing code before submitting it.

**Step 5:** Report Submission

1.  User Review:  
    The user reviews the final BIRP-format report for completeness and accuracy, including all details auto-populated student names and student ids, and the Billing Code.
2.  Submit Report:  
    Once the user is satisfied with the report, they submit it to the Admin for review. The system stores the report securely and marks it as Pending Admin Approval.

**Step 6:** Post-Submission Handling

1.  Report Storage:  
    The completed counseling report, including the Billing Code, is securely stored in the system’s database.
2.  Admin Notification:  
    The Admin is notified of the new counseling report submission and can begin the review process.

**Step 7:** Draft Resumption Feature

1.  Access Draft Reports:  
    If the user needs to leave the session, they can save the draft. Clicking on the draft report will automatically take the user back to the exact point in the counseling chat where they left off. This allows users to continue from the last entered field.
2.  Seamless Continuity:  
    This feature ensures that users do not lose any progress, allowing them to seamlessly return to the same chat interface to complete their report.

**Alternate Flows**

**AF-1:** Missing Data or Fields

- If any mandatory fields (e.g., Behavior, Intervention, or Billing Code) are missing, the Counseling Bot will prompt the user to complete the missing information before proceeding to report submission.

**AF-2:** Invalid Billing Code

- If the user enters an invalid billing code (e.g., one that doesn't match predefined codes), the system will prompt the user to correct it or select from a list of valid codes.

**AF-3:** Session Interruption

- If the session is interrupted (e.g., due to system errors or user inactivity), the system will automatically save the user’s progress. The user can resume entering data once logged back in.

**Postconditions**

- The BIRP report is submitted, including the Billing Code (either auto-generated or edited with a reason), and marked as Pending Admin Review.
- The report is securely stored in the system.
- Audit logs are created for the counseling session, including user ID, timestamps, and reason for any changes to the billing code.

**Business Rules**

- Counseling reports must include Behavior, Intervention, Response, Plan, and Billing Code.
- Billing Code can be auto-generated or edited by the user. If edited, a valid reason must be provided, which will be logged for compliance and auditing.
- Counseling reports cannot be submitted without all required fields.
- Billing codes are automatically generated or validated by the system to ensure compliance with predefined rules.
- Only authorized users (Counselors) can submit counseling reports.

**Assumptions**

- Users are familiar with the counseling process and the required details for each BIRP section.
- The Counseling Bot helps guide users in selecting or editing the appropriate billing code based on session data.
- Admin reviews and approves the submitted reports and may request revisions if necessary.

#### 2.5.3. Group Counseling Workflows – Three Operational Scenarios (With QR Registration)

**Overview**

The system supports three structured counseling scenarios:

1.  Group Counseling (Incident-Based Group Session)
2.  Class Counseling (Mock Incident-Based Class Session with QR Invitation)
3.  Independent Counseling (Individual Sessions BIRP per Group combined with the incident report with SOAP format )

For **Class Counseling**, the Counselor may invite students using a **QR Code**.  
Students scan the QR code and enter their own information (Name, Student ID, Class), and the system automatically registers them for the session.

Each scenario defines how incidents are created, how participants are added, how BIRP reports are generated, and how billing is processed.

**Scenario 1: Group Counseling (Incident-Based Group Session)**

**Description**

In this scenario, the Counselor reports a real incident involving more than one student and conducts a single group counseling session for all participants. A single consolidated BIRP report is generated for the entire group.

⚠️ In this scenario, students are added directly through the incident report.  
No QR code is used.

**Functional Flow**

**Step 1: Incident Reporting**

1.  Counselor logs into the system.
2.  Counselor creates an Incident Report.
3.  Counselor selects more than one student as participants.
4.  System generates a SOAP-format incident report.
5.  Incident is saved or submitted.

**Step 2: Start Group Counseling**

1.  Counselor selects Start Counseling from the incident.
2.  System detects multiple participants.
3.  Session type automatically becomes Group Counseling.
4.  Participant list is automatically carried forward from the incident.

**Step 3: Conduct Group Counseling**

1.  Counseling Bot initiates structured prompts.
2.  Counselor provides:

- Behavior (group-level summary)
- Intervention (group strategies used)
- Response (overall group engagement)
- Plan (collective follow-up actions)

**Step 4: Report & Billing**

1.  System generates **one consolidated Group BIRP Report**.
2.  System auto-generates group billing code based on:

- Session duration
- Session type
- Counselor role

1.  Counselor may edit billing code (mandatory reason required).
2.  Report submitted to Admin.

**Output**

- 1 Incident Report (SOAP)
- 1 Group Counseling BIRP Report
- 1 Group Billing Code

**Images:**

 

**Scenario 2: Class Counseling (Mock Incident-Based with QR Invitation)**

**Description**

The Counselor selects a Mock Incident and conducts a Class-Level Counseling Session. Students register through QR code. A single class-level BIRP report is generated.

**Functional Flow**

**Step 1: Select Mock Incident**

1.  Counselor selects Mock Incident for Class Session.
2.  Incident marked as Class-Level.

**Step 2: Generate QR Code**

1.  Counselor creates Class Counseling Session.
2.  The system generates session ID.
3.  The system generates QR Code for the class.
4.  Counselor displays QR to class.

**Step 3: Student Registration**

1.  Students scan QR code.
2.  Students enter:
    - Name
    - Student ID
    - Class
3.  System validates entries.
4.  Students added to the session participant list.

**Step 4: Conduct Class Counseling**

1.  Counseling Bot collects:

- Behavior (class-wide issue)
- Intervention (workshop/discussion)
- Response (overall class engagement)
- Plan (follow-up awareness program)

**Step 5: Report & Billing**

1.  The system generates one Class BIRP Report.
2.  System auto-generates class/group billing code.
3.  Billing code editable with mandatory reason.
4.  Report submitted to Admin.

**Output**

- 1 Mock Incident
- 1 Class-Level BIRP Report
- 1 Class Billing Code
- QR-based student participation list

**Images:**

 

**Scenario 3: Independent Counseling (Individual Sessions per Group)**

**Description**

The Counselor reports an incident involving multiple participants and conducts group counseling sessions while reporting the incident.

QR code may optionally be used to confirm participant presence.

**Functional Flow**

**Step 1: Incident Reporting**

1.  Counselor creates Incident Report.
2.  Multiple students selected or registered via QR.
3.  The system generates incident report.

**Step 2: Independent Counseling Mode**

1.  Counselor reports a group counseling session.
2.  System processes participants collectively as a group under one session.

**Step 3: Group Sessions**

1.  For all students in a group
    - Counseling Bot opens individual session (separate drafts creation).
    - Counselor enters:
        - Behavior (individual)
        - Intervention
        - Response
        - Plan
    - System generates separate BIRP report.

**Step 4: Billing**

1.  The system generates individual billing code per student.
2.  Billing code editable with reason.
3.  Each report is submitted separately.

**Output**

- 1 Incident Report
- Group BIRP Reports
- Group Billing Codes
- Optional QR attendance record

**Images:**

 

**Business Rules (QR-Based Sessions)**

- QR code must be session-specific and secure.
- QR link must expire after session closes.
- Duplicate student entries must be prevented.
- Students must enter their own information.
- Minimum participants required for group sessions.
- Billing edits require mandatory reason.
- All QR registrations must be audit logged.

**Status Lifecycle**

Draft → Submitted → Pending Admin Review →  
Approved / Rejected / Revision Requested → Resubmitted → Approved

**Summary**

The system supports:

- Group Counseling with QR-based student registration.
- Class Counseling using Mock Incident + QR.
- Independent Counseling with separate reports for a group

QR functionality enables students to self-register securely, ensuring accurate participant tracking and automated BIRP documentation with controlled billing logic and administrative oversight.

### 2.6. Admin Workflow

#### 2.6.1. Billable Actions

Billable actions refer to actions taken by the Admin that directly affect the financial documentation of coaching and counseling sessions. These actions include approving or rejecting reports with associated billing codes, and requesting revisions that may alter the billing codes. Each of these actions must be logged with detailed reasoning and notes to ensure transparency and compliance.

**Step 1:** Review of Billable Reports (Coaching & Counseling)

1.  View Submitted Reports:  
    The Admin reviews coaching and counseling reports submitted by Certified Classified Staff / CHW and Counselors. The reports include a billing code for each session, which is generated either automatically or manually by the user.

**Step 2:** Approve Billable Reports

1.  Approve Reports:  
    The Admin can approve coaching and counseling reports that contain valid billing codes.
    - Action Taken: The Admin approves the report and it is marked as finalized.

**Step 3:** Reject Billable Reports

1.  Reject Reports:  
    If a coaching or counseling report contains an invalid billing code or other discrepancies, the Admin may reject the report.
    - Action Taken: The report is rejected and the user is notified.
    - Reason & Notes: The Admin must provide a reason and notes for the rejection (e.g., "Invalid billing code", "Session duration mismatch").
    - Impact on Billing: The billing code is not considered valid for reimbursement until the necessary revisions are made.

**Step 4:** Request Revisions for Billable Reports

1.  Request Revision for Reports:  
    When the Admin identifies discrepancies or required changes in coaching or counseling reports that impact the billing code or session details, they can request a revision.
    - Action Taken: The report status is updated to "Revision Requested".
    - Reason & Notes: The Admin provides specific instructions on what needs to be corrected and includes a reason for the revision request (e.g., "Adjust billing code due to session length adjustment").
    - Impact on Billing: A revised billing code may be issued once the changes are made, and the session can be re-approved for billing.

**Step 5:** Impact on Billing Codes

1.  Billing Code Tracking:  
    Every approved, rejected, or revision requested action by the Admin should be logged with the corresponding billing code.
    - Billing Code Validation: The Admin validates each billing code during report review.
    - Audit & Compliance: Any changes to the billing code (e.g., rejection or revision) must be logged with the reason and notes to ensure proper audit tracking for financial compliance. The time a counseling/coaching session is generated vs billed and to what counselor/coach it was assigned need to be recorded in the audit log.

**Postconditions for Billable Actions**

- **Approved Reports**:  
    Once a coaching or counseling report is approved, it is marked as finalized, and the billing code is locked for that session.  
    
- **Rejected Reports**:  
    Rejected reports are flagged for further action (e.g., revisions), and the billing code is not accepted for financial processing until corrections are made.  
    
- **Revision Requests**:  
    Reports flagged for revision require the user to make necessary changes. After the revised report is submitted, the Admin reviews and either approves or rejects the updated report.

**Business Rules for Billable Actions**

1.  **Approval/Rejection Impact on Billing**:  
    Only approved reports with valid billing codes are eligible for billing or reimbursement.  
    
2.  **Audit Log Requirement**:  
    All actions involving billing codes (approval, rejection, revision requests) must be logged with a reason and notes. This ensures full transparency and compliance with financial tracking and audit standards.  
    
3.  **Billing Code Revisions**:  
    The Admin must specify the reason for any billing code revisions and ensure that all corrections are compliant with financial guidelines.

**Assumptions for Billable Actions**

- Admins are trained to recognize valid billing codes and understand how to handle discrepancies.
- The system ensures billing codes are automatically validated and logged for audit purposes.
- Users are notified promptly of rejected reports or revision requests with detailed instructions for correction.

#### 2.6.2. User Management

**Overview**

The User Management module provides administrators with centralized control to manage all system users. Through this module, the Admin can search, filter, approve, reject, activate or deactivate user accounts, reset security keys, and view detailed user profiles. The module ensures secure access control, regulatory compliance, and efficient user administration.

**Key Features & Screen Descriptions**

**1\. User Management List Screen**

- Displays a list of all registered users in the system.
- Each user card includes:
    - User name
    - Account status (Activated / Deactivated / Pending)
    - User role (Counselor / Classified Staff/Certified Health Worker/Admin)
    - NPI certification status
    - Account creation date

**2\. Search & Filtering**

- **Search Bar:**
    - Allows the Admin to search users by name.
- **Role Filter:**
    - Filter users based on role:
        - Counselor
        - Classified Staff
        - All
- **Status Filter:**
    - Filter users based on account status:
        - Activated
        - Deactivated
        - Pending
        - All
- **Certification Filter:**
    - Filter users based on NPI certification status.

**3\. Pending User Approval Workflow**

- Users with **Pending** status require Admin action.
- Admin can:
    - **Approve** the account to activate system access.
    - **Reject** the account request.
    - **Pending** the account request is under review and awaiting admin action.
- A confirmation dialog is displayed before rejecting an account to prevent accidental actions.

**4\. Activate / Deactivate User Accounts**

- Admin can activate or deactivate user accounts using a toggle switch.
- Deactivated users are restricted from accessing the system.
- Status updates are reflected in real time across the platform.

**5\. Reset One-Time Security Key**

- Admin can assign or reset a **one-time security key** for any user.
- A secure popup is displayed with:
    - New Key field
    - Confirm Key field
- Upon successful assignment, a confirmation message is shown.
- This feature strengthens account security and access control.

**6\. View User Profile & Details**

- Admin can open a user’s detailed profile by clicking **View Details**.
- The profile screen displays:
    - Profile photo
    - Full name
    - Role (Counselor / Classified Staff/Certified Health Worker)
    - NPI certification status
    - Account status
    - Email address
    - Member since date
    - Last login time
    - Activity details (Students Helped, Counseling/Coaching Sessions Provided, Incidents, Hours, Counseling/Coaching Revenue)
- From this screen, Admin can:
    - Reset the user’s security key
    - Deactivate the user account

**7\. Linked Student Overview**

- Admin can view a summary of students associated with a user, including:
    - Number of incident reports
    - Coaching session count
    - Counseling session count
- This overview helps assess user activity and workload.

**Security & Compliance**

- Access to the User Management module is restricted to Admin users only.
- All critical actions (approval, rejection, activation, deactivation, key reset) are auditable.
- One-time security keys prevent unauthorized access.
- Role-based access control is strictly enforced to maintain compliance and data security.

#### 2.6.3 Student Management – Secure Upload via Admin Web Portal

The system will provide the Admin with a secure web-based portal to manage student records efficiently.

**Student Upload Functionality**

- The Admin will have access to an **“Add Students”** option within the web portal.
- By clicking on **Add Students**, the Admin will be able to **securely upload an Excel file** containing the students’ list.
- The upload process will be protected using secure protocols to ensure data confidentiality and integrity.

**Student List Management**

- Once the Excel file is successfully uploaded, the system will process the data and display the **complete list of added students** within the Admin portal.
- The Admin will be able to:
    - **View** all uploaded student records
    - **Edit** individual student information as required
    - **Delete** student records if needed

**Data Control & Accuracy**

- Any updates made by the Admin (edit or delete actions) will be reflected in real time within the system.
- This functionality ensures that the Admin maintains **full control over student data**, keeping records accurate and up to date at all times.

#### 

#### 

#### 

#### 

#### 2.6.4 Reporting

The CYBHI platform provides Administrators with a comprehensive reporting and analytics suite that consolidates operational, clinical, financial, and compliance‑related data. The reporting module supports real‑time monitoring, trend analysis, and exportable insights to inform decision‑making at the school, district, and program levels.

#### 2.6.4.1 Incident Reporting Metrics

Provides system‑wide visibility into safety, behavioral, and crisis‑related incidents.

Capabilities

- Incident volume by type, severity, location, and timeframe
- Resolution timelines and response compliance
- Trend analysis and cross‑school comparisons
- Identification of recurring patterns or high‑risk contexts
- Permission‑based drill‑down to case‑level detail

#### 2.6.4.2 Coaching Metrics

Enables monitoring of coaching service delivery and student engagement.

Capabilities

- Session volume by school, provider, or student group
- Attendance, cancellations, and no‑show rates
- Session duration and frequency patterns
- Goal‑aligned progress indicators
- Aggregate improvement trends

#### 2.6.4.3 Counseling Metrics

Supports oversight of therapeutic and clinical services.

Capabilities

- Caseload distribution and acuity levels
- Counseling session volume and engagement metrics
- Treatment plan progress and milestone tracking
- Referral sources and referral‑to‑service timelines
- Outcome measures and symptom‑reduction indicators

#### 2.6.4.4 Financial & Billing Metrics

Provides financial transparency and supports billing compliance.

Capabilities

- Billable service utilization and unit tracking
- Revenue summaries and reimbursement projections
- Payer‑specific breakdowns (Medi‑Cal, private insurance, district‑funded)
- Claim submission status, denials, and resubmission rates
- Cost‑per‑service and cost‑per‑student analytics

#### 2.6.4.5 School‑Level Operational Metrics

Offers a holistic view of operational performance across school sites.

Capabilities

- Service penetration rates and student reach
- Wait times for coaching, counseling, and assessments
- Provider availability and scheduling efficiency
- Utilization by grade level, demographic group, or need category
- Cross‑site benchmarking and performance comparisons

#### 2.6.4.6 Outcomes & Impact Metrics

Measures the effectiveness and long‑term impact of CYBHI interventions.

Capabilities

- Student‑level and population‑level outcome improvements
- Pre‑/post‑assessment comparisons
- Longitudinal well‑being and functioning indicators
- Intervention‑specific impact analysis
- Equity‑focused outcome reporting

#### 2.6.4.7 Staffing & Performance Metrics

Supports workforce planning, productivity monitoring, and quality assurance.

Capabilities

- Provider caseloads and workload distribution
- Session delivery rates vs. expected capacity
- Documentation timeliness and quality indicators
- Credentialing, training, and compliance tracking
- Staffing gap identification and forecasting

#### 2.6.4.8 Compliance & Audit‑Ready Reports

Ensures readiness for regulatory, contractual, and internal audits.

Capabilities

- Documentation completeness and timeliness checks
- Consent, signatures, and required form tracking
- Service delivery compliance against program standards
- Immutable audit logs with timestamps
- Exportable compliance packets for oversight bodies

#### 2.6.4.9 System Health & Operational Monitoring Metrics

Provides visibility into the technical and operational performance of the CYBHI platform.

Capabilities

- System uptime, latency, and error‑rate dashboards
- Integration health monitoring (SIS, EHR, billing systems)
- Data quality checks and anomaly detection
- User activity logs and access monitoring

### 2.7. Listing Operations

#### 2.7.1. Incident Lists (Non-Certified Staff)

- Non-certified classified staff can view a list of incident reports only.
- Filters are available horizontally for classification by All, status, date, and favorites.
- The status filter allows classification by Submitted, Generated, and Draft statuses.
- The date filter allows incidents to be filtered by any specific date.
- Each incident has a Participants section showing the number of participants (e.g., Participants (2)).
- When classified staff click on the Participants section, a list of participants will appear in a pop-up.  
    

**Description:**

Non-certified classified staff can view a list of incident reports, with the ability to filter by status (Submitted, Generated, Draft), date (any specific date), and favorites. Each incident report includes a Participants section, indicating the number of participants (e.g., Participants (2)). When clicked, a pop-up will display the list of participants involved in the incident, allowing for easy access to participant details.

#### 2.7.2. Incident Lists (CHW)

- The list of incidents can be viewed by clicking on “View All” in the dashboard.
- Filters are available horizontally, allowing classification by All, Status, Date, and Favorites.
- The status filter allows classification by Submitted, Generated, and Draft statuses.
- The Assigned by filter allows filtering by Assigned by Self or Assigned by Admin.
- The date filter allows incidents to be filtered by any specific date.
- Upon clicking Participants, they can view the list of total participants involved in that Incident.
- Upon clicking the Coaching button, they can view the list of Coachings provided associated with the incident.

**Description:**

When certified staff click on “View All” from the dashboard, a list of incidents is displayed. The incidents are presented with several filter options horizontally, including Status, Date, and Favorites. Users can filter incidents by status (Submitted, Generated, Draft, All), and date (any specific date). This provides a customizable and easy way to view and manage incidents according to the desired criteria.

#### 2.7.3. Coaching Lists

- In coaching, filters are available horizontally for classification by All, Status, Date, and Favorites.
- The Status filter allows classification by Approved, Reviewed, Draft, Rejected, Generated, and All.
- The Favorites filter allows sessions to be filtered based on user favorites.
- The Date filter allows sessions to be filtered by specific dates.
- See the total number of coaching sessions and total number of incident reports.
- Upon clicking Participants, they can view the list of total participants involved in that Incident.
- Upon clicking the Coaching button, they can view the list of Coachings provided associated with the incident.
- Clicking over the info icon, the user can view the details such as Incident ID, and Incident Title.

**Description:**

In the coaching section, users can view a list of sessions with the option to filter them horizontally by Status, Billed, State, Assigned by, Date, and Favorites. The Status filter allows classification by Upcoming, Submitted, Past, Canceled, and All statuses.The Billed filter allows classification by Approved, Pending, Review, Rejected, and All statuses.The State filter allows classification by Submitted, Generated, Draft and All statuses. The Assigned by filter allows classification by Self and Admin. Users can also filter sessions based on their favorites or by a specific date using the date filter, providing a customizable way to view and manage coaching sessions.

#### 2.7.4. Counseling Lists

- In the counseling sessions list, filters are available horizontally for classification by Status, Billing, Assigned By, Date, and Favorites.
- The Status filter can further classify sessions as Upcoming, Submitted, Past, Cancelled, or All.
- The Billing filter can further classify sessions as Approved, Pending, Rejected, Reviewed, or All.
- The Assigned By filter allows classification by Myself or Admin.
- The Date filter allows sessions to be filtered by specific dates.
- The Favorites filter allows sessions to be filtered based on user favorites.

**Description:**

In the counseling sessions list, users can filter sessions horizontally by Status, Billed, Assigned By, Date, and Favorites. The Status filter classifies sessions into Upcoming, Submitted, Past, Cancelled, or All. The Billed filter allows further classification by Approved, Pending, Rejected, Reviewed, or All. The Assigned By filter lets users filter sessions by whether they were assigned by Myself or Admin. Additionally, the Date filter enables users to filter sessions by a specific date, while the Favorites filter allows sessions to be filtered based on user preferences.

# **SIGN-OFF TABLE (Evolo AI):**

| **Release** | **Scope Section Reference** | **Req. Owner** | **Lock Date** | **Dev** | **QA** | **TF Version** |
| --- | --- | --- | --- | --- | --- | --- |
| 01/23/26 | 4.2,4.3,4.4, 5.1, 5.3, 5.4.2 | AC, AA | 01/16 |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
| 01/29/26 | 1.1-2.6.2 | AC  | 01/28 |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |
| --- | --- | --- | --- | --- | --- | --- |

# **SIGN-OFF TABLE (Client):**

| **FDS Version** | **Date Reviewed** | **Client Confirmation  <br>Sign Off** | **Notes** |
| --- | --- | --- | --- |
| 1.0 |     |     |     |
| --- | --- | --- | --- |
|     |     |     |     |
| --- | --- | --- | --- |
|     |     |     |     |
| --- | --- | --- | --- |
|     |     |     |     |
| --- | --- | --- | --- |
|     |     |     |     |
| --- | --- | --- | --- |
|     |     |     |     |
| --- | --- | --- | --- |

# Tab 2

## **Student Management – Secure Upload via Admin Web Portal**

The system will provide the Admin with a secure web-based portal to manage student records efficiently.

### Student Upload Functionality

- The Admin will have access to an **“Add Students”** option within the web portal.
- By clicking on **Add Students**, the Admin will be able to **securely upload an Excel file** containing the students’ list.
- The upload process will be protected using secure protocols to ensure data confidentiality and integrity.

### Student List Management

- Once the Excel file is successfully uploaded, the system will process the data and display the **complete list of added students** within the Admin portal.
- The Admin will be able to:
    - **View** all uploaded student records
    - **Edit** individual student information as required
    - **Delete** student records if needed

### Data Control & Accuracy

- Any updates made by the Admin (edit or delete actions) will be reflected in real time within the system.
- This functionality ensures that the Admin maintains **full control over student data**, keeping records accurate and up to date at all times.

# Group Counseling**Mock Incident-Based Class Counselling with QR Code Registration Workflow**

## **Overview**

This workflow enables a **Counsellor** to choose a **mock incident**, initiate a **group counselling session**, and invite students to join using a **QR code**. Students register themselves by scanning the QR code and entering their own information (Name, Student ID, Class). After the session, the Counsellor uses the **Counselling Bot** to generate a **BIRP-format group report**, which includes an automatically generated **billing code**, and submits the report to the **Admin** for approval.

This process ensures structured documentation, secure participant registration, automated billing logic, and administrative oversight.

## **Actors**

- Counsellor
- Students
- Admin

## **Preconditions**

- The Counsellor account is active and authorized.
- Student App or web access for QR scanning is available.
- QR code generation service is operational.
- Billing logic is configured.
- Admin approval workflow is active.

## **Trigger**

The Counsellor selects **“Create Mock Incident & Group Counselling Session”** from the dashboard.

# **Functional Workflow**

## **Step 1: Mock Incident Selection**

1.  Counsellor logs into the system.
2.  Counsellor selects **Create Mock Incident**.
3.  Counsellor enters:
    - Incident title
    - Incident date and time
    - Location
    - Brief description
4.  The system generates a **SOAP-format mock incident report**.
5.  The incident is saved as linked to the upcoming group session.

## **Step 2: Group Counselling Session Creation**

1.  Counsellor selects **Create Group Counselling Session**.
2.  Counsellor enters:
    - Session date
    - Start time
    - End time
    - Location
3.  The system validates session timing and duration.
4.  The system generates a **unique session ID**.

## **Step 3: QR Code Generation & Distribution**

1.  The system generates a **unique QR Code** linked to the session.
2.  The QR code contains a secure session registration link.
3.  Counsellor can:
    - View QR code
    - Download QR code image
    - Save QR code to device
    - Print QR code for physical display
4.  QR code remains active until session closes.

## **Step 4: Student Self-Registration via QR Code**

1.  Students scan the QR code using their mobile device.
2.  The QR link opens a secure registration form.
3.  Students enter:
    - Full Name
    - Student ID
    - Class/Grade
4.  Students submit the form.
5.  System validates:
    - Required fields completed
    - Student ID format validity
6.  Student information is stored and linked to the session.
7.  Students are added to the **participant list** for that group session.

⚠️ Important:  
Students enter their own information. The Counsellor does not manually add students.

## **Step 5: Live Participant Monitoring**

1.  Counsellor can view a real-time participant list.
2.  Counsellor can see:
    - Registered student names
    - Student IDs
    - Class
3.  Counsellor may lock registration once session begins.

## **Step 6: Conducting the Group Session**

1.  Counsellor conducts group counselling session.
2.  After session completion, Counsellor opens the **Counselling Bot**.

## **Step 7: BIRP Report Generation (Group Format)**

1.  Counselling Bot collects structured inputs for:

- **Behavior  
    **Overall group behavior, presenting issues, common concerns.
- **Intervention  
    **Group therapy techniques used (discussion, CBT exercises, role-play, etc.).
- **Response  
    **Group engagement level and participant responses.
- **Plan  
    **Follow-up steps, next session scheduling, referrals.

1.  System generates **Group BIRP Report**.

## **Step 8: Billing Code Auto-Generation**

1.  Based on:

- Group session type
- Duration
- Number of participants
- Counsellor role

1.  The system automatically generates appropriate **Group Billing Code**.

## **Step 9: Editable Billing Code (With Reason)**

1.  Counsellor may:

- Accept auto-generated billing code
- Edit billing code

1.  If edited:

- Mandatory reason must be provided
- System validates new billing code
- Both original and edited codes are logged

## **Step 10: Report Preview**

1.  System displays complete preview including:

- Mock incident reference
- Participant list (QR registered students)
- Session duration
- BIRP content
- Billing code
- Billing edit reason (if applicable)

## **Step 11: Draft & Resumption**

1.  Counsellor may save as **Draft**.
2.  Clicking draft:

- Returns to same chat position
- Restores session data
- Maintains participant list

## **Step 12: Submission to Admin**

1.  Counsellor submits report.
2.  Status updates to **Pending Admin Approval**.
3.  Admin receives notification.

# **Admin Review Workflow**

1.  Admin reviews:

- Incident details
- Participant list (QR registered students)
- BIRP report
- Billing code
- Billing edit reason (if applicable)

1.  Admin may:

- Approve
- Reject (mandatory reason + notes)
- Request Revision (mandatory reason + notes)

1.  If revision requested:

- Counsellor edits and resubmits
- Version history maintained

# **Postconditions**

- The approved session becomes finalized.
- Billing code becomes locked.
- Student participation record stored.
- Audit log created for:
    - QR registrations
    - Billing code generation
    - Billing edits
    - Admin decision
    - Revision history

# **Business Rules**

- QR code must be session-specific and unique.
- Students must enter their own information.
- Duplicate student entries must be prevented.
- Minimum participant threshold may be enforced.
- Billing code edits require mandatory justification.
- Only approved sessions are eligible for billing export.
- QR registration link expires after session closure.

# **Status Lifecycle**

Draft → Submitted → Pending Admin Review →  
Approved / Rejected / Revision Requested → Resubmitted → Approved

# **Security & Compliance**

- QR links must be secure and non-guessable.
- Student data must be encrypted at rest.
- Session access must expire after defined duration.
- All billing modifications must be audit logged.
- Admin notes must be stored permanently.

# **Summary**

This workflow allows a Counsellor to:

- Create a mock incident
- Generate a group counselling session
- Invite students via QR code
- Allow students to self-register
- Generate a BIRP group report through the Counselling Bot
- Auto-generate and optionally edit billing codes
- Submit for Admin approval

The system ensures structured documentation, secure student registration, automated billing logic, and full administrative control.

# **Admin-Initiated Counselling Session (Individual & Group)**

## **Overview**

This workflow enables the **Admin** to create a counselling session and assign it to a Counsellor. During session creation, the Admin selects the **“Type”** field to determine whether the session is **Individual** or **Group**.

- If **Individual** is selected → only **one student** can be assigned.
- If **Group** is selected → **more than one student** must be selected.

This ensures structured session assignment and prevents incorrect participant configuration.

## **Actors**

- **Admin**
- **Counsellor**
- **Students**
- **Counselling Bot** (for documentation)

## **Preconditions**

- Admin account is active and authorized.
- Student records exist in the system.
- Counsellor accounts are active.
- Counselling module is enabled.

## **Trigger**

Admin selects **“Create Counselling Session”** from the Admin Dashboard.

# **Functional Workflow**

## **Step 1: Session Creation by Admin**

1.  Admin logs into the system.
2.  Admin navigates to **Counselling Sessions**.
3.  Admin clicks **Create New Session**.
4.  Admin selects:
    - Assigned Counsellor
    - Session Date
    - Start Time
    - End Time
    - Location (if applicable)

## **Step 2: Select Type (Individual / Group)**

1.  Admin selects **Type** from dropdown:
    - **Individual**
    - **Group**

## **Step 3A: Individual Counselling Flow**

If **Type = Individual**:

1.  System allows selection of **only one student**.
2.  Admin selects a single student from the student list.
3.  System validates:
    - Only one student selected.
    - Student is active.
4.  Admin confirms session creation.
5.  Session is saved as **Individual Counselling Session**.

## **Step 3B: Group Counselling Flow**

If **Type = Group**:

1.  System allows selection of **multiple students**.
2.  Admin selects **more than one student**.
3.  System validates:
    - Minimum participants > 1.
    - No duplicate students.
    - Students are active.
4.  Admin confirms session creation.
5.  Session is saved as **Group Counselling Session**.

## **Step 4: Counsellor Notification**

1.  System notifies assigned Counsellor.
2.  Session appears in Counsellor Dashboard under:

- Upcoming Sessions
- Assigned Sessions

## **Step 5: Counselling Documentation (By Counsellor)**

1.  Counsellor opens the assigned session.
2.  Counselling Bot initiates structured documentation.
3.  Counsellor provides BIRP inputs:

- Behavior
- Intervention
- Response
- Plan

## **Step 6: Billing Code Auto-Generation**

1.  System auto-generates billing code based on:

- Session Type (Individual / Group)
- Duration
- Role
- Number of participants (for group)

1.  Billing code is displayed to Counsellor.

## **Step 7: Editable Billing Code (With Reason)**

1.  Counsellor may:

- Accept auto-generated billing code
- Edit billing code

1.  If edited:

- Mandatory reason must be entered.
- System logs original code + edited code.
- Billing change is audit logged.

## **Step 8: Draft & Resume**

1.  Counsellor may save session as Draft.
2.  Clicking draft:

- Restores BIRP inputs
- Returns to last chat position
- Maintains participant list

## **Step 9: Submission to Admin**

1.  Counsellor submits session report.
2.  Status updates to **Pending Admin Review**.

## **Step 10: Admin Review**

1.  Admin reviews:

- Session type
- Participant(s)
- BIRP report
- Billing code
- Billing edit reason (if applicable)  
    

1.  Admin may:  
    

- Approve
- Reject (mandatory reason + notes)
- Request Revision (mandatory reason + notes)

# **Validation Rules**

### For Individual Type:

- Only one student allowed.
- System must block submission if multiple selected.

### For Group Type:

- Minimum two students required.
- System must block submission if only one selected.
- Duplicate student selection not allowed.

# **Postconditions**

- Approved session becomes finalized.
- Billing code becomes locked.
- Participant records updated.
- Audit log captures:
    - Session type
    - Participants
    - Billing code generation
    - Billing edits
    - Admin decision

# **Business Rules**

- Type field is mandatory.
- Type cannot be changed after submission.
- Individual billing rules differ from Group billing rules.
- Billing edits require mandatory justification.
- All admin decisions must include reason if rejecting or requesting revision.

# **Status Lifecycle**

Draft → Assigned → Submitted → Pending Admin Review →  
Approved / Rejected / Revision Requested → Resubmitted → Approved

# **Summary**

This workflow ensures:

- Admin controls whether a counselling session is Individual or Group.
- Proper student selection validation.
- Structured BIRP documentation.
- Automated billing logic.
- Controlled billing edits with audit tracking.
- Complete administrative oversight.

# Tab 4**Counseling Workflows – Three Operational Scenarios (With QR Registration)**

## **Overview**

The system supports three structured counseling scenarios:

1.  Group Counseling (Incident-Based Group Session with QR Invitation)
2.  Class Counseling (Mock Incident-Based Class Session with QR Invitation)
3.  Independent Counseling (Individual Sessions BIRP per Group combined with the incident report with SOAP format )

For **Group** and **Class Counseling**, the Counselor may invite students using a **QR Code**.  
Students scan the QR code and enter their own information (Name, Student ID, Class), and the system automatically registers them for the session.

Each scenario defines how incidents are created, how participants are added, how BIRP reports are generated, and how billing is processed.

# **Scenario 1: Group Counseling (Incident-Based Group Session)**

## **Description**

In this scenario, the **Counselor reports a real incident involving more than one student** and conducts a **single group counseling session** for all participants. A **single consolidated BIRP report** is generated for the entire group.

⚠️ In this scenario, students are added directly through the incident report.  
No QR code is used.

## **Functional Flow**

### Step 1: Incident Reporting

1.  Counselor logs into the system.
2.  Counselor creates an **Incident Report**.
3.  Counselor selects **more than one student** as participants.
4.  System generates a **SOAP-format incident report**.
5.  Incident is saved or submitted.

### Step 2: Start Group Counseling

1.  Counselor selects **Start Counseling** from the incident.
2.  System detects multiple participants.
3.  Session type automatically becomes **Group Counseling**.
4.  Participant list is automatically carried forward from the incident.

### Step 3: Conduct Group Counseling

1.  Counseling Bot initiates structured prompts.
2.  Counselor provides:

- Behavior (group-level summary)
- Intervention (group strategies used)
- Response (overall group engagement)
- Plan (collective follow-up actions)

### Step 4: Report & Billing

1.  System generates **one consolidated Group BIRP Report**.
2.  System auto-generates group billing code based on:

- Session duration
- Session type
- Counselor role

1.  Counselor may edit billing code (mandatory reason required).
2.  Report submitted to Admin.

**Output**

- 1 Incident Report (SOAP)
- 1 Group Counseling BIRP Report
- 1 Group Billing Code

**Images:**

# **Scenario 2: Class Counseling (Mock Incident-Based with QR Invitation)**

## **Description**

The Counselor selects a Mock Incident and conducts a Class-Level Counseling Session. Students register through QR code. A single class-level BIRP report is generated.

## **Functional Flow**

### Step 1: Select Mock Incident

1.  Counselor selects Mock Incident for Class Session.
2.  Incident marked as Class-Level.

### Step 2: Generate QR Code

1.  Counselor creates Class Counseling Session.
2.  The system generates session ID.
3.  The system generates QR Code for the class.
4.  Counselor displays QR to class.

### Step 3: Student Registration

1.  Students scan QR code.
2.  Students enter:
    - Name
    - Student ID
    - Class
3.  System validates entries.
4.  Students added to the session participant list.

### Step 4: Conduct Class Counseling

1.  Counseling Bot collects:

- Behavior (class-wide issue)
- Intervention (workshop/discussion)
- Response (overall class engagement)
- Plan (follow-up awareness program)

### Step 5: Report & Billing

1.  The system generates one Class BIRP Report.
2.  System auto-generates class/group billing code.
3.  Billing code editable with mandatory reason.
4.  Report submitted to Admin.

## **Output**

- 1 Mock Incident
- 1 Class-Level BIRP Report
- 1 Class Billing Code
- QR-based student participation list

**Images:**

 

# **Scenario 3: Independent Counseling (Individual Sessions per Group)**

## **Description**

The Counselor reports an incident involving multiple participants and conducts group counseling sessions while reporting the incident.

QR code may optionally be used to confirm participant presence.

## **Functional Flow**

### Step 1: Incident Reporting

1.  Counselor creates Incident Report.
2.  Multiple students selected or registered via QR.
3.  The system generates incident report.

### Step 2: Independent Counseling Mode

1.  Counselor reports a group counseling session.
2.  System processes participants collectively as a group under one session.

### Step 3: Group Sessions

1.  For all students in a group
    - Counseling Bot opens individual session (separate drafts creation).
    - Counselor enters:
        - Behavior (individual)
        - Intervention
        - Response
        - Plan
    - System generates separate BIRP report.

### Step 4: Billing

1.  The system generates individual billing code per student.
2.  Billing code editable with reason.
3.  Each report is submitted separately.

## **Output**

- 1 Incident Report
- Group BIRP Reports
- Group Billing Codes
- Optional QR attendance record

## **Images:**

 

# **Business Rules (QR-Based Sessions)**

- QR code must be session-specific and secure.
- QR link must expire after session closes.
- Duplicate student entries must be prevented.
- Students must enter their own information.
- Minimum participants required for group sessions.
- Billing edits require mandatory reason.
- All QR registrations must be audit logged.

# **Status Lifecycle**

Draft → Submitted → Pending Admin Review →  
Approved / Rejected / Revision Requested → Resubmitted → Approved

# **Summary**

The system supports:

- Group Counseling with QR-based student registration.
- Class Counseling using Mock Incident + QR.
- Independent Counseling with separate reports for a group

QR functionality enables students to self-register securely, ensuring accurate participant tracking and automated BIRP documentation with controlled billing logic and administrative oversight.
