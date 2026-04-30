# Exercise 1: Microsoft Entra ID Governance (IGA)

**Estimated Duration: 90 minutes**

## Overview

In this exercise, you will explore Microsoft Entra ID Governance capabilities including Identity Lifecycle Management, Access Reviews, and Conditional Access Policies. Entra ID Governance helps organizations balance security and employee productivity by ensuring the right people have the right access at the right time.

## Prerequisites

- An active Microsoft Entra ID (Azure Active Directory) tenant
- A user account with the **Global Administrator** or **Identity Governance Administrator** role
- Microsoft Entra ID P2 or Microsoft Entra Governance license (required for Lifecycle Workflows and Access Reviews)
- At least 2–3 test user accounts in your tenant

## Task 1: Manage Users using Dynamic groups

Dynamic groups in Microsoft Entra ID automatically manage membership based on user or device attributes. This eliminates the need to manually add or remove members when attributes change.

1. Open a new tab in the browser and navigate to **Microsoft Entra admin center** using below link.

   ```
   https://entra.microsoft.com
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

1. Check for **Managers** group to confirm the `ADUser1` added as a member in the group.

   ![](./Images/ETS1118.png)

1. You can also validate the users by using **Validate rules**.

1. Click on **Dynamic membership rules (1)** and select **Validate rules (2)**.

1. Click on **+ Add user (3)**, select **ADUser2 (4)** and check the **validation (5)** details.

   ![](./Images/ETS1119.png)

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

1.  Review the order of tasks. Drag tasks to reorder them if needed. Recommended order:
      - Enable user account
      - Send welcome email
      - Add user to groups
      - Generate Temporary Access Pass

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

      - **Name**: Onboarding email notification **(1)**
      - **Description**: Send the onboarding email notification to New joinee **(2)**.

      ![](./Images/ETS1214.png)

1. On the Task behavior tab, leave it as default and click on **Next Details**

   ![](./Images/ETS1215.png)

1. On the Details page, Provide the below details and click on **Create logoc app(4)**
      - **Subscription**: Leave it as default **(1)**
      - **Resource group**: <inject key="Resourcegroup"></inject> **(2)**
      - **Logic app name**: `custom-task-workflow` **(3)**
   ![](./Images/ETS1216.png)

1. Once the deployment is successful, click on **Next Review + create** and then click on **Create**.

   ![](./Images/ETS1217.png)

1. Now go to custom extensions and click on **custom-task-workflow**

   ![](./Images/ETS1228.png)

1. Copy the below code. Click on **Logic app code view (1)** then select all the existing code with **Ctrl + A** and then clcik **Ctrl + V** to paste the code and then click on **Save (3)** to save the code

   ```
   {
    "definition": {
        "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
        "contentVersion": "1.0.0.0",
        "triggers": {
            "manual": {
                "type": "Request",
                "kind": "Http",
                "inputs": {
                    "schema": {
                        "properties": {
                            "data": {
                                "properties": {
                                    "callbackUriPath": {
                                        "description": "CallbackUriPath used for Resume Action",
                                        "title": "Data.CallbackUriPath",
                                        "type": "string"
                                    },
                                    "subject": {
                                        "properties": {
                                            "displayName": {
                                                "description": "DisplayName of the Subject",
                                                "title": "Subject.DisplayName",
                                                "type": "string"
                                            },
                                            "email": {
                                                "description": "Email of the Subject",
                                                "title": "Subject.Email",
                                                "type": "string"
                                            },
                                            "id": {
                                                "description": "Id of the Subject",
                                                "title": "Subject.Id",
                                                "type": "string"
                                            },
                                            "manager": {
                                                "properties": {
                                                    "displayName": {
                                                        "description": "DisplayName parameter for Manager",
                                                        "title": "Manager.DisplayName",
                                                        "type": "string"
                                                    },
                                                    "email": {
                                                        "description": "Mail parameter for Manager",
                                                        "title": "Manager.Mail",
                                                        "type": "string"
                                                    },
                                                    "id": {
                                                        "description": "Id parameter for Manager",
                                                        "title": "Manager.Id",
                                                        "type": "string"
                                                    }
                                                },
                                                "type": "object"
                                            },
                                            "userPrincipalName": {
                                                "description": "UserPrincipalName of the Subject",
                                                "title": "Subject.UserPrincipalName",
                                                "type": "string"
                                            }
                                        },
                                        "type": "object"
                                    },
                                    "task": {
                                        "properties": {
                                            "displayName": {
                                                "description": "DisplayName for Task Object",
                                                "title": "Task.DisplayName",
                                                "type": "string"
                                            },
                                            "id": {
                                                "description": "Id for Task Object",
                                                "title": "Task.Id",
                                                "type": "string"
                                            }
                                        },
                                        "type": "object"
                                    },
                                    "taskProcessingResult": {
                                        "properties": {
                                            "createdDateTime": {
                                                "description": "CreatedDateTime for TaskProcessingResult Object",
                                                "title": "TaskProcessingResult.CreatedDateTime",
                                                "type": "string"
                                            },
                                            "id": {
                                                "description": "Id for TaskProcessingResult Object",
                                                "title": "TaskProcessingResult.Id",
                                                "type": "string"
                                            }
                                        },
                                        "type": "object"
                                    },
                                    "workflow": {
                                        "properties": {
                                            "displayName": {
                                                "description": "DisplayName for Workflow Object",
                                                "title": "Workflow.DisplayName",
                                                "type": "string"
                                            },
                                            "id": {
                                                "description": "Id for Workflow Object",
                                                "title": "Workflow.Id",
                                                "type": "string"
                                            },
                                            "workflowVersion": {
                                                "description": "WorkflowVersion for Workflow Object",
                                                "title": "Workflow.WorkflowVersion",
                                                "type": "integer"
                                            }
                                        },
                                        "type": "object"
                                    }
                                },
                                "type": "object"
                            },
                            "source": {
                                "description": "Context in which an event happened",
                                "title": "Request.Source",
                                "type": "string"
                            },
                            "type": {
                                "description": "Value describing the type of event related to the originating occurrence.",
                                "title": "Request.Type",
                                "type": "string"
                            }
                        },
                        "type": "object"
                    }
                }
            }
        },
        "actions": {
            "HTTP": {
                "runAfter": {},
                "type": "Http",
                "inputs": {
                    "uri": "https://graph.microsoft.com/beta@{triggerBody()?['data']?['callbackUriPath']}",
                    "method": "POST",
                    "body": {
                        "data": {
                            "operationStatus": "Completed"
                        },
                        "source": "sample",
                        "type": "lifecycleEvent"
                    },
                    "authentication": {
                        "audience": "https://graph.microsoft.com",
                        "type": "ManagedServiceIdentity"
                    }
                }
            },
            "Send_an_email_(V2)": {
                "runAfter": {
                    "HTTP": [
                        "Succeeded"
                    ]
                },
                "type": "ApiConnection",
                "inputs": {
                    "host": {
                        "connection": {
                            "name": "@parameters('$connections')['office365']['connectionId']"
                        }
                    },
                    "method": "post",
                    "body": {
                        "To": "@triggerBody()?['data']?['subject']?['email']",
                        "Subject": "Welcome Onboarding",
                        Body": "<p class=\"editor-paragraph\">Hello Dear @{triggerBody()?['data']?['subject']?['displayName']},<br><br>Welcome to your first day at work.<br><br>Attached, you will find the information for your first login:<br>Use the following username to log in:<br>@{triggerBody()?['data']?['subject']?['email']}<br><br>Set it up here: <a href=\"https://portal.office.com\" class=\"editor-link\">portal.office.com</a><br><br>If you have any questions, please contact your manager:<br>@{triggerBody()?['data']?['subject']?['manager']?['displayName']}<br>@{triggerBody()?['data']?['subject']?['manager']?['email']}<br><br>You can reach our helpdesk at 00011112222<br>Welcome “On-Board”!</p>",
                        "Cc": "@triggerBody()?['data']?['subject']?['manager']?['email']",
                        "Importance": "Normal"
                    },
                    "path": "/v2/Mail"
                }
            }
        },
        "outputs": {},
        "parameters": {
            "$connections": {
                "type": "Object",
                "defaultValue": {}
            }
        }
    },
    "parameters": {
        "$connections": {
            "type": "Object",
            "value": {
                "office365": {
                    "id": "/subscriptions/ecbef7ad-49f5-464f-b511-51b084562ecd/providers/Microsoft.Web/locations/eastus/managedApis/office365",
                    "connectionId": "/subscriptions/ecbef7ad-49f5-464f-b511-51b084562ecd/resourceGroups/ODL-DFC-2199368/providers/Microsoft.Web/connections/office365",
                    "connectionName": "office365",
                    "connectionProperties": {}
                }
            }
        }
    }
   }
   ```

   ![](./Images/ETS1229.png)

1. In the left pane, Go to **Identity (1)**
and make the status **On (2)** then click on **Save(3)**.

   ![](./Images/ETS1230.png)

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**.

1. Select **Workflows (3)**, then click on **Onboard New Employee - IT Department (4)**.

   ![](./Images/ETS1220.png)

1. Click **Tasks (1)** in the workflow menu and then click **+ Add task (2)**.

   ![](./Images/ETS1221.png)

1. Select **Run a Custom Task Extension (1)** from the task list and click on **Save (2)**.

   ![](./Images/ETS1222.png)

1. Click on **Run a Custom Task Extension (1)** , Select custom extension as **Onboarding email notification (2)** then click on **Save(3)**.

   ![](./Images/ETS1223.png)

1. Then Click **Save**.

   ![](./Images/ETS1224.png)

1. Now go to **Overview (1)** and select **Run on demand (2)**. 

   ![](./Images/ETS1225.png)

1. Click on **+ Select user (1)** then select the **ADUser3 (2)** and click on **Select (3)**.

   ![](./Images/ETS1226.png)

1. Click **Run workflow**.

   ![](./Images/ETS1227.png)

1. After a few minutes, check the **Workflow history (1)** to verify. click on **ADUser3 (2)** and check each task shows a **Completed (3)** status.

   ![](./Images/ETS1231.png)

1. Open a new tab and navigate to outlook using below link

   ```
   https://outlook.office.com/
   ```
1. If prompted to sign, provide the credentials below:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>

1. Check the email inbox (or Logic App run history) to confirm the notification was sent.

   ![](./Images/ETS1232.png)

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

1. Verify:
   - The user account is disabled
   - The user has been removed from groups
   - The user has been removed from teams
   - Licenses have been removed

1. Navigate to the user profile to confirm the status changes.

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

Conditional Access policies enforce security requirements based on conditions such as user identity, device compliance, location, and application being accessed. In this task, you will create a policy that requires MFA for cloud app access.

1. In the Entra Admin portal left pane, expand **ID Protection (1)** and select **Risk-based Conditional Access  (2)** then click on **+ New policy (3)**.

   ![](./Images/ETS1411.png)

1. Configure the Conditional Access Policy with the following details:

      - Name: **Require MFA for IT Department - Cloud Apps** **(1)**
      - Click on **0 users or agents (Preview) selected** **(2)** under Users or agents (Preview) option.
      - A new window will slide in, click on **Select users and Groups** **(3)** and then select the check box saying **Users and groups** **(4)**
      - Now a Select window will open, here select **IT-Department** and then click on **Select** **(5)** button.
   
         ![](./Images/ETS1412.png)
   
      - Click on **No target resources selected** **(1)** under Target resources option.
      - Click on **All resources (formally All cloud apps)** **(2)**

         ![](./Images/ETS1413.png)

      - Click on **0 conditions selected** **(1)** under Conditions option.
      - Then select **Device platforms** **(2)**
      - Now in the Device platforms blade, toggle the *Configure* switch to **Yes** **(3)** and make sure that all the checkboxes below are selected.
      - Then click on **Done** **(4)**

         ![](./Images/ETS1414.png)

      - Then select **Location** **(1)**
      - Now in the Location blade, toggle the *Configure* switch to **Yes** **(2)** and select **Any network or location **(3)**.

         ![](./Images/ETS1415.png)
  
      - Then select **Client Apps** **(1)**
      - Now in the Client Apps blade, toggle the *Configure* switch to **Yes** **(2)** and make sure that all the checkboxes below are selected.
      - Then click on **Done** **(3)**.

         ![](./Images/ETS1416.png)

      - Click on **0 controls selected (1)** of `Grant` Section under the Access Control option.
      - in the **Grant** pane, click on **Grant access**
      - Select the check Box saying **Require multi-factor authentication** **(2)** 
      - Then click on **Select** **(3)**

         ![](./Images/ETS1417.png)
   
      - Toggle the **Enable Policy** switch to **On (1)** and click on **Create (2)**.

      ![](./Images/ETS1418.png)

1. Open an **Incognito browser**, paste the provided link, and log in using below **credentials**.

      ```
      portal.azure.com
      ```

   - Username: Paste the username  **<inject key="ADUser1 Email"></inject>** then click on **Next**.

      ![](./Images/ETS1419.png)   

   - Password:  Paste the password **<inject key="ADUser Password"></inject> (1)** and click on **Sign in (2)**.

      ![](./Images/ETS1420.png)   

1. If you see the **Action Required** pop up, click on **Ask later.**

   >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

      ![](./Images/ETS1421.png)   

     >**Note:** Follow the below steps, if MFA prompted:

     - Click **Next** in **Lets keep your account secure**.
     - On **Install Microsoft Authenticator**, click **Next**.

       ![](./Images/ETS1422.png)    

     - Click **Next**.

       ![](./Images/ETS1423.png)     

     - In **android**, go to the play store and Search for **Microsoft Authenticator** and Tap on **Install**.      

       ![](./Images/ETS1424.png)    

        >Note: For iOS, open the App Store and repeat the steps.

        >Note: Skip if already installed.       

     - Open the app and tap on **Scan a QR code**.

     - Scan the QR code visible on the screen **(1)** and click on **Next (2)**.

       ![](./Images/ETS1425.png)   

     - Enter the digit displayed on the Screen in the Authenticator app on your mobile and tap on **Yes**.

       ![](./Images/ETS1426.png)       

     - Once the notification is approved, click on **Next**.

     - Click on **Done**.

       ![](./Images/ETS1427.png)        

1. If prompted to stay signed in, you can click **"No"**.

1. Tap on **Finish** in the Mobile Device.

   > NOTE: While logging in again, enter the digits displayed on the screen in the **Authenticator app** and click on Yes.

1. Return to the Microsoft Entra admin center and navigate to **Entra ID** > **Monitoring & health** > **Sign-in logs**.

1. Verify the recent sign-ins

   ![](./Images/ETS1428.png)

## Task 5: Monitoring and Auditing with Entra ID (Optional/Read Only)

1. In the Microsoft Entra admin center, navigate to **Entra ID** > **Monitoring & health** > **Audit logs**.

1. Review the Audit logs activity across your tenant.

   ![](./Images/ETS1511.png)

1. Navigate to **Sign-in logs**.

1. Review the sign-in activity across your tenant.

   ![](./Images/ETS1512.png)

1. Apply filters to find interesting 

1. Navigate to **Home** to see the tenant summary dashboard.

   ![](./Images/ETS1513.png)

## Summary

In this exercise, you implemented key Identity Governance capabilities in Microsoft Entra ID, including dynamic group-based access, automated onboarding and offboarding workflows, and automatic license and application provisioning. You also integrated a custom Logic App extension, performed access reviews, and enforced security through Conditional Access with MFA. Additionally, you monitored activity using audit and sign-in logs. Together, these features establish a strong foundation for a modern Identity Governance and Administration (IGA) solution.

Now, click on Next from the lower right corner to move on to the next page.

   ![](./Images/Nextpage.png)
