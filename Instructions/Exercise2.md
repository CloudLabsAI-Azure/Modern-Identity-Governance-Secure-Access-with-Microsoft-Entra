# Exercise 2: Microsoft Entra Global Secure Access (Internet & Private Access)

**Estimated Duration: 60 minutes**

## Overview

Microsoft Entra Global Secure Access is Microsoft's Security Service Edge (SSE) solution that combines identity-aware network security with Zero Trust network access. It provides two core capabilities:

- **Microsoft Entra Private Access**: Replaces traditional VPN by providing Zero Trust access to private/on-premises resources
- **Microsoft Entra Internet Access**: Provides Secure Web Gateway (SWG) capabilities for internet-bound traffic

## Prerequisites

- Microsoft Entra ID P1 or P2 license
- **Microsoft Entra Internet Access** and **Microsoft Entra Private Access** licenses (included in Microsoft Entra Suite)
- Global Administrator or Security Administrator role
- A Windows 10/11 device that is either **Microsoft Entra joined** or **Microsoft Entra hybrid joined**
- For Private Access: An on-premises server or Azure VM to act as the Private Access connector host
- Internet connectivity from the connector server to Microsoft Entra

## Task 1: Enable Microsoft Entra Global Secure Access

1. In the left navigation pane, expand **Global Secure Access (1)** then select **Dashboard (2)** and click on **Activate (3)**.

   ![](./Images/ETS2101.png)

1. Review the licensing requirements shown on the page:
   - **Microsoft Entra Internet Access**: Requires Microsoft Entra Internet Access add-on or Microsoft Entra Suite
   - **Microsoft Entra Private Access**: Requires Microsoft Entra Private Access add-on or Microsoft Entra Suite

1. Once activated, you will get a notification Tenant activation is completed successfully.

   ![](./Images/ETS2102.png)

1. Navigate to **Global Secure Access** > **Settings** to review tenant-level settings.

1. Ensure the following prerequisites are met for your test device:
   - Windows 10 version 21H2 or later, or Windows 11
   - Device is Microsoft Entra joined (not just registered)
   - Device has outbound TCP connectivity to internet on port 443

1. In the Microsoft Entra admin center, navigate to **Connect (1)** and click on **Client download (2)**.

2. Click on **Download client (3)** to download the **Global Secure Access client** installer for your endpoint OS.

   ![](./Images/ETS2103.png)

## Task 2: Configure Entra Private Access

Entra Private Access provides Zero Trust Network Access (ZTNA) to private resources without requiring a traditional VPN. You will set up a Private Access connector and configure access to an internal RDP server.

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

1. Select **** user/ group and click on **select**

   ![](./Images/ETS2119.png)

1. Click on **Assign** to add the group

   ![](./Images/ETS2120.png)

1. Now, in your desktop **Start** menu, search for **RDP (1)** and select **Remote Connection Desktop (2)**.

   ![](./Images/ETS2121.png)

1. Provide Computer Name as <inject key="DNSname"></inject> then click on **Connect**.

   ![](./Images/ETS2122.png)

1. Now click on **More choices (1)** then select **Use different account (2)**. Provide the below credentials and click on **Ok (5)**.

      - Username :  .\ <inject key="username"></inject> **(3)**
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

      - Username :   <inject key="username"></inject> **(1)**
      - Password :  <inject key="Password"></inject> **(2)**

      ![](./Images/ETS2127.png)

1. Now Click on **Yes** to connect to the **Client VM**

      ![](./Images/ETS2124.png)

1. Verify that the RDP session establishes successfully — this traffic is being routed through the Global Secure Access Private Access tunnel without a traditional VPN.

   > **Note:** RDP should connect successfully using only the private IP address, even without a VPN, because the Global Secure Access client is tunneling the traffic to the Private Access connector on the corporate network.

1. Once connected close the Remote desktop connection

## Task 3: Apply Conditional Access Policies to Private Access

You can apply Conditional Access policies specifically to Private Access traffic, requiring device compliance and MFA before users can connect to private resources.

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

3. To validate, attempt an RDP connection from the test device:

   a. Ensure the Global Secure Access client is connected.
   
   b. Open Remote Desktop Connection and connect to the private IP address.
   
   c. Observe if MFA or compliance prompts appear.

4. Review sign-in logs to verify the policy evaluation:
   - Navigate to **Identity** > **Monitoring & health** > **Sign-in logs**
   - Find the sign-in event for the Private Access application
   - Check the **Conditional Access** tab for policy results

5. Once validated, change the policy to **On** and save.

## Task 4: Configure Entra Internet Access

Microsoft Entra Internet Access provides a Secure Web Gateway (SWG) that filters internet-bound traffic, enforces web content filtering policies, and optimizes Microsoft 365 traffic.

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

   - **Rule name**: `Blockweb`
   - **Destination type**: URL
   - **Url destination**: netflix.com

   **Rule 2 – Block Gambling:**
   - **Rule name**: `Block Gambling Sites`
   - **Action**: Block
   - **Destination type**: Web category
   - **Categories**: Select:
     - Gambling

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

1. Now open Edge browser and paste the below link and verify that the website 

   ```
   netflix.com
   ```

   ![](./Images/ETS2329.png)
   >**Note**: These changes may take upto 1 hour to reflect. you can check this at the end of the lab. please proceed with next exercise.

1. Now open other websites as your wish and check the website.

## Task 5: Validate Access and Review Logs

1. On the **Microsoft Entra joined test device** with the Global Secure Access client connected:

   **Test Private Access:**
   - Open Remote Desktop Connection
   - Connect to the private IP of the RDP server
   - Verify successful connection (traffic goes through Private Access)

   **Test Internet Access:**
   - Open a web browser and navigate to a legitimate website (e.g., `https://www.microsoft.com`)
   - Verify the page loads successfully

   **Test Web Content Filtering:**
   - Attempt to access a blocked category website (test with a known safe test URL or use a URL categorized under a blocked category)
   - Verify the access is blocked and a block page is displayed


### Step 2: Review Traffic Logs in Global Secure Access Dashboard

1. In the Microsoft Entra admin center, navigate to **Global Secure Access** > **Monitor** > **Traffic logs**.

2. Review the traffic log dashboard:
   - **Source IP**: The IP of the client device
   - **Destination**: The accessed resource
   - **Action**: Allow or Block
   - **Traffic type**: Private Access or Internet Access
   - **User**: The authenticated user

   ![Global Secure Access traffic logs](Images/ex2-task5-traffic-logs.png)

3. Filter logs by:
   - **Date**: Last 1 hour
   - **Action**: Block (to see filtered traffic)
   - **Traffic profile**: Internet Access

4. Click on individual log entries to see detailed information including:
   - Policy that triggered the action
   - Web category (for internet traffic)
   - Application segment (for private access traffic)

### Step 3: Analyze Security Insights and Policy Violations

1. Navigate to **Global Secure Access** > **Monitor** > **Dashboard**.

2. Review the overview metrics:
   - Total traffic volume
   - Number of blocked requests
   - Top users by traffic
   - Top blocked categories

3. Look for any anomalies or unexpected patterns:
   - Unusual traffic volumes from specific users
   - Access attempts to blocked categories
   - Failed private access connection attempts

4. Navigate to **Global Secure Access** > **Monitor** > **Security alerts** (if available).

5. Review any alerts triggered by:
   - Policy violations
   - Suspicious traffic patterns
   - Connector connectivity issues

### Step 4: Verify Traffic Routing and Policy Application

1. Navigate to **Global Secure Access** > **Monitor** > **Remote network health** (if applicable).

2. Check the health status of:
   - Private Access connectors (should show as Healthy/Active)
   - Traffic forwarding profiles (should show as Enabled)

3. Navigate to **Global Secure Access** > **Applications** > **Enterprise applications** (Private Access).

4. Select **Lab-RDP-Server** and review:
   - **Usage**: Recent access attempts and their status
   - **User sessions**: Active and recent sessions

5. Confirm that:
   - Traffic from the test device is being routed through Global Secure Access
   - Policies are being correctly applied
   - No traffic is bypassing the GSA tunnel

## Summary

In this exercise, you have:

-  Enabled and configured Microsoft Entra Global Secure Access in your tenant
-  Installed the Global Secure Access client on a Windows endpoint
-  Deployed a Private Access connector for on-premises resource access
-  Configured Private Access application segments for RDP access
-  Applied Conditional Access policies to Private Access traffic
-  Enabled Internet Access with web content filtering
-  Created security profiles with linked content filtering policies
- Validated access and reviewed traffic logs and security insights
