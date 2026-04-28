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

## Task 1: Manage Users using Dynamic groups

Dynamic groups in Microsoft Entra ID automatically manage membership based on user or device attributes. This eliminates the need to manually add or remove members when attributes change.

1. Open a new tab in the browser and navigate to **Microsoft Entra admin center** using below link.

   ```
   https://entra.microsoft.com\
   ```

1. If prompted, provide the credentials below:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>

1. Take a moment to familiarize yourself with Microsoft Entra admin center.

   ![](./Images/Adminportal.png)

1. In the left pane, expand **Entra ID (1)** and select **Groups (2)** then click on **New group (3)**.

   ![](./Images/ETS114.png)

1. On the **New Group** page, configure the following:

   - **Group type**: Security **(1)**
   - **Group name**: `IT-Department` **(2)**
   - **Group description**: `Dynamic group for IT Department members` **(3)**
   - **Membership type**: Dynamic User **(4)**

1. Under **Dynamic user members**, click **Add dynamic query (5)**.

   ![](./Images/ETS116.png)

1. In the **Dynamic membership rules** page, configure the following rules and click on **Save (5)**.

   - **Property**: `department` **(1)**
   - **Operator**: `Equals` **(2)**
   - **Value**: `IT`**(3)**

   In the Rule syntax, the rule expression should like below **(4)**:
   ```
   (user.department -eq "IT")
   ```
   ![](./Images/ETS117.png)

1. Click **Create**.

   ![](./Images/ETS118.png)

1. Click **+ New group** again to create another dynamic group.

   ![](./Images/ETS119.png)

1. On the **New Group** page, configure the following:
   - **Group type**: Security **(1)**
   - **Group name**: `Managers` **(2)**
   - **Group description**: `Dynamic group for all managers` **(3)**
   - **Membership type**: Dynamic User **(4)**

1. Under **Dynamic user members**, click **Add dynamic query (5)**.

   ![](./Images/ETS1110.png)

1. In the **Dynamic membership rules** page, click on **Edit (1)** then configure the following rules **(2)** and click on **Save (3)**.
   ```
   (user.jobTitle -contains "Manager")
   ```
   ![](./Images/ETS1111.png)

1. Click **Create**.

   ![](./Images/ETS1112.png)

1. In the left pane, go to **Users**, click on **ADUser1** to open the profile.

   ![](./Images/ETS1113.png)

1. Click **Edit properties**.

   ![](./Images/ETS1114.png)

1. Select **Job Information (1)** and set the following attributes and then click on **Save (4)**

   - **Job title**: `IT Manager` **(2)**
   - **Department**: `IT` **(3)**

   ![](./Images/ETS1115.png)

1. Navigate back to **Groups (1)**, select **All groups (2)** and Click on **IT-Department (3)**.

   ![](./Images/ETS1116.png)

1. Select **Members**, verify that **ADUser1** now added to the group dynamically.

   ![](./Images/ETS1117.png)
   >**Note:** If user is not appeared then wait a few minutes and then click Refresh.

1. Repeat for **Dynamic-Managers** to confirm the user with `jobTitle = IT Manager` appears.

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

Microsoft Entra Lifecycle Workflows automate identity processes across the employee lifecycle — from onboarding (joiner) to role changes (mover) to offboarding (leaver). This replaces manual HR-driven processes and reduces the risk of orphaned accounts.

### Task 2.1: Configure Onboarding Workflow

In this task, you are going to create onboarding workflow using the built in templates

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**. Then click on **+ Create workflow (3)**

   ![](./Images/ETS121.png)

1. You will see the Lifecycle Workflows dashboard with template categories: **Joiner**, **Mover**, and **Leaver**. Select **Onboard new hire employee**.

   ![](./Images/ETS122.png)

1. On the **Basics** page, provide Name as **Onboard New Employee - IT Department (1)**. and then click **Next: Configure (2)**.

   ![](./Images/ETS122-1.png)

1. On the **Configure** page, Make the rule department value as **IT (1)** then click **Next: Review tasks (2)**.

   ![](./Images/ETS123.png)

1. On the **Review tasks** page, you will see pre-configured tasks. Click **+ Add task (1)** to add additional tasks.

1. Select **Generate TAP and send email (2)** then click on **Add (3)**.

   ![](./Images/ETS124.png)

1. Click on **Add user to group** then click on **0 group selected**

   ![](./Images/ETS125.png)

1. Select **New joiners (1)** group and the click on **Select**

   ![](./Images/ETS126.png)

1. Click on **Save (1)**.

 Review the order of tasks. Drag tasks to reorder them if needed. Recommended order:
   1. Enable user account
   2. Send welcome email
   3. Add user to groups
   4. Generate Temporary Access Pass

1. Click **Next: Review + create (2)**.

   ![](./Images/ETS127.png)

1. Review the workflow configuration summary and click **Create**.

   ![](./Images/ETS128.png)

### Task 2.2: Configure Provisioning and License Handling

In this task you will assign the license and application provisioning to the user

1. Open a new tab in the browser and navigate to **M365 admin center** using below link.

   ```
   https://admin.microsoft.com/Adminportal/Home?referrer=entra#/licenses
   ```
   ![](./Images/ETS1210.png)

1. Expand **Team & groups (1)** then select **Active teams & groups (2)**.

1. Click on **security groups (3)**. Select the **IT-Department (4)** group created in Task 1.

   ![](./Images/ETS1211.png)

1. Select **License and apps (1)** then select **Office 365 E1 (2)** and click on **Save changes (3)**.

   ![](./Images/ETS1212.png)

   > **Note:** With group-based licensing, any user added to **IT-Department** (manually or through the dynamic rule) will automatically receive the assigned licenses.

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

Custom task extensions allow you to extend Lifecycle Workflows with custom business logic using Azure Logic Apps. This is useful for scenarios such as creating IT tickets, sending notifications to external systems, or triggering custom workflows.

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**.

1. Select **Custom extensions (3)**, then click on **+ Add a custom extension (4)**.

   ![](./Images/ETS1213.png)

1. On the Basics tab, Provide the below details then click on **Next Task behavior (3)**

      - **Name**: **(1)**
      - **Description**:**(2)**

1. On the Task behavior, leave it as default and click on **Next Details**

   ![](./Images/ETS1215.png)

1. On the Details page, Provide the below details and click on **Create logoc app(4)**
      - **Subscription**: Leave it as default **(1)**
      - **Resource group**: <inject key="Resourcegroup"></inject> **(2)**
      - **Logic app name**: `custom-task-workflow` **(3)**
   ![](./Images/ETS1216.png)

1. Once the deployment is successful, click on **Next Review + create** and then click on **Create**.

   ![](./Images/ETS1217.png)


1. Open a new browser tab and navigate to the Azure portal using below link

   ```
   https://portal.azure.com
   ```

1. Search for **Logic Apps** and select it.

   ![](./Images/ETS1218.png)

1.  Click on **custom-task-workflow** and then select **Logic app designer**.

   ![](./Images/ETS1219.png)

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

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**.

1. Select **Workflows (3)**, then click on **Onboard New Employee - IT Department (4)**.

   ![](./Images/ETS1220.png)

1. Click **Tasks (1)** in the workflow menu and then click **+ Add task (2)**.

   ![](./Images/ETS1221.png)

1. Select **Run a Custom Task Extension (1)** from the task list and click on **Save (2)**.

   ![](./Images/ETS1222.png)

1. Click on **Run a Custom Task Extension (1)** , Select custom extension as **New joinee onboarding (2)** then click on **Save(3)**.

   ![](./Images/ETS1223.png)

1. Then Click **Save**.

   ![](./Images/ETS1223.png)

1. Now go to **Overview (1)** and select **Run on demand (2)**. 

   ![](./Images/ETS1223.png)

3. Click on **+ Select user** to simulate onboarding for.

4. Click **Run workflow**.

5. After a few minutes, check the **Workflow execution history** to verify:
   - The workflow ran successfully
   - The custom task extension task shows a **Completed** status

6. Check the email inbox (or Logic App run history) to confirm the notification was sent.

### Task 2.4: Configure Offboarding Workflow

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**. Then click on **+ Create workflow (3)**

   ![](./Images/ETS121.png)

1. Scroll down and select the **Offboard an employee** template under the **Leaver** category.

   ![](./Images/ETS1251.png)

1. On the **Basics** page, leave it as default and click **Next: Configure**.

   ![](./Images/ETS1252.png)

1. On the **Configure** page, Make the rule department value as **IT (1)** then click **Next: Review tasks (2)**.

   ![](./Images/ETS1253.png)

1. On the **Review tasks** page, you will see pre-configured tasks. Click **+ Add task (1)** to add additional tasks.

1. Select **Remove all licenses for user  (2)** then click on **Add (3)** and click **Next: Review tasks (4)**.

   ![](./Images/ETS1254.png)

 Review the order of tasks. Drag tasks to reorder them if needed. Recommended order:
   1. Disable user account
   2. Remove user from all groups
   3. Remove user from all Teams
   4. Remove all licenses for user

1. Click **Create**.

   ![](./Images/ETS1255.png)

1. Now select **Offboard an employee** and click on **Run on demand**.

   ![](./Images/ETS1256.png)

1. Click on **+ Select users** then select **ADUser3** and click **Select**.

   ![](./Images/ETS1257.png)

1. Now click on **Run workflow**.

   ![](./Images/ETS1258.png)

7. Verify:
   - The user account is disabled
   - The user has been removed from groups
   - Licenses have been removed

8. Navigate to the user profile to confirm the status changes.

---

## Task 3: Setting Up Access Reviews

Access Reviews in Microsoft Entra ID Governance enable organizations to efficiently manage group memberships, application assignments, and privileged role assignments. Reviewers can approve or deny access based on actual business need.

1. In the Microsoft Entra admin center, navigate to **ID Governance** and select **Access reviews** then click on **+ Access reviews**

   ![](./Images/ETS1261.png)

1. Select **Review access to a resource type**

   ![](./Images/ETS1262.png)

1. On the **Review type** page, provide the below details:
      - Select what to review :**Teams + Groups (1)**
      - Review scope : **Select Teams + groups (2)**
      - Group : click on **+ Select groups (3)**

   ![](./Images/ETS1263.png)

1. Select **IT-Department (1)** and click on **Select (2)**.

   ![](./Images/ETS1264.png)

1. Select **All users (1)** on the scope and click on **Next Reviews (2)**.

   ![](./Images/ETS1265.png)

1. In the select reviewers drop down, click on **Selected user(s) or group(s)**

   ![](./Images/ETS1266.png)

1. Then click on **+ select reviwers (1)** and select **ODL_User <inject key="DeploymentID"></inject> (2)** then clcik on **select (3)**.

   ![](./Images/ETS1267.png)

1. Now select review occurence as **Weekly (1)** then click on **Next settings (2)**

   ![](./Images/ETS1268.png)

1. Click on **Next Review + Create**

1. On the **Review + create** page, provide Review name as **Access review (1)** and click on **Create (2)**.

   ![](./Images/ETS1269.png)

1. The designated reviewer will receive an email notification to perform the review.

1. Open a new tab and navigate to outlook using below link

   ```
   https://outlook.office.com/
   ```
1. If prompted to sign, provide the credentials below:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>

1. You will receive a mail. Select the mail and click on **Start review**.

   ![](./Images/ETS1311.png)

1. It will navigate to my access portal.

1. Now select **ADUser1** and click on **Deny**. Optionally, provide a justification in the **Reason** field and **Sumbit**.

   ![](./Images/ETS1312.png)
   ![](./Images/ETS1313.png)

1. Navigate back to Entra admin portal and select **Access review**

   ![](./Images/ETS1314.png)

1. Now click on **Results** to see the denied log.

   ![](./Images/ETS1315.png)

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
