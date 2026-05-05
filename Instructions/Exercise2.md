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

   ![](./Images/ETS2101.png)

1. Once activated, you will get a notification Tenant activation is completed successfully.

   ![](./Images/ETS2102.png)

1. In the Microsoft Entra admin center, navigate to **Connect (1)** and click on **Client download (2)**.

1. Click on **Download client (3)** to download the **Global Secure Access client** installer for your endpoint OS.

   ![](./Images/ETS2103.png)
   >**Note**: Global Secure Access client software will be installed in the next exercise on the endpoint device

## Task 2: Configure Entra Private Access

In this task, you will configure Microsoft Entra Private Access by enabling traffic forwarding and deploying a Private Access connector. You will create an application segment and assign users to securely access internal resources. Finally, you will validate connectivity by accessing a private resource (RDP) through the Global Secure Access client without using a traditional VPN.

1. In the Microsoft Entra admin center, navigate to **Traffic forwarding(1)**
and enable the **Private access profile (2)**. Once it is enabled click on **View (3)** to add the user and group assignments.

   ![](./Images/ETS2104.png)

1. Enable **Assign to all users (1)** and click on **Done (2)**.

   ![](./Images/ETS2105.png)

1. Now navigate to **Connectors and sensors (1)** and click **Download connector service (2)** to download the Private Access connector installer.

   ![](./Images/ETS2106.png)

1. Click on **Accept terms & Download**.

   ![](./Images/ETS2107.png)

1. Once installer file is downloaded click on **Open file**

   ![](./Images/ETS2108.png)

1. Check the box **(1)** and click on **Install (2)**.

   ![](./Images/ETS2109.png)

1. While installing, it will prompt to sign in. Provide the below credentials:

   - Username: Paste the username  **<inject key="ADUser1 Email"></inject>** then click on **Next**.

      ![](./Images/ETS2110.png)   

   - Password:  Paste the password **<inject key="ADUser Password"></inject> (1)** and click on **Sign in (2)**.

      ![](./Images/ETS2112.png)  

1. Once installation is completed, click on **Close**

   ![](./Images/ETS2113.png) 

1. Navigate to **Microsoft Entra admin portal** and check the **Connectors** status as active

   ![](./Images/ETS2114.png) 
   >**Note**: If the connector is not appeared, refresh the browser and check again

1. Navigate to **Applications (1)** then select **Quick actions (2)**.

1. Provide the Name as **Quick Access (3)** and click on **+ Add Quick Access application segment (4)**

   ![](./Images/ETS2115.png) 

1. Provide the below details and click **Apply (5)** to add the segment.

   - **Destination type**: IP address **(1)**
   - **IP address**:`10.0.0.1/24` **(2)**
   - **Ports**: `3389`  **(3)**
   - **Protocol**: TCP **(4)**

   ![](./Images/ETS2116.png) 

1. Click **Save** to create the Quick Access application segment.

   ![](./Images/ETS2117.png)

1. Navigate to **Users and groups (1)** and click **+ Add user/group (2)**.

   ![](./Images/ETS2118.png)
   >**Note**: If you could not find users and groups option, click on **Quick access** again in the left pane to get the option.

1. Click on **None selected (1)** then select **IT-Department (2)** and click on **select**

   ![](./Images/ETS2119.png)

1. Click on **Assign** to add the group

   ![](./Images/ETS2120.png)

1. Now, in your desktop **Start** menu, search for **RDP (1)** and select **Remote Connection Desktop (2)**.

   ![](./Images/ETS2121.png)

1. Provide Computer Name as <inject key="ClientDNSname"></inject> then click on **Connect**.

   ![](./Images/ETS2122.png)

1. Now click on **More choices (1)** then select **Use different account (2)**. Provide the below credentials and click on **Ok (5)**.

      - Username :  .\ <inject key="adminUsername"></inject> **(3)**
      - Password :  <inject key="Password"></inject> **(4)**

      ![](./Images/ETS2123.png)

1. Now Click on **Yes** to connect to the **Client VM**

      ![](./Images/ETS2124.png)

1. On the Client VM Desktop, double click on **Global Secure Access Client** installer file.
   
   ![](./Images/ETS2125.png) 

1. Accept the license agreement **(1)** and click on **Install **(1)**.
   
   ![](./Images/ETS2126.png) 

1. Once the installation is completed, click on **Close**.

   ![](./Images/EST2127.png)

1. Wait for a minute to open the Global Secure Access Client then click on **Sign in**.

   ![](./Images/ETS2129.png)

1. If prompted sign in with the below credentials.

   - Username: Paste the username  **<inject key="ADUser1 Email"></inject>** then click on **Next**.

      ![](./Images/ETS1419.png)   

   - Password:  Paste the password **<inject key="ADUser Password"></inject> (1)** and click on **Sign in (2)**.

      ![](./Images/ETS1420.png)

1. On **Sign in to all apps and websites on this device** pop up, select **Yes**.

   ![](./Images/ETS2130.png)

1. On **Allow your Oraganization to manage your device** pop up. Select **Yes**.

   ![](./Images/ETS2131.png)

1. On the **Account added to this device** select **Done**.

   ![](./Images/ETS2132.png)

1. Wait for few seconds and check the status as **Connected**

   ![](./Images/ETS2133.png)

1. Now in the **Start** menu, search for **RDP (1)** and select **Remote Connection Desktop (2)**.

   ![](./Images/ETS2121.png)

1. In the **Computer** field, enter the **private IP address** of the RDP server  `10.0.0.5`. Click **Connect**.

   ![](./Images/ETS2122.png)

1. Enter the credentials for the RDP server when prompted and click on **Ok (3)**.

      - Username : <inject key="username"></inject> **(1)**
      - Password :  <inject key="Password"></inject> **(2)**

      ![](./Images/ETS2127.png)

1. Now Click on **Yes** to connect to the **RDPserver**

      ![](./Images/ETS2124.png)

1. Verify that the RDP session establishes successfully — this traffic is being routed through the Global Secure Access Private Access tunnel without a traditional VPN.

   > **Note:** RDP should connect successfully using only the private IP address, even without a VPN, because the Global Secure Access client is tunneling the traffic to the Private Access connector on the corporate network.

1. Once connected close the Remote desktop connection of RDP server.

## Task 3: Apply Conditional Access Policies to Private Access

In this task, you will apply Conditional Access policies specifically to Private Access traffic, requiring device compliance and MFA before users can connect to private resources.

1. In the Lab VM, navigate back to Microsoft Entra admin center and select **Conditional access (1)** in the **Quick Access** page and click on **+ New policy (2)**.

   ![](./Images/ETS2211.png)

1. Configure the Conditional Access Policy with the following details:

   - Name: **Require Compliant Device for Private Access** **(1)**
   - **Assignments**:
     - Click on **0 users or agents (Preview) selected** **(2)** under Users or agents (Preview) option.
     - A new window will slide in, click on **Select users and Groups** **(3)** and then select the check box saying **Users and groups** **(4)**
     - Now a Select window will open, here select **IT-Department** and then click on **Select** **(5)** button.
   
         ![](./Images/ETS2212.png)
   
      - Click on **1 resources included** **(1)** under Target resources option and verify that **Quick Access** **(2)** is already selected

         ![](./Images/ETS2213.png)

      - Click on **0 conditions selected** **(1)** under Conditions option.
      - Then select **Device platforms** **(2)**
      - Now in the Device platforms blade, toggle the *Configure* switch to **Yes** **(3)** and make sure that all the checkboxes below are selected.
      - Then click on **Done** **(4)**

         ![](./Images/ETS2216.png)

      - Click on **0 controls selected (1)** of `Grant` Section under the Access Control option.
      - In the **Grant** pane, click on **Grant access**
      - Select the check boxs saying **Require multi-factor authentication** **(2)** and **Require device to be marked as compliant (3)**
      - Then click on **Select** **(4)**

         ![](./Images/ETS2214.png)
   
      - Toggle the **Enable Policy** switch to **On (1)** and click on **Create (2)**.

         ![](./Images/ETS2215.png)
         >**Note**: Applying Conditional access policy will take few minutes to reflect.

1. Now again in the **Start** menu, search for **RDP (1)** and select **Remote Connection Desktop (2)**.

   ![](./Images/ETS2121.png)

1. In the **Computer** field, enter the **private IP address** of the RDP server  `10.0.0.5`. Click **Connect**.

   ![](./Images/ETS2122.png)

1. It will prompt for MFA. Enter the digit displayed on the Screen in the Authenticator app on your mobile and tap on **Yes**.

   ![](./Images/ETS2187.png)

1. Once it is authenticated, enter the credentials for the RDP server when prompted and click on **Ok (3)**.

      - Username : <inject key="username"></inject> **(1)**
      - Password :  <inject key="Password"></inject> **(2)**

      ![](./Images/ETS2127.png)

1. Now Click on **Yes** to connect to the **RDPserver**

      ![](./Images/ETS2124.png)

1. Once connected close the Remote desktop connection of RDP server.

## Task 4: Configure Entra Internet Access

In this task, you will configure Microsoft Entra Internet Access by enabling traffic forwarding profiles and applying web content filtering policies. You will create a security profile and enforce it using Conditional Access to control and restrict access to specific websites. Finally, you will validate the policy by testing internet access from a client device.

1. In the Microsoft Entra admin center, navigate to **Global Secure Access** expand **Connect (1)** and click on **Traffic forwarding (2)** then toggle the **Internet access profile (3)** to **Enabled**.

   ![](./Images/ETS2311.png)

1. Click on **Enable Microsoft and Internet Access profiles**

   ![](./Images/ETS2312.png)

1. On the Internet access profile, click on **View** to add the users.

   ![](./Images/ETS2313.png)

1. Enable **Assign to all users (1)** and click on **Done (2)**.

   ![](./Images/ETS2105.png)

1. Repeat the same steps for **Microsoft traffic profile** to add the users.

1. Now expand **Secure (1)** select **Web content filtering policies (2)** and click **+ New policy (3)**.

   ![](./Images/ETS2314.png)

1. On the **Create a web content filtering policy** page, provide name as **BlockAccess (1)** and click on **Next (2)**.

   ![](./Images/ETS2315.png)

4. Under **Policy rules**, click **+ Add rule (1)**:

   - **Rule name**: `Blockcategory` **(2)**
   - **Destination type**: Webcategory **(3)**
   - **Url destination**: Search and select **Streaming media and download **(4)**

1. Click on **Add (5)** and click on **Next (6)** 

   ![](./Images/ETS2316.png)

1. On the **Review** tab, click on **Create policy**

   ![](./Images/ETS2317.png)

1. Navigate to **Security profiles (1)** and click on **+ Create profile (2)**.

   ![](./Images/ETS2318.png)

1. Provide the profile name as **Webprofile (1)** and click on **Next (2)**.

   ![](./Images/ETS2319.png)

1. Click on **+ Link profile (1)** and select **Existing web filtering policy (2)**.

   ![](./Images/ETS2320.png)

1. On the link a policy tab, select **BlockAccess (1)** and click on **Add (2)**.

   ![](./Images/ETS2321.png)

1. Once it is added, click on **Next**.

   ![](./Images/ETS2322.png)

1. On the **Review** tab, click on **Create a profile**

   ![](./Images/ETS2323.png)

1. Navigate to **ID Protection (1)** and select **Risk-based Conditional Access  (2)** then click on **+ New policy (3)**.

   ![](./Images/ETS2324.png)

1. Configure the Conditional Access Policy with the following details:

      - Name: **Streamingwebsiteblocked** **(1)**
      - Click on **0 users or agents (Preview) selected** **(2)** under Users or agents (Preview) option.
      - A new window will slide in, click on **Select users and Groups** **(3)** and then select the check box saying **Users and groups** **(4)**
      - Now a Select window will open, here select **IT-Department** and then click on **Select** **(5)** button.

         ![](./Images/ETS2325.png)

      - Click on **No target resources selected** **(1)** under Target resources option.
      - Click on **All internet resources with Global secure access** **(2)**

         ![](./Images/ETS2326.png)

      - Click on **0 controls selected (1)** of `Session` Section under the Access Control option.
      - in the **Session** pane, select the check Box saying **use Global Secure Access Security profile (2)** **(2)** and select **Webprofile (3)**
      - Then click on **Select** **(4)**

         ![](./Images/ETS2327.png)
   
      - Toggle the **Enable Policy** switch to **On (1)** and click on **Create (2)**.

         ![](./Images/ETS1418.png)

1. Open the ClientVM, verify the Global Secure Access client is connected from system tray.

   ![](./Images/ETS2328.png)

1. Now open Edge browser and paste the below link and verify that the website is not reachable.

   ```
   netflix.com
   ```

   ![](./Images/ETS2329.png)
   >**Note**: These changes may take upto 1 hour to reflect. you can check this at the end of the lab. please proceed with next exercise.

1. Now open other websites as your wish and check the website.

## Task 5: Validate Access and Review Logs

In this task, you will collect and analyze network traffic using the Global Secure Access client and apply filters to validate Private and Internet access behavior. You will also review traffic logs and insights in the Entra portal to confirm policy enforcement and usage patterns.

1. On the Client VM, right click on Global secure access client on the systems tray and select **Advanced logs**.

   ![](./Images/ETS2511.png)

1. Select **Traffic (1)** and click on **Start collecting logs (2)**.

   ![](./Images/ETS2512.png)

1. Open browser and search for **Netflix or primevideo** and verify that the pages are not acceesiable.

   ![](./Images/ETS2513.png)

1. Now open **Remote desktop connection** and login into RDP server `10.0.0.5` using below credentials

      - Username : <inject key="username"></inject> **(1)**
      - Password :  <inject key="Password"></inject> **(2)**

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
