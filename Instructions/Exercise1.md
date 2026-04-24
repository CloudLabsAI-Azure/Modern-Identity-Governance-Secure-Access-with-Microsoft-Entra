# Exercise 1: Microsoft Entra ID Governance (IGA)

**Estimated Duration: 90 minutes**

## Overview

In this exercise, you will explore Microsoft Entra ID Governance capabilities including Identity Lifecycle Management, Access Reviews, and Conditional Access Policies. Entra ID Governance helps organizations balance security and employee productivity by ensuring the right people have the right access at the right time.

## Prerequisites

- An active Microsoft Entra ID (Azure Active Directory) tenant
- A user account with the **Global Administrator** or **Identity Governance Administrator** role
- Microsoft Entra ID P2 or Microsoft Entra Governance license (required for Lifecycle Workflows and Access Reviews)
- At least 2–3 test user accounts in your tenant

---

## Task 1: Manage Users and Groups

**Estimated Duration: 15 minutes**

### Overview

Dynamic groups in Microsoft Entra ID automatically manage membership based on user or device attributes. This eliminates the need to manually add or remove members when attributes change.

### Step 1: Create a Dynamic Security Group

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) using your Global Administrator credentials.

2. In the left navigation pane, expand **Identity** and select **Groups** > **All groups**.

   ![Groups navigation](Images/ex1-task1-groups-nav.png)

3. Click **+ New group** at the top of the Groups blade.

4. On the **New Group** page, configure the following:
   - **Group type**: Security
   - **Group name**: `Dynamic-Department-IT`
   - **Group description**: `Dynamic group for IT Department members`
   - **Membership type**: Dynamic User

5. Under **Dynamic user members**, click **Add dynamic query**.

   ![New group configuration](Images/ex1-task1-new-group.png)

6. In the **Dynamic membership rules** editor, configure the following rule:
   - **Property**: `department`
   - **Operator**: `Equals`
   - **Value**: `IT`

   The rule expression should read:
   ```
   (user.department -eq "IT")
   ```

7. Click **Save** to save the dynamic membership rule.

8. Back on the **New Group** page, click **Create**.

   > **Note:** Dynamic group membership updates can take up to 24 hours for large tenants. In lab environments, updates typically occur within a few minutes.

### Step 2: Create a Second Dynamic Group by Job Title

1. Click **+ New group** again to create another dynamic group.

2. Configure the following:
   - **Group type**: Security
   - **Group name**: `Dynamic-Managers`
   - **Group description**: `Dynamic group for all managers`
   - **Membership type**: Dynamic User

3. Click **Add dynamic query** and enter the following rule:
   - **Property**: `jobTitle`
   - **Operator**: `Contains`
   - **Value**: `Manager`

   The expression should read:
   ```
   (user.jobTitle -contains "Manager")
   ```

4. Click **Save**, then click **Create**.

### Step 3: Assign Users to Groups and Validate Membership

1. In the left navigation pane, go to **Identity** > **Users** > **All users**.

2. Select a test user (e.g., **Alex Johnson**) and click on their name to open their profile.

3. Click **Edit properties** (pencil icon) and set the following attributes:
   - **Department**: `IT`
   - **Job title**: `IT Manager`

4. Click **Save**.

5. Navigate back to **Groups** > **All groups** and open **Dynamic-Department-IT**.

6. Under **Members**, wait a few minutes and then click **Refresh**. Verify that the test user with `department = IT` now appears in the group.

7. Repeat for **Dynamic-Managers** to confirm the user with `jobTitle = IT Manager` appears.

   > **Tip:** You can also use **Validate Rules** on the dynamic membership rules page to check if a specific user would match the rule before the automatic sync completes.

### Step 4: Configure Advanced Group Membership Rules

1. Open the **Dynamic-Department-IT** group and navigate to **Dynamic membership rules**.

2. Click **Edit** to modify the existing rule and add an additional condition using **AND** logic:

   ```
   (user.department -eq "IT") and (user.accountEnabled -eq true)
   ```

3. Click **Save** to apply the updated rule.

   > **Note:** This ensures only active (enabled) IT department users are included in the group, which is an important security consideration.

4. To validate the rule:
   - Click **Validate Rules**
   - Search for and select a test user
   - Review whether the user matches the rule criteria

---

## Task 2: Entra ID Lifecycle Management

### Overview

Microsoft Entra Lifecycle Workflows automate identity processes across the employee lifecycle — from onboarding (joiner) to role changes (mover) to offboarding (leaver). This replaces manual HR-driven processes and reduces the risk of orphaned accounts.

---

### Task 2.1: Configure Onboarding Workflow

**Estimated Duration: 15 minutes**

#### Step 1: Navigate to Lifecycle Workflows

1. In the Microsoft Entra admin center, expand **Identity Governance** in the left navigation pane.

2. Select **Lifecycle workflows**.

   ![Lifecycle workflows navigation](Images/ex1-task2-lifecycle-nav.png)

3. You will see the Lifecycle Workflows dashboard with template categories: **Joiner**, **Mover**, and **Leaver**.

#### Step 2: Create an Onboarding Workflow from Template

1. Click **+ Create workflow** or **New workflow**.

2. In the **Create a workflow** wizard, select the **Onboard new hire employee** template under the **Joiner** category.

3. Click **Next: Basics**.

4. On the **Basics** page, configure:
   - **Name**: `Onboard New Employee - Standard`
   - **Description**: `Automated onboarding for new employees joining the organization`
   - **Enable schedule**: Toggle to **On**

5. Click **Next: Configure**.

#### Step 3: Configure Workflow Trigger

1. On the **Configure** page, review the trigger settings:
   - **Trigger type**: Attribute changes
   - **Trigger attribute**: `employeeHireDate`
   - **Days before/after hire date**: Set to **0** (triggers on the hire date itself)

   > **Note:** You can configure this to run days before the actual start date, which is useful for pre-provisioning access.

2. Optionally, configure **Scope** to target specific users by adding conditions:
   - Click **Add condition**
   - **Property**: `department`
   - **Operator**: `Equals`
   - **Value**: `IT`

   This limits the workflow to only process IT department new hires.

3. Click **Next: Review tasks**.

#### Step 4: Configure Automated Onboarding Tasks

1. On the **Review tasks** page, you'll see pre-configured tasks. Click **+ Add task** to add additional tasks.

2. Add the following tasks (if not already present):

   **Task 1 – Send Welcome Email:**
   - Select **Send welcome email to new hire**
   - Configure the email template and sender

   **Task 2 – Add User to Group:**
   - Select **Add user to groups**
   - Click **Select groups** and choose **Dynamic-Department-IT**

   **Task 3 – Enable User Account:**
   - Select **Enable user account**

   **Task 4 – Generate Temporary Access Pass:**
   - Select **Generate Temporary Access Pass and send via email to manager**
   - This provides the new employee a secure way to sign in for the first time

   ![Onboarding tasks configuration](Images/ex1-task2-onboarding-tasks.png)

3. Review the order of tasks. Drag tasks to reorder them if needed. Recommended order:
   1. Enable user account
   2. Add user to groups
   3. Generate Temporary Access Pass
   4. Send welcome email

4. Click **Next: Review + create**.

5. Review the workflow configuration summary and click **Create**.

---

### Task 2.2: Configure Provisioning and License Handling

**Estimated Duration: 15 minutes**

#### Step 1: Configure Automatic License Assignment via Group-Based Licensing

1. In the Microsoft Entra admin center, navigate to **Identity** > **Groups** > **All groups**.

2. Select the **Dynamic-Department-IT** group created in Task 1.

3. In the group menu, click **Licenses**.

4. Click **+ Assignments**.

5. In the **Assign licenses** panel, select the appropriate licenses:
   - **Microsoft 365 E3** (or the license available in your tenant)
   - **Microsoft Entra ID P2** (for advanced governance features)

6. For each selected license, review the service plans and disable any plans not needed for IT users if required.

7. Click **Save**.

   > **Note:** With group-based licensing, any user added to **Dynamic-Department-IT** (manually or through the dynamic rule) will automatically receive the assigned licenses.

#### Step 2: Configure Application Provisioning

1. In the Microsoft Entra admin center, navigate to **Identity** > **Applications** > **Enterprise applications**.

2. Click **+ New application**.

3. Search for **ServiceNow** (or another application available for provisioning in your tenant) and select it.

4. Click **Create** to add it to your tenant.

5. In the ServiceNow application page, select **Provisioning** from the left menu.

6. Click **Get started** or change **Provisioning Mode** to **Automatic**.

7. In the **Admin Credentials** section, provide the required connection details:
   - **Tenant URL**: Your ServiceNow instance URL (e.g., `https://instance.service-now.com/api/now/table/sys_user`)
   - **Secret Token**: ServiceNow admin credentials or OAuth token

   > **Lab Note:** In a lab environment, you may use a provisioning-enabled application already configured in your tenant. If ServiceNow is not available, you can use **AWS Single Sign-On** or **Box** as alternatives.

8. Click **Test Connection** to verify connectivity.

9. Under **Mappings**, click **Provision Microsoft Entra ID Users** to review attribute mappings.

10. Under **Settings**, configure:
    - **Provisioning Status**: On
    - **Scope**: Sync only assigned users and groups

11. Click **Save**.

12. Navigate to **Users and groups** in the application and click **+ Add user/group**.

13. Select the **Dynamic-Department-IT** group and click **Assign**.

   > **Result:** All users in the Dynamic-Department-IT group will now be automatically provisioned into the target application.

#### Step 3: Validate License Assignment and Provisioning

1. Navigate to **Identity** > **Users** > **All users** and select a user in the IT department.

2. Click **Licenses** in the user's menu to verify the Microsoft 365 license has been assigned via group inheritance.

3. Navigate back to the provisioning application and select **Provisioning logs** to review the provisioning activity:
   - Filter by **Status**: Success or Failure
   - Review the **Action** column: Create, Update, or Disable
   - Click on individual log entries to see detailed provisioning steps

   ![Provisioning logs](Images/ex1-task2-provisioning-logs.png)

---

### Task 2.3: Implement Custom Extensions

**Estimated Duration: 15 minutes**

#### Overview

Custom task extensions allow you to extend Lifecycle Workflows with custom business logic using Azure Logic Apps. This is useful for scenarios such as creating IT tickets, sending notifications to external systems, or triggering custom workflows.

#### Step 1: Create an Azure Logic App

1. Open a new browser tab and navigate to the [Azure portal](https://portal.azure.com).

2. Search for **Logic Apps** and select it.

3. Click **+ Add** to create a new Logic App.

4. Configure the following:
   - **Subscription**: Select your subscription
   - **Resource Group**: Select or create a resource group (e.g., `rg-identity-lab`)
   - **Logic App name**: `la-lifecycle-onboarding-extension`
   - **Region**: Select your preferred region
   - **Plan type**: Consumption

5. Click **Review + create**, then **Create**.

6. Once deployed, click **Go to resource** and then click **Logic app designer**.

7. Select **When a HTTP request is received** as the trigger.

8. In the **Request Body JSON Schema**, paste the following schema:

   ```json
   {
     "type": "object",
     "properties": {
       "data": {
         "type": "object",
         "properties": {
           "subject": {
             "type": "object",
             "properties": {
               "id": { "type": "string" },
               "displayName": { "type": "string" },
               "mail": { "type": "string" }
             }
           }
         }
       }
     }
   }
   ```

9. Add a new step: Click **+ New step** and search for **Send an email (V2)** (using Office 365 Outlook connector).

10. Configure the email action:
    - **To**: `helpdesk@yourdomain.com` (replace with your test email)
    - **Subject**: `New Employee Onboarding - Action Required`
    - **Body**: 
      ```
      A new employee has been onboarded:
      Name: @{triggerBody()?['data']?['subject']?['displayName']}
      Email: @{triggerBody()?['data']?['subject']?['mail']}
      
      Please create the necessary IT equipment ticket.
      ```

11. Click **Save** to save the Logic App.

12. Copy the **HTTP POST URL** from the trigger — you will need this for the custom extension.

#### Step 2: Register the Custom Extension in Lifecycle Workflows

1. Return to the Microsoft Entra admin center.

2. Navigate to **Identity Governance** > **Lifecycle workflows** > **Custom task extensions**.

3. Click **+ Create a custom task extension**.

4. On the **Basics** page, configure:
   - **Name**: `IT Ticket Creation on Onboarding`
   - **Description**: `Creates an IT helpdesk ticket when a new employee is onboarded`

5. Click **Next: Details**.

6. On the **Details** page:
   - **Extension type**: Azure Logic App
   - **Subscription**: Select your Azure subscription
   - **Logic App**: Select `la-lifecycle-onboarding-extension`

7. Configure the **Behavior**:
   - **Launch and continue** (recommended for non-blocking operations like ticket creation)

8. Click **Next: Review + create**, then **Create**.

#### Step 3: Add the Custom Extension to the Onboarding Workflow

1. Navigate to **Identity Governance** > **Lifecycle workflows** > **Workflows**.

2. Open the **Onboard New Employee - Standard** workflow created in Task 2.1.

3. Click **Tasks** in the workflow menu.

4. Click **+ Add task**.

5. Select **Run a Custom Task Extension** from the task list.

6. In the configuration panel:
   - **Select a custom task extension**: Choose `IT Ticket Creation on Onboarding`

7. Set the task execution order to run after the **Enable user account** task.

8. Click **Save**.

#### Step 4: Test the Custom Extension

1. In the Lifecycle Workflows section, select the **Onboard New Employee - Standard** workflow.

2. Click **Run on demand** (or similar option to trigger a test run).

3. Select a test user to simulate onboarding for.

4. Click **Run workflow**.

5. After a few minutes, check the **Workflow execution history** to verify:
   - The workflow ran successfully
   - The custom task extension task shows a **Completed** status

6. Check the email inbox (or Logic App run history) to confirm the notification was sent.

   ![Custom extension execution](Images/ex1-task2-custom-extension.png)

---

### Task 2.4: Configure Offboarding Workflow

**Estimated Duration: 15 minutes**

#### Step 1: Create an Offboarding Workflow

1. In the Microsoft Entra admin center, navigate to **Identity Governance** > **Lifecycle workflows**.

2. Click **+ Create workflow** or **New workflow**.

3. Select the **Offboard an employee** template under the **Leaver** category.

4. Click **Next: Basics**.

5. Configure the following:
   - **Name**: `Offboard Employee - Standard`
   - **Description**: `Automated offboarding for departing employees`
   - **Enable schedule**: Toggle to **On**

6. Click **Next: Configure**.

#### Step 2: Configure Offboarding Trigger

1. On the **Configure** page, set the trigger:
   - **Trigger type**: Attribute changes
   - **Trigger attribute**: `employeeLeaveDateTime`
   - **Days before/after leave date**: Set to **0**

2. Add a scope condition if desired:
   - **Property**: `department`
   - **Operator**: `Not Equals`
   - **Value**: `Contractors`

   > This ensures contractors are handled through a separate workflow.

3. Click **Next: Review tasks**.

#### Step 3: Configure Offboarding Tasks

1. On the **Review tasks** page, add and configure the following tasks:

   **Task 1 – Remove User from All Groups:**
   - Select **Remove user from all groups**

   **Task 2 – Remove User from All Teams:**
   - Select **Remove user from all Teams**

   **Task 3 – Disable User Account:**
   - Select **Disable user account**

   **Task 4 – Remove User Licenses:**
   - Select **Remove all licenses for user**

   **Task 5 – Revoke User Sign-in Sessions:**
   - Select **Revoke user sign-in sessions**

   **Task 6 – Delete User Account** *(optional, typically after a retention period)*:
   - If required by policy, add **Delete user** task with a delay

   Recommended task order:
   1. Revoke user sign-in sessions
   2. Remove user from all Teams
   3. Remove user from all groups
   4. Disable user account
   5. Remove all licenses for user

   ![Offboarding tasks configuration](Images/ex1-task2-offboarding-tasks.png)

2. Click **Next: Review + create**.

3. Review the configuration and click **Create**.

#### Step 4: Test the Offboarding Workflow

1. Navigate to **Identity** > **Users** > **All users** and identify a test user to simulate offboarding.

2. Update the test user's **Employee leave date** attribute:
   - Open the user profile
   - Click **Edit properties**
   - Set **Employee leave date** to today's date or a past date

3. Return to **Identity Governance** > **Lifecycle workflows** > **Workflows**.

4. Open **Offboard Employee - Standard** and click **Run on demand**.

5. Select the test user and click **Run workflow**.

6. Wait a few minutes and then check the **Workflow execution history**.

7. Verify:
   - The user account is disabled
   - The user has been removed from groups
   - Licenses have been removed

8. Navigate to the user profile to confirm the status changes.

---

## Task 3: Setting Up Access Reviews

**Estimated Duration: 20 minutes**

### Overview

Access Reviews in Microsoft Entra ID Governance enable organizations to efficiently manage group memberships, application assignments, and privileged role assignments. Reviewers can approve or deny access based on actual business need.

### Step 1: Create an Access Review

1. In the Microsoft Entra admin center, navigate to **Identity Governance** > **Access reviews**.

2. Click **+ New access review**.

   ![New access review](Images/ex1-task3-access-review-new.png)

3. On the **Review type** page:
   - **Select what to review**: Teams + Groups
   - **Review scope**: Select teams + groups
   - Click **Select group(s)** and choose **Dynamic-Department-IT**
   - **Scope**: All users (or Guest users only, depending on your scenario)

4. Click **Next: Reviews**.

### Step 2: Configure Review Settings

1. On the **Reviews** page, configure:
   - **Select reviewers**: Selected user(s)
   - Click **Select reviewers** and add the appropriate manager or IT admin user
   - Alternatively, select **Group owners** as reviewers

2. Configure the schedule:
   - **Duration (in days)**: `14`
   - **Review recurrence**: Monthly (or Quarterly for less frequent reviews)
   - **Start date**: Today's date
   - **End**: Never (or specify an end date)

3. Click **Next: Settings**.

### Step 3: Configure Access Review Behavior Settings

1. On the **Settings** page, configure:

   **Upon completion settings:**
   - **Auto apply results to resource**: Toggle to **Enable**
   - **If reviewers don't respond**: Remove access (recommended for security)

   **Advanced settings:**
   - **Show recommendations**: Enable (helps reviewers make informed decisions)
   - **Require reason on approval**: Enable
   - **Email notifications**: Enable
   - **Reminders**: Enable
   - **Additional content for reviewer email**: Add a message such as:
     > "Please review the IT Department group membership. Remove access for any users who no longer require IT-level access."

2. Click **Next: Review + create**.

### Step 4: Create the Access Review

1. Review the access review configuration summary.

2. Click **Create**.

3. The access review will now appear in the **Access reviews** list.

   ![Access review created](Images/ex1-task3-access-review-created.png)

### Step 5: Perform the Access Review (Reviewer Experience)

1. The designated reviewer will receive an email notification to perform the review.

2. As the reviewer, navigate to [My Access portal](https://myaccess.microsoft.com).

3. Click on **Access reviews** in the left navigation pane.

4. Select the pending access review for **Dynamic-Department-IT**.

5. For each user listed, review their information:
   - Last sign-in date
   - Recommendations (if enabled)
   - User details

6. For each user, select one of the following actions:
   - **Approve**: The user should retain access
   - **Deny**: The user should be removed from the group
   - **Don't know**: Skip this decision (the policy setting will handle it)

7. Optionally, provide a justification in the **Reason** field.

8. Click **Submit** to submit your decisions.

### Step 6: Review Audit Logs and Completion Reports

1. Return to the Microsoft Entra admin center.

2. Navigate to **Identity Governance** > **Access reviews**.

3. Select the completed access review.

4. Click **Results** to view the decisions made:
   - **Approved**: Users whose access was approved
   - **Denied**: Users whose access was removed

5. To review audit logs, navigate to **Identity** > **Monitoring & health** > **Audit logs**.

6. Filter the audit logs:
   - **Category**: AccessReviews
   - **Date range**: Last 24 hours

7. Review the log entries for access review creation, decisions, and auto-applied results.

---

## Task 4: Implementing Conditional Access Policies

**Estimated Duration: 10 minutes**

### Overview

Conditional Access policies enforce security requirements based on conditions such as user identity, device compliance, location, and application being accessed. In this task, you will create a policy that requires MFA for cloud app access.

### Step 1: Create a Conditional Access Policy

1. In the Microsoft Entra admin center, navigate to **Protection** > **Conditional Access**.

2. Click **+ New policy** to create a new policy.

   ![New conditional access policy](Images/ex1-task4-ca-new-policy.png)

3. Set the **Name**: `Require MFA for IT Department - Cloud Apps`

### Step 2: Configure Policy Assignments

#### Users

1. Under **Assignments**, click **Users** (or **0 users and groups selected**).

2. Select **Select users and groups** > **Users and groups**.

3. Click **Select** and search for **Dynamic-Department-IT** group.

4. Click **Select** to confirm.

#### Cloud Apps or Actions

1. Click **Cloud apps or actions**.

2. Select **Cloud apps** > **Select apps**.

3. Click **Select** and choose:
   - **Office 365** (or **Microsoft 365**) — to protect Microsoft 365 applications
   - Click **Select** to confirm.

#### Conditions

1. Click **Conditions** to expand it.

2. Click **Locations**:
   - **Configure**: Yes
   - **Exclude**: Select **All trusted locations**
   - This means the policy applies when users are outside trusted locations.

3. Click **Done**.

4. Click **Device platforms** (optional):
   - **Configure**: Yes
   - **Include**: Any device
   - Click **Done**.

### Step 3: Configure Access Controls

1. Under **Access controls**, click **Grant**.

2. Select **Grant access** and check:
   - **Require multi-factor authentication**

3. Under multiple controls, select **Require all the selected controls**.

4. Click **Select**.

5. Optionally, under **Session**, click **Sign-in frequency**:
   - Select **Periodic reauthentication**
   - Set to **Every 8 hours**

   ![CA policy grant settings](Images/ex1-task4-ca-grant.png)

### Step 4: Enable and Save the Policy

1. Under **Enable policy**, select **Report-only** first to test without enforcement.

   > **Important:** Always test Conditional Access policies in **Report-only** mode before enabling them. This allows you to analyze the impact without affecting user access.

2. Click **Create** to save the policy.

### Step 5: Validate Policy Enforcement

1. To test the policy in Report-only mode:

   a. Open an **InPrivate/Incognito** browser window.
   
   b. Navigate to [https://portal.office.com](https://portal.office.com) and sign in as a test user from the **Dynamic-Department-IT** group.
   
   c. Complete the sign-in process.

2. Return to the Microsoft Entra admin center and navigate to **Identity** > **Monitoring & health** > **Sign-in logs**.

3. Find the recent sign-in for the test user.

4. Click on the sign-in event and go to the **Conditional Access** tab.

5. Review the policy evaluation results:
   - **Policy name**: Require MFA for IT Department - Cloud Apps
   - **Result**: Report-only: Would Require MFA (or similar)

6. Once satisfied with the report-only results, return to the policy and change **Enable policy** to **On**.

7. Click **Save** to enforce the policy.

   > **Warning:** Before enabling, ensure you have excluded at least one break-glass admin account from this policy to prevent lockout.

### Step 6: Review Sign-in Logs for Policy Effectiveness

1. Navigate to **Identity** > **Monitoring & health** > **Sign-in logs**.

2. Apply filters:
   - **Date**: Last 1 hour
   - **Conditional Access**: Failure (to see enforced denials) or Success (to see successful MFA completions)

3. Click on individual sign-in events and review:
   - **Conditional Access policies applied**
   - **Authentication methods used**
   - **Status** of MFA completion

---

## Task 5: Monitoring and Auditing with Entra ID (Optional/Read Only)

**Estimated Duration: 5 minutes**

> **Note:** This task is optional and read-only. It provides visibility into governance and security activity in your tenant.

### Step 1: Review Audit Logs

1. In the Microsoft Entra admin center, navigate to **Identity** > **Monitoring & health** > **Audit logs**.

2. Review the available filters:
   - **Date range**: Select last 7 days
   - **Service**: Identity Governance
   - **Category**: GroupManagement, UserManagement, or EntitlementManagement

3. Browse recent audit log entries to understand what governance activities have been recorded.

4. Click on any log entry to view:
   - Activity type
   - Initiated by (actor)
   - Target resources
   - Modified properties

### Step 2: Analyze Sign-in Logs

1. Navigate to **Identity** > **Monitoring & health** > **Sign-in logs**.

2. Review the sign-in activity across your tenant.

3. Apply filters to find interesting patterns:
   - **Status**: Failure (to identify failed sign-in attempts)
   - **Conditional Access**: Failure (to identify policy enforcement)

4. Click on a failed sign-in to review:
   - **Basic info**: IP address, location, device details
   - **Authentication Details**: Methods attempted
   - **Conditional Access**: Which policies blocked or allowed access

### Step 3: Explore Entra ID Reporting Dashboards

1. Navigate to **Identity** > **Overview** to see the tenant summary dashboard.

2. Explore the following built-in reports:
   - **Usage & insights**: Application usage, MFA adoption, Self-service password reset activity
   - **Identity Protection**: Risk events, risky users, risky sign-ins
   - **Access Reviews**: Review completion rates and outcomes

3. Navigate to **Identity Governance** > **Dashboard** (if available) to see an overview of:
   - Active access reviews
   - Pending entitlement management requests
   - Lifecycle workflow execution statistics

---

## Summary

In this exercise, you have:

- ✅ Created dynamic groups with attribute-based membership rules
- ✅ Configured automated onboarding and offboarding lifecycle workflows
- ✅ Set up automatic license assignment and application provisioning
- ✅ Integrated a custom Logic App extension into a lifecycle workflow
- ✅ Created and performed an access review for group membership
- ✅ Implemented a Conditional Access policy requiring MFA
- ✅ Reviewed audit logs and sign-in logs for governance visibility

These capabilities form the foundation of a modern Identity Governance and Administration (IGA) solution built on Microsoft Entra ID.

## Additional Resources

- [Microsoft Entra ID Governance Documentation](https://learn.microsoft.com/en-us/entra/id-governance/)
- [Lifecycle Workflows Overview](https://learn.microsoft.com/en-us/entra/id-governance/lifecycle-workflows-overview)
- [Access Reviews Documentation](https://learn.microsoft.com/en-us/entra/id-governance/access-reviews-overview)
- [Conditional Access Documentation](https://learn.microsoft.com/en-us/entra/identity/conditional-access/overview)
- [Group-Based Licensing](https://learn.microsoft.com/en-us/entra/identity/users/licensing-groups-assign)
