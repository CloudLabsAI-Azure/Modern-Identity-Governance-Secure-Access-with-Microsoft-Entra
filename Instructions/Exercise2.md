# Exercise 2: Microsoft Entra Global Secure Access (Internet & Private Access)

**Estimated Duration: 60 minutes**

## Overview
In this lab, you will configure Microsoft Entra Global Secure Access to securely manage access to private and internet resources. You will enable the service, install the client, and set up Private Access to connect to internal resources without a VPN. You will apply Conditional Access policies to enforce security controls and configure Internet Access for web filtering. Finally, you will validate connectivity and review logs to ensure policies are working as expected.

## Prerequisites

The following are prerequisites to complete this exercise which are already configured in this lab environment:

- Microsoft Entra ID P1 or P2 license
- **Microsoft Entra Internet Access** and **Microsoft Entra Private Access** licenses (included in Microsoft Entra Suite)
- Global Administrator or Security Administrator role
- A Windows 10/11 device that is either **Microsoft Entra joined** or **Microsoft Entra hybrid joined**
- For Private Access: An on-premises server or Azure VM to act as the Private Access connector host
- Internet connectivity from the connector server to Microsoft Entra

## Lab Objectives

In this lab, you will complete the following exercise:

- Task 1: Enable Microsoft Entra Global Secure Access
- Task 2: Configure Entra Private Access
- Task 3: Apply Conditional Access Policies to Private Access
- Task 4: Configure Entra Internet Access
- Task 5: Validate Access and Review Logs

## Task 1: Enable Microsoft Entra Global Secure Access

Microsoft Entra Global Secure Access is Microsoft's Security Service Edge (SSE) solution that combines identity-aware network security with Zero Trust network access. It provides two core capabilities:

- **Microsoft Entra Private Access**: Replaces traditional VPN by providing Zero Trust access to private/on-premises resources
- **Microsoft Entra Internet Access**: Provides Secure Web Gateway (SWG) capabilities for internet-bound traffic.

In this task, you will activate Microsoft Entra Global Secure Access in your tenant and prepare the environment for secure access to both private and internet resources. You will also download the Global Secure Access client, which will be used in later tasks to connect endpoint devices securely

1. In the left navigation pane, expand **Global Secure Access (1)** then select **Dashboard (2)** and click on **Activate (3)**.

   ![](./Images/E2T1S1new.png)

1. Once activated, you will get a notification Tenant activation is completed successfully.

   ![](./Images/E2T1S2.png)

1. Now, in your desktop **Start** menu, search for **RDP (1)** and select **Remote Connection Desktop (2)**.

   ![](./Images/E3T3S3.png)

1. Provide Computer Name as <inject key="ClientVM DNS Name"></inject> **(1)** then click on **Connect (2)**.

   ![](./Images/E3T3S4.png)

1. Now click on **More choices (1)** then select **Use a different account (2)**. Provide the below credentials and click on **OK (5)**.

      - Username :  .\ <inject key="ClientVM Admin Username"></inject> **(3)**
      - Password :  <inject key="ClientVM Admin Password"></inject> **(4)**

         ![](./Images/E2T1S19.png)

1. Now Click on **Yes** to connect to the **Client VM**

      ![](./Images/ETS2124.png)

   >Note: A window will appeat to Choose privacy setting for your device, click on **Next** and then **Accept**.
      ![](./Images/ETS4111.png)

1. On the Start menu, search for **settings (1)** and click on **Setting (2)**.

      ![](./Images/ETS4112.png)

1. Select **Accounts (1)** and click on **Access work or school (2)**.

      ![](./Images/ETS4113.png)

1. Click on **Connect**.

      ![](./Images/ETS4114.png)

1. Select **Join this device to Microsoft Entra ID**.

      ![](./Images/ETS4115.png)

1. It will prompt to sign in. Provide the below credentials:

   - **Username:** Paste the username  <inject key="AzureAdUserEmail"></inject> in the **Sign in (1)** field. Click **Next (2)** to continue.

      ![](./Images/GS6.png)   

   - **Password:**  Paste the password <inject key="AzureAdUserPassword"></inject> **(1)** and click **Sign in (2)**.

      ![](./Images/GS6-1.png)  

1. Click on **Join** and then click on **Done**.

      ![](./Images/ETS4116.png)
      ![](./Images/ETS4117.png)

1. On the Start menu, search for **cmd (1)** and click on **Command Prompt (2)**.

      ![](./Images/ETS4118.png)

1. Paste the below command **(1)** and check the status of the device as **AzureADjoined (2)**.

   ```
   dsregcmd /status
   ```

      ![](./Images/ETS4119.png)

## Task 2: Configure Entra Private Access

In this task, you will configure Microsoft Entra Private Access by enabling traffic forwarding and deploying a Private Access connector. You will create an application segment and assign users to securely access internal resources. Finally, you will validate connectivity by accessing a private resource (RDP) through the Global Secure Access client without using a traditional VPN.

1. On the Lab VM, in the Microsoft Entra admin center, navigate to **Traffic forwarding(1)**
and enable the **Private access profile (2)**. Click on **Ok** on the pop-up window.

   ![](./Images/E2T2S1.png)

   ![](./Images/E2T2S1-1.png)

1. Once it is enabled click on **View (3)** to add the user and group assignments.

   ![](./Images/E2T2S2.png)

1. Enable **Assign to all users (1)**, on pop-up, click on **OK (2)**, once enabled select **Done (3)**.

   ![](./Images/E2T2S3-1.png)

   ![](./Images/E2T2S3-2.png)

   ![](./Images/E2T2S3-3.png)

1. Now navigate to **Connectors and sensors (1)** and click **Download connector service (2)** to download the Private Access connector installer.

   ![](./Images/E2T2S4.png)

1. Click on **Accept terms & Download**.

   ![](./Images/E2T2S5.png)

1. Once installer file is downloaded click on **Open file**

   ![](./Images/ETS2108.png)

1. Check the box **(1)** and click on **Install (2)**.

   ![](./Images/ETS2109.png)

1. While installing, it will prompt to sign in. Provide the below credentials:

   - **Username:** Paste the username  <inject key="AzureAdUserEmail"></inject> in the **Sign in** field. Click **Next** to continue.

      ![](./Images/GS6.png)   

   - **Password:**  Paste the password <inject key="AzureAdUserPassword"></inject> **(1)** and click **Sign in (2)**.

      ![](./Images/GS6-1.png)  

1. Once installation is completed, click on **Close**

   ![](./Images/ETS2113.png) 

1. Navigate to **Microsoft Entra admin portal** and check the **Connectors** status as **Active**.

   ![](./Images/E2T2S10.png) 

   >**Note**: If the connector is not appeared, refresh the browser and check again.

1. Navigate to **Applications (1)** then select **Quick Access (2)**. Provide the Name as **Quick actions (3)** and click on **+ Add Quick Access application segment (4)**.

   ![](./Images/E2T2S11.png) 

1. Provide the below details and click **Apply (5)** to add the segment.

   - **Destination type**: IP address **(1)**
   - **IP address**: `10.0.0.1/24` **(2)**
   - **Ports**: `3389`  **(3)**
   - **Protocol**: TCP **(4)**

      ![](./Images/E2T2S12.png) 

1. Click **Save** to create the Quick Access application segment.

   ![](./Images/E2T2S13.png)

1. Navigate to **Quick Acces (1)** under Applications, select **Users and groups (2)** and click **+ Add user/group (3)**.

   ![](./Images/E2T2S14.png)

   >**Note**: If you could not find users and groups option, click on **Quick access** again in the left pane to get the option.

1. Click on **None selected (1)** then select **IT-Department (2)** and click on **select (3)**.

   ![](./Images/E2T2S15.png)

1. Click on **Assign** to add the group.

   ![](./Images/E2T2S16.png)

1. On the Client VM Desktop, double click on **Global Secure Access Client** installer file.
   
   ![](./Images/ETS2125.png) 

1. On the Global Secure Access Client, click on **Sign in**.

   ![](./Images/E1T2S22.png)

1. If prompted sign in with the below credentials.

   - **Username:** Paste the username  **<inject key="User 01 UPN"></inject>** then click on **Next**.

      ![](./Images/ETS1419.png)   

   - **Password:**  Paste the password **<inject key="User's Password"></inject> (1)** and click on **Sign in (2)**.

      ![](./Images/E1T2S23-2.png)

1. On **Sign in to all apps and websites on this device** pop up, select **Yes**.

   ![](./Images/E1T2S24.png)

1. On **Allow your Oraganization to manage your device** pop up. Select **Yes**.

   ![](./Images/E1T2S25.png)

1. On the **Account added to this device** select **Done**.

   ![](./Images/ETS2132.png)

1. Wait for few seconds and check the status as **Connected**.

   ![](./Images/E1T2S27.png)

1. Navigate to Azure portal, search for **Virtual machines (1)** and select it **(2)**.   

   ![](./Images/E1T2S28.png)

1. Select rdpvm-<inject key="Deployment ID" enableCopy="false"></inject> from the list. Copy **Private IP address** and paste it in the notepad.

   ![](./Images/E1T2S29.png)

   ![](./Images/E1T2S29-1.png)

1. Navigate to ClientVM, in the **Start** menu, search for **RDP (1)** and select **Remote Connection Desktop (2)**.

   ![](./Images/E3T3S3.png)

1. In the **Computer** field, enter the **private IP address (1)** of the RDP server copied in Step 29. Click **Connect (2)**.

   ![](./Images/ETS2122.png)

1. Enter the credentials for the RDP server when prompted and click on **Ok (3)**.

      - Username : <inject key="adminUsername"></inject> **(1)**
      - Password :  <inject key="admingPassword"></inject> **(2)**

      ![](./Images/E3T3S6.png)

1. Now Click on **Yes** to connect to the **RDPserver**

      ![](./Images/ETS2124.png)

1. Verify that the RDP session establishes successfully — this traffic is being routed through the Global Secure Access Private Access tunnel without a traditional VPN.

   ![](./Images/E2T2S34.png)

   > **Note:** RDP should connect successfully using only the private IP address, even without a VPN, because the Global Secure Access client is tunneling the traffic to the Private Access connector on the corporate network.

1. Once connected close the Remote desktop connection of RDP server.

## Task 3: Apply Conditional Access Policies to Private Access

In this task, you will apply Conditional Access policies specifically to Private Access traffic, requiring device compliance and MFA before users can connect to private resources.

1. In the Lab VM, navigate back to Microsoft Entra admin center, expand **Global Secure Accesss (1)**, then **Application (2)**, under it click on **Quick Access (3)**, select **Conditional Access (4)**. Click on **+ New policy (5)**.

   ![](./Images/E2T3S1.png)

1. Configure the Conditional Access Policy with the following details:

   - **Name:** Require Compliant Device for Private Access **(1)**
   - **Assignments**: Click on **0 users or agents (Preview) selected** **(2)** under Users or agents (Preview) option.

      ![](./Images/E2T3S2-1.png)

   - A new window will slide in, click on **Select users and Groups** **(1)** then select the check box saying **Users and groups** **(2)**

   - Select window will open, select **IT-Department (3)** and then click on **Select**  button.
   
      ![](./Images/E2T3S2-2.png)
   
   - Click on **1 resources included** **(1)** under Target resources option and verify that **Quick actions** **(2)** is already selected.

      ![](./Images/E2T3S2-3.png)

   - Click on **0 conditions selected** **(1)** under Conditions option. Then select **Not configured (2)** under Device platforms.

      ![](./Images/E2T3S2-4.png)

   - Now in the Device platforms blade, toggle the *Configure* switch to **Yes (1)** and make sure that all **Any device (2)** option is selected. Then click on **Done** **(3)**

      ![](./Images/E2T3S2-5.png)

   - Click on **0 controls selected (1)** of `Grant` section under the Access Control option.

      ![](./Images/E2T3S2-6.png)

   - In the **Grant** pane, click on **Grant access**. Select the check box for **Require multi-factor authentication (1)** and **Require device to be marked as compliant (2)**. Then click on **Select** **(3)**. 

      ![](./Images/E2T3S2-7.png)
   
   - Toggle the **Enable Policy** switch to **On (1)** and click on **Create (2)**.

      ![](./Images/E2T3S2-8.png)
         
      >**Note**: Applying Conditional access policy will take few minutes to reflect.

1. Now again in the **Start** menu, search for **RDP (1)** and select **Remote Desktop Connection (2)**.

   ![](./Images/E3T3S3.png)

1. In the **Computer** field, enter the **private IP address (1)** of the RDP server. Click **Connect (2)**.

   ![](./Images/E3T3S4.png)

1. It will prompt for MFA. Enter the digit displayed on the Screen in the Authenticator app on your mobile and tap on **Yes**.

1. Once it is authenticated, enter the credentials for the RDP server when prompted and click on **OK (3)**.

      - Username : <inject key="adminUsername"></inject> **(1)**
      - Password :  <inject key="adminPassword"></inject> **(2)**

         ![](./Images/E3T3S6.png)

1. Now Click on **Yes** to connect to the **RDP server**.

      ![](./Images/ETS2124.png)

1. Once connected close the Remote desktop connection of RDP server.

## Task 4: Configure Entra Internet Access

In this task, you will configure Microsoft Entra Internet Access by enabling traffic forwarding profiles and applying web content filtering policies. You will create a security profile and enforce it using Conditional Access to control and restrict access to specific websites. Finally, you will validate the policy by testing internet access from a client device.

1. In the Microsoft Entra admin center, navigate to **Global Secure Access** expand **Connect (1)** and click on **Traffic forwarding (2)** then toggle the **Internet access profile (3)** to **Enabled**.

   ![](./Images/E3T4S1.png)

1. Click on **Enable Microsoft and Internet Access profiles**

   ![](./Images/E3T4S2.png)

1. On the Internet access profile, click on **View** to add the users.

   ![](./Images/E3T4S3.png)

1. Enable **Assign to all users (1)**. On the pop-up click on **OK (2)**. Once enable, click on **Done (3)**.

   ![](./Images/E3T4S4-1.png)

   ![](./Images/E3T4S4-2.png)

   ![](./Images/E3T4S4-3.png)

1. Repeat the same steps for **Microsoft traffic profile** to add the users.

   ![](./Images/E3T4S5.png)

1. Now expand **Secure (1)**, select **Web content filtering policies (2)** then click on **+ Create policy (3)**.

   ![](./Images/E3T4S6.png)

1. On the **Create a web content filtering policy** page, provide name as **BlockAccess (1)** and click on **Next (2)**.

   ![](./Images/E3T4S7.png)

4. Under **Policy rules**, click **+ Add rule (1)**:

   - **Rule name**: `Blockcategory` **(2)**
   - **Destination type**: Webcategory **(3)**
   - **Search**: Search **Streaming Media And Downloads (4)** and select **(5)** it.
   - Click on **Add (6)**, after added select **Next (7)** .

       ![](./Images/E3T4S8.png)

1. On the **Review** tab, click on **Create policy**.

   ![](./Images/E3T4S9.png)

1. Navigate to **Security profiles (1)** under Secure and click on **+ Create profile (2)**.

   ![](./Images/E3T4S10.png)

1. Provide the profile name as **Webprofile (1)** and click on **Next (2)**.

   ![](./Images/E3T4S11.png)

1. Click on **+ Link profile (1)** and select **Existing web filtering policy (2)**.

   ![](./Images/E3T4S12.png)

1. On the link a policy tab, select **BlockAccess (1)** and click on **Add (2)**.

   ![](./Images/E2T4S13.png)

1. Once it is added, click on **Next**.

   ![](./Images/E2T4S14.png)

1. On the **Review** tab, click on **Create a profile**

   ![](./Images/E2T4S15.png)

1. Navigate to **ID Protection (1)** and select **Risk-based Conditional Access  (2)** then click on **+ New policy (3)**.

   ![](./Images/E2T4S16.png)

1. Configure the Conditional Access Policy with the following details:

      - Name: **Streamingwebsiteblocked** **(1)**
      - Click on **0 users or agents (Preview) selected** **(2)** under Users or agents (Preview) option.

         ![](./Images/E2T4S17-1.png)

      - A new window will slide in, click on **Select users and groups** **(1)** and then select the check box for **Users and groups** **(2)**. Then, select window will open, click on **IT-Department (3)** and then **Select** button.

         ![](./Images/E2T4S17-2.png)

      - Click on **No target resources selected** **(1)** under Target resources option.

         ![](./Images/E2T4S17-3.png)

      - Click on **All internet resources with Global secure access** **(2)**

         ![](./Images/E2T4S17-4.png)

      - Click on **0 controls selected (1)** of `Session` Section under the Access Control option.

         ![](./Images/E2T4S17-5.png)

      - In the **Session** pane, select the check Box for **use Global Secure Access Security profile (1)** then, select **Webprofile (2)** and click on **Select (3)**.

         ![](./Images/E2T4S17-6.png)
   
      - Toggle the **Enable Policy** switch to **On (1)** and click on **Create (2)**.

         ![](./Images/E2T4S17-7.png)

1. Open the ClientVM, verify the Global Secure Access client is connected from system tray.

   ![](./Images/ETS2328.png)

1. Now open Edge browser and paste the below link and verify that the website is not reachable.

   ```
   netflix.com
   ```

   ![](./Images/ETS2329.png)
   >**Note**: These changes may take upto 1 hour to reflect. you can check this at the end of the lab. please proceed with next exercise.

1. Now open other websites as your wish and check the status.

## Task 5: Validate Access and Review Logs

In this task, you will collect and analyze network traffic using the Global Secure Access client and apply filters to validate Private and Internet access behavior. You will also review traffic logs and insights in the Entra portal to confirm policy enforcement and usage patterns.

1. On the Client VM, right click on Global secure access client on the systems tray and select **Advanced logs**.

   ![](./Images/ETS2511.png)

1. Select **Traffic (1)** and click on **Start collecting logs (2)**.

   ![](./Images/ETS2512.png)

1. Open browser and search for **Netflix** or **primevideo** and verify that the pages are not acceesiable.

   ![](./Images/ETS2513.png)

1. Now open **Remote desktop connection** and login into RDP server using Private ipaddress and provising below credentials: 

      - Username : <inject key="adminUsername"></inject> **(1)**
      - Password :  <inject key="adminPassword"></inject> **(2)**

      ![](./Images/ETS2127.png)

1. Once connected close the Remote desktop connection of RDP server.

1. Now open **Global secure access client- advanced diagnostics** and click on **Stop collecting** and check the logs.

   ![](./Images/ETS2514.png)

1. You can add filter as well. Click on **Add filter** and provide the below information and click on **Ok (4)**

   - **Filter**: Channel **(1)**
   - **Operator**: == **(2)**
   - **Vaule** : Private **(3)**

   ![](./Images/ETS2515.png)

1. You can verify from the logs that the Action is shown as Tunnel, which indicates that the RDP traffic is being routed through the Global Secure Access client tunnel using Private Access.

   ![](./Images/ETS2516.png)

1. Add the filter for **Internet access** and check the logs

   ![](./Images/ETS2517.png)

1. Now, in the lab VM, navigate to Microsoft Entra admin center and click on **Dashboard** under Global secure access and check the insights like users and devices, uasge profiling, top used cloud applications etc.

   ![](./Images/ETS2518.png)

1. Scroll down and check the web content filtering, top used deninations and top discovered application segment etc.

   ![](./Images/ETS2519.png)

1. Now expand Monitor and click on **Traffic log** to check the all the logs (Internet access, Private access, Micorsoft 365 access logs).

   ![](./Images/ETS2520.png)

1. you can also filter the logs. For example click on add filter and select the destination FQDN and provide value as **netflix.com** and apply. You can the actions as blocked.

   ![](./Images/ETS2521.png)

## Summary

In this exercise, you configured Microsoft Entra Global Secure Access and set up secure connectivity for both private and internet resources. You deployed the client and Private Access connector, enabled secure RDP access without a VPN, and applied Conditional Access policies for enhanced security. You also configured Internet Access with web filtering and security profiles. Finally, you validated access and reviewed logs to understand traffic flow and policy enforcement.

Now, click on Next from the lower right corner to move on to the next page.

   ![](./Images/Nextpage.png)
