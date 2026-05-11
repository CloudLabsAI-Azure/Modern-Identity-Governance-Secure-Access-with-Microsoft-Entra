# Exercise 1: Microsoft Entra ID Governance (IGA)

**Estimated Duration: 90 minutes**

## Overview

In this exercise, you will explore how to implement core Identity Governance capabilities using Microsoft Entra ID. You will learn how to manage users and groups dynamically, automate onboarding and offboarding processes using lifecycle workflows, and enhance security through access reviews and conditional access policies. By the end of this exercise, you will gain hands-on experience in building a modern identity governance solution.

## Prerequisites

The following are prerequisites to complete this exercise which are already configured in this lab environment:

- An active Microsoft Entra ID tenant
- A user account with Global Administrator or Identity Governance Administrator role
- Microsoft Entra ID P2 / Governance /Entra suite license enabled  
- 2–3 test users available for validation

## Lab Objectives

In this lab, you will complete the following exercise:

- Task 1: Manage Users and Groups
- Task 2: Entra ID Lifecycle Management
- Task 3: Setting Up Access Reviews
- Task 4: Implementing Conditional Access Policies
- Task 5: Monitoring and Auditing with Entra ID (Optional)

## Task 1: Manage Users and Groups

In this task, you will create dynamic groups based on user attributes and define rules to automatically manage group membership and validate that memberships are updated dynamically based on the configured conditions.

1. Open a new tab in the browser and navigate to **Microsoft Entra admin center** using below link.

   ```
   https://entra.microsoft.com
   ```

1. If prompted, provide the credentials below:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
    ![Enter Your Username](./Images/GS6.png)
 
3. Next, provide your temporary password **(1)** and select **Sign in (2)**.
 
   - **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject>
 
      ![Enter Your Password](./Images/GS6-1.png)

   >**Note:** If pop-up appears asking about Give feedback to Microsoft, close it. 

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

1. In the **Dynamic membership rules** page, click on **Edit (1)** under **Rule syntax**, put the below mentioned rule **(2)** then click on **Ok** and **Save (3)**.
   ```
   (user.jobTitle -contains "Manager")
   ```
   ![](./Images/ETS1111.png)

1. Click **Create**.

   ![](./Images/ETS1112.png)

1. In the left pane, go to **Users (1)**, click on **AD User1 (2)** to open the profile.

   ![](./Images/E1T1S14.png)

1. Click **Edit properties**.

   ![](./Images/E1T1S15.png)

1. Select **Job Information (1)** and set the following attributes and then click on **Save (4)**

   - **Job title**: `IT Manager` **(2)**
   - **Department**: `IT` **(3)**

      ![](./Images/E1T1S16.png)

1. Navigate back to **Groups (1)**, select **All groups (2)** and Click on **IT-Department (3)**.

   ![](./Images/ETS1116.png)

1. Select **Members**, verify that **AD User1** is now added to the group dynamically. Click **Cancel**.

   ![](./Images/E1T1S18.png)

   >**Note:** If user is not appeared then wait a few minutes and then click Refresh.

1. From the list, select **Managers** group. Select **Members**, verify that **AD User1** is added as a member in the group.

   ![](./Images/E1T1S19.png)

1. You can also validate the users by using **Validate rules**. Click on **Dynamic membership rules (1)** under **Manage**, select **Validate rules (2)** and then **+ Add users (3)**.

   ![](./Images/E1T1S20.png)

1. From the list, select **AD User2 (1)** and click on **Select (2)**. Once the user is selected, the user **(3)** will appear under the **Validate Rules** section. To verify the status, click on **View details (4)**.

   ![](./Images/E1T1S21.png)

   ![](./Images/E1T1S21-1.png)

1. For AD User2, the job title attribute does not contain "Manager", therefore the user is not added to the group dynamically.

   ![](./Images/E1T1S21-2.png)

## Task 2: Entra ID Lifecycle Management

Microsoft Entra Lifecycle Workflows automate identity processes across the employee lifecycle from onboarding (joiner) to role changes (mover) to offboarding (leaver). This replaces manual HR-driven processes and reduces the risk of orphaned accounts.

### Task 2.1: Configure Onboarding Workflow

In this task, you will set up a lifecycle workflow to automate the onboarding process for new employees. You will configure triggers and automate tasks such as sending welcome emails, assigning licenses, and adding users to groups.

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**. Then click on **+ Create workflow (3)**

   ![](./Images/E1T2-1S1.png)

1. You will see the Lifecycle Workflows dashboard with template categories: **Joiner**, **Mover**, and **Leaver**. Locate **Onboard new hire employee** and click on **Select** under it.

   ![](./Images/E1T2-1S2.png)

1. On the **Basics** page, provide Name as **Onboard New Employee - IT Department (1)** and then click **Next: Configure (2)**.

   ![](./Images/E1T2-1S3.png)

1. On the **Configure** page, Make the rule department value as **IT (1)** then click **Next: Review tasks (2)**.

   ![](./Images/E1T2-1S4.png)

1. On the **Review tasks** page, you will see pre-configured tasks. Click **+ Add task** to add additional tasks.

   ![](./Images/E1T2-1S5.png)

1. Select **Generate TAP and send email (1)** and **Add license to User (2)** then click on **Add (3)**.

   ![](./Images/E1T2-1S6.png)

1. Click on **Add license to User (1)** then click on **0 license selected (2)**

   ![](./Images/E1T2-1S7.png)

1. Select **Office_365_E1_(no_Teams) (1)** then click on **Select (2)**.

   ![](./Images/E1T2-1S8.png)

1. Click on **Save**.

   ![](./Images/E1T2-1S9.png)

1. Now click on **Add user to group (1)** then click on **0 group selected (2)**

   ![](./Images/E1T2-1S10.png)

1. Select **New joiners (1)** group and the click on **Select (2)**

   ![](./Images/E1T2-1S11.png)

1. Click on **Save (1)**.

   ![](./Images/E1T2-1S12.png)

1.  Review the order of tasks. Drag tasks to reorder them if needed. Recommended order is mentioned below **(1)**. Click **Review + create (2)**.

      - Enable user account
      - Send welcome email
      - Add user to groups
      - Generate TAP and send Email
      - Assign license to User

         ![](./Images/E1T2-1S13.png)

1. Review the workflow configuration summary and click **Create**.

   ![](./Images/E1T2-1S14.png)

### Task 2.2: Implement Custom Extensions

In this task, you will create a custom task extension using a Logic App. This extension will be integrated into the lifecycle workflow to perform additional actions, such as sending notifications, and you will test its execution.

1. Select **Lifecycle workflows (1)** under **ID Governance** in the left navigation pane then **Lifecycle workflows (1)**. Under **Manage** select **Custom extensions (2)**, then click on **+ Add a custom extension (3)**.

   ![](./Images/E1T2-2S1.png)

1. On the Basics tab, provide the below details then click on **Next Task behavior (3)**

      - **Name**: Onboarding email notification **(1)**
      - **Description**: Send the onboarding email notification to New joinee **(2)**.

         ![](./Images/E1T2-2S2.png)

1. On the Task behavior tab, leave it as default and click on **Next Details**

   ![](./Images/E1T2-2S3.png)

1. On the Details page, Provide the below details and click on **Create logoc app(4)**

      - **Subscription**: Leave it as default **(1)**
      - **Resource group**: **ODL-Entra-<inject key="Deployment ID" enableCopy="false"></inject>-01 (2)**
      - **Logic app name**: `custom-task-workflow` **(3)**

         ![](./Images/E1T2-2S4.png)

1. Once the deployment is successful **(1)**, click on **Next: Review + create**. 

   ![](./Images/E1T2-2S5.png)

1. Then, select **Create** to initiate the deployment.

   ![](./Images/E1T2-2S6.png)

1. After the deployment is successful, click on **custom-task-workflow** under Logic app.

   ![](./Images/E1T2-2S7.png)

1. Copy the below code. Click on **Logic app code view (1)** then select all the existing code with **Ctrl + A** and then click **Ctrl + V** to paste the code and then click on **Save (3)** to save the code

   ```
   {
   "definition": {
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "actions": {
      "HTTP": {
        "inputs": {
          "authentication": {
            "audience": "https://graph.microsoft.com",
            "type": "ManagedServiceIdentity"
          },
          "body": {
            "data": {
              "operationStatus": "Completed"
            },
            "source": "sample",
            "type": "lifecycleEvent"
          },
          "method": "POST",
          "uri": "https://graph.microsoft.com/beta@{triggerBody()?['data']?['callbackUriPath']}"
        },
        "runAfter": {},
        "type": "Http"
      }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
      "manual": {
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
        },
        "kind": "Http",
        "type": "Request"
      }
    }
   },
   "parameters": {}
   }
   ```

   ![](./Images/E1T2-2S8.png)

1. In the left pane, go to **Logic app designer (1)** then click on **+ (2)** and select **Add an action(3)**.

   ![](./Images/E1T2-2S9.png)

1. Search for **send an email (1)** and select **Send an email (V2) (2)** under `Office 365 outlook`.

   ![](./Images/ETS12112.png)

1. Click on **Sign in**. A new window will pop-up, select the **ODL_user_<inject key="Deployment ID" enableCopy="false"></inject>**.

   ![](./Images/ETS12113.png)
   
   ![](./Images/ETS12114.png)

1. Provide the below information in the `Parameters` tab.

   - **To**: ```@triggerBody()?['data']?['subject']?['email']``` **(1)** 

   - **Subject**: ```Welcome Onboarding``` **(2)**

   - **Body** : Copy and paste the below content **(3)**
      ```
      <p class="editor-paragraph">
      Hello Dear @{triggerBody()?['data']?['subject']?['displayName']},<br><br>

      Welcome to your first day at work.<br><br>

      Attached, you will find the information for your first login.<br><br>

      Use the following username to log in:<br>
      @{triggerBody()?['data']?['subject']?['email']}<br><br>

      Set it up here:
      <a href="https://portal.office.com" class="editor-link">portal.office.com</a><br><br>

      If you have any questions, please contact your manager:<br>
      @{triggerBody()?['data']?['subject']?['manager']?['displayName']}<br>

      <a href="mailto:@{triggerBody()?['data']?['subject']?['manager']?['email']}">
      @{triggerBody()?['data']?['subject']?['manager']?['email']}
      </a><br><br>

      You can reach our helpdesk at 00011112222.<br><br>

      Welcome On-Board!
      </p>
      ```
    
      ![](./Images/E1T2-2S12.png)

1. Scroll down and click on the dropdown of the Advanced parameters **(1)** then select **CC (2)**.

   ![](./Images/E1T2-2S13.png)

   ![](./Images/E1T2-2S13-1.png)

1. In the cc box provide the following text **(1)** to add Manager ID and click on **Save (2)** save it. 

   ```
   @triggerBody()?['data']?['subject']?['manager']?['email']
   ```

   ![](./Images/E1T2-2S14.png)

1. Check the flow of the logic app.

   ![](./Images/E1T2-2S15new.png)

   >**Note**: The manual step is the trigger that starts the Logic App when the lifecycle workflow sends a request. The HTTP action processes the request and marks the task as completed, and the Send an email (V2) step sends a notification to the user and manager.

1. Navigate to the **Identity (1)** option under **Settings** and switch the toggle to **On (2)** then click on **Save(3)**.

   ![](./Images/E1T2-2S16new.png)

1. On the Enable system assigned managed identity pop-up, select **Yes** to enable to managed identity.

   ![](./Images/E1T2-2S17.png)

1. Navigate to **Lifecycle workflows (1)** in the left navigation pane under ID Governance, select **Workflows (2)** and then **Onboard New Employee - IT Department (3)**.

   ![](./Images/E1T2-2S18.png)

1. Select **Tasks (1)** under Manage and then click on **+ Add task (2)**.

   ![](./Images/E1T2-2S19.png)

1. From the list, select **Run a Custom Task Extension (1)** and click on **Add (2)**.

   ![](./Images/E1T2-2S20.png)

1. Click on **Run a Custom Task Extension (1)**, select custom extension as **Onboarding email notification (2)** then click on **Save(3)**.

   ![](./Images/E1T2-2S21.png)

1. Then, click **Save**.

   ![](./Images/E1T2-2S22new.png)

1. Before running the Lifecysle workflow, you need to update few properties of the user. Navigate to **Users (1)** under Entra ID and click on **AD User3 (2)**.

   ![](./Images/E1T2-2S22-1.png)

1. Click **Edit properties**.

   ![](./Images/E1T2-2S23.png)

1. Select **All** then scroll down and set the following attributes and then click on **Save (4)**

   - **Employee Hire date**: Select today's date **(1)**
   - **Manager**: Click on **+Add manager** and select <inject key="AzureAdUserEmail"></inject>**(2)**
   - **Email**: Go to Environment tab and copy **User 03 UPN (3)**

      ![](./Images/E1T2-2S25.png)

1. Navigate to **Lifecycle workflows (1)** under ID Goverenance, select **Workflows (2)**, then click on **Onboard New Employee - IT Department (3)**.

   ![](./Images/E1T2-2S18.png)

1. Select **Run on demand** under Overview. 

   ![](./Images/E1T2-2S27.png)

1. Click on **+ Select users (1)** then, select the **AD User3 (2)** and click on **Select (3)**.

   ![](./Images/E1T2-2S28.png)

1. Click **Run workflow**.

   ![](./Images/E1T2-2S29.png)

1. Click on **Refresh** to get the summary. Then, click on **AD User3**.

   ![](./Images/E1T2-2S30.png)

1.  Notice that each task shows a status as **Completed**.

      ![](./Images/E1T2-2S31.png)

1. Navigate to **Users (1)** under Entra ID and click on **AD User3 (2)**.

   ![](./Images/E1T2-2S22-1.png)

1. Now click on **Group** to check the user has been assigned to the group **Newjoiner** and navigate back to users and check that **license** that has been assigned to the user.

   ![](./Images/E1T2-2S33.png)
   
   ![](./Images/E1T2-2S33-1.png)

1. Open a new tab and navigate to outlook using below link.

   ```
   https://outlook.office.com/
   ```
1. If prompted to sign, provide the credentials below:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>

   - **Password:** <inject key="AzureAdUserPassword"></inject>

1. After login, check that the email has recieved in Inbox.

   ![](./Images/ETS1232.png)

### Task 2.3: Configure Offboarding Workflow

In this task, you will configure a lifecycle workflow to handle employee offboarding. You will automate tasks such as removing licenses, disabling accounts, and cleaning up group memberships, and validate the complete offboarding process.

1. In the Microsoft Entra admin center, expand **ID Governance (1)** in the left navigation pane and select **Lifecycle workflows (2)**. Then click on **+ Create workflow (3)**

   ![](./Images/E1T2-1S1.png)

1. Scroll down and select the **Offboard an employee** template under the **Leaver** category.

   ![](./Images/E1T2-3S2new.png)

1. On the **Basics** page, leave it as default and click **Next: Configure scope >**.

   ![](./Images/E1T2-3S3.png)

1. On the **Configure** page, make the rule department value as **IT (1)** then click **Next: Review tasks > (2)**.

   ![](./Images/E1T2-3S4.png)

1. On the **Review tasks** page, you will see pre-configured tasks. Click **+ Add task (1)** to add additional tasks.

   ![](./Images/E1T2-3S5.png)

1. Select **Remove all licenses for user  (2)** then click on **Add (3)**.

   ![](./Images/E1T2-3S6.png)   

1. Review the order of tasks. Drag tasks to reorder them if needed. Recommended order is mentioned below **(1)**. Click on **Review + Create (2)**

   1. Disable user account
   2. Remove user from all groups
   3. Remove user from all Teams
   4. Remove all licenses for user

      ![](./Images/E1T2-3S7.png)  

1. Click **Create**.

   ![](./Images/E1T2-3S8.png)

1. Now select **Offboard an employee** and click on **Run on demand**.

   ![](./Images/E1T2-3S9.png)

1. Click on **+ Select users (1)** then select **AD User3 (2)** and click **Select (3)**.

   ![](./Images/E1T2-3S10.png)

1. Now click on **Run workflow**.

   ![](./Images/E1T2-3S11.png)

1. To get the summary click on **Refresh** then, select **AD User3**.

   ![](./Images/E1T2-3S12.png)

1. On the pop-up ntice that each task shows a status as **Completed**. Click on cancel. 

   ![](./Images/E1T2-3S13.png)

1. Navigate to **Users (1)** under Entra ID and click on **AD User3 (2)**.

   ![](./Images/E1T2-2S22-1.png)

1. Verify the **Groups Memberships (1)** and **License (2)** of the **ADUser3** has been revoked and account is **disabled (3)**.

   ![](./Images/E1T2-3S15.png)

## Task 3: Setting Up Access Reviews

In this task, you will create and configure access reviews to regularly validate user access to groups or applications. You will define reviewers, schedules, and perform approval or denial actions while reviewing audit logs.

1. In the Microsoft Entra admin center, navigate to **ID Governance** and select **Access reviews** then click on **+ Access reviews**

   ![](./Images/E1T3S1.png)

1. Under **Review access to a resource type**, click on **Select**.

   ![](./Images/E1T3S2.png)

1. On the **Review type** page, provide the below details:
      - Select what to review :**Teams + Groups (1)**
      - Review scope : **Select Teams + groups (2)**
      - Group : click on **+ Select groups (3)**

         ![](./Images/E1T3S3.png)

      -  Under Groups, select **IT-Department (4)** and then click on **Select (5)**.

         ![](./Images/E1T3S3-1.png)

1. Select **All users (1)** under scope and click on **Next Reviews (2)**.

   ![](./Images/E1T3S4.png)

1. In the select reviewers drop down, click on **Selected user(s) or group(s)**

   ![](./Images/ETS1266.png)

1. Then click on **+ select reviwers (1)** and select **ODL_User <inject key="DeploymentID"></inject> (2)** then click on **select (3)**.

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

1. Now select **ADUser 1 (1)** and click on **Approve (2)**. 
   ![](./Images/ETS5111.png)

1. Provide a justification in the reason as **Valid access (1)** field and **Sumbit (2)**.

   ![](./Images/ETS5112.png)

1. Navigate back to Entra admin portal and select **Access review** under ID Governance. 

   ![](./Images/ETS1314.png)

1. Now click on **Results** to see the log.

   ![](./Images/ETS5113.png)

## Task 4: Implementing Conditional Access Policies

In this task, you will create a Conditional Access policy to enforce multi-factor authentication (MFA). You will configure conditions such as users, apps, and locations, and validate the policy using sign-in logs.

1. In the Entra Admin portal left pane, expand **ID Protection (1)** and select **Risk-based Conditional Access  (2)** then click on **+ New policy (3)**.

   ![](./Images/E1T4S1.png)

1. Configure the Conditional Access Policy with the following details:

      - Name: **Require MFA for IT Department - Cloud Apps** **(1)**
      - Click on **0 users or agents (Preview) selected** **(2)** under Users or agents (Preview) option.

         ![](./Images/E1T4S2-1.png)

      - A new window will slide in, click on **Select users and Groups** **(1)** and then select the check box for **Users and groups** **(2)**.

         ![](./Images/E1T4S2-2.png)

      - Under Select users and groups, check the box for **IT-Department (1)** and then click on **Select (2)** button.
   
         ![](./Images/E1T4S2-3.png)
   
      - Click on **No target resources selected** **(1)** under Target resources option.

         ![](./Images/E1T4S2-4.png)

      - Click on **All resources (formally All cloud apps)** **(2)**

         ![](./Images/E1T4S2-5.png)

      - Click on **0 conditions selected** **(1)** under Conditions option. Then select **Not configured (2)** under Device platforms.
         
         ![](./Images/E1T4S2-6.png)

      - Now in the Device platforms blade, toggle the *Configure* switch to **Yes** **(1)** and make sure that **Any device (2)** option is selected. Then click on **Done** **(3)**

         ![](./Images/E1T4S2-7.png)

      - Then select **Not configured (1)** under Locations.

         ![](./Images/E1T4S2-8.png)

      - Now in the Location blade, toggle the *Configure* switch to **Yes** **(1)** and select **Any network or location (2)**.

         ![](./Images/E1T4S2-9.png)
  
      - Select **Not configured (1)** under Client apps.

         ![](./Images/E1T4S2-10.png)

      - Now in the Client Apps blade, toggle the *Configure* switch to **Yes** **(1)** and make sure that all the checkboxes below are selected **(2)**. Then click on **Done** **(3)**.

         ![](./Images/E1T4S2-11.png)

      - Click on **0 controls selected (1)** of `Grant` Section under the Access Control option.

         ![](./Images/E1T4S2-12.png)

      - Under **Grant** pane, click on **Grant access (1)** then, select the check Box for **Require multi-factor authentication** **(2)**. Click on **Select** **(3)**. 

         ![](./Images/E1T4S2-13.png)
   
      - Toggle the **Enable Policy** switch to **On (1)** and click on **Create (2)**.

        ![](./Images/E1T4S2-14.png)

1. Open a **New InPrivate window**, paste the provided link, and log in using below **credentials**.

      ```
      portal.azure.com
      ```

   - **Username:** Paste the username **<inject key="User 01 UPN"></inject>**

      ![](./Images/ETS1419.png)   

   - **Password:** Paste the password **<inject key="User's Password"></inject> (1)** and click on **Sign in (2)**

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

       ![](./Images/QR.png)   

     - Enter the digit displayed on the Screen in the Authenticator app on your mobile and tap on **Yes**.

       ![](./Images/ETS1426.png)       

     - Once the notification is approved, click on **Next**.

     - Click on **Done**.

       ![](./Images/Done.png)        

1. If prompted to stay signed in, you can click **"No"**.

1. Tap on **Finish** in the Mobile Device.

   >NOTE: While logging in again, enter the digits displayed on the screen in the **Authenticator app** and click on Yes.

1. Return to the Microsoft Entra admin center and navigate to **Entra ID** > **Monitoring & health (1)** > **Sign-in logs (2)**. Verify the recent User sign-ins **(3)**.

   ![](./Images/E1T4S8.png)

## Task 5: Monitoring and Auditing with Entra ID (Optional/Read Only)

In this task, you will explore audit logs, sign-in logs, and reporting dashboards to monitor identity activities. This helps in gaining visibility into governance operations and improving security posture.

1. In the Microsoft Entra admin center, navigate to **Entra ID** > **Monitoring & health** > **Audit logs**. Review the Audit logs activity across the tenant.

   ![](./Images/E1T5S1.png)

1. Navigate to **Sign-in logs**. Review the sign-in activity across the tenant.

   ![](./Images/E1T5S2.png)

1. You can also add filters to find more details. 

1. Navigate to **Home** to see the tenant summary dashboard.

   ![](./Images/E1T5S4.png)

## Summary

In this exercise, you implemented key Identity Governance capabilities in Microsoft Entra ID, including dynamic group-based access, automated onboarding and offboarding workflows, and automatic license provisioning. You also integrated a custom Logic App extension, performed access reviews, and enforced security through Conditional Access with MFA. Additionally, you monitored activity using audit and sign-in logs. Together, these features establish a strong foundation for a modern Identity Governance and Administration (IGA) solution.

Now, click on Next from the lower right corner to move on to the next page.

   ![](./Images/Nextpage.png)
