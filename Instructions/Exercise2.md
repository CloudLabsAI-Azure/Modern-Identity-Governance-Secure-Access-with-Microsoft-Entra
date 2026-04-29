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

1. In the left navigation pane, expand **Global Secure Access** then select **Dashboard** and click on **Activate**.

   ![](./Images/ETS2101.png)

1. Review the licensing requirements shown on the page:
   - **Microsoft Entra Internet Access**: Requires Microsoft Entra Internet Access add-on or Microsoft Entra Suite
   - **Microsoft Entra Private Access**: Requires Microsoft Entra Private Access add-on or Microsoft Entra Suite

5. Once activated, you should see the Global Secure Access dashboard with traffic insights and configuration options.


1. In the Microsoft Entra admin center, navigate to **Billing** > **Licenses** to verify you have the appropriate licenses assigned.

2. Navigate to **Global Secure Access** > **Settings** to review tenant-level settings.

3. Ensure the following prerequisites are met for your test device:
   - Windows 10 version 21H2 or later, or Windows 11
   - Device is Microsoft Entra joined (not just registered)
   - Device has outbound TCP connectivity to internet on port 443

### Step 3: Install and Configure the Global Secure Access Client

1. In the Microsoft Entra admin center, navigate to **Global Secure Access** > **Connect** > **Client download**.

2. Download the **Global Secure Access client** installer appropriate for your endpoint OS.

   ![Client download page](Images/ex2-task1-client-download.png)

3. On your **test Windows device** (Microsoft Entra joined), run the downloaded installer as Administrator:
   
   a. Double-click the installer file
   
   b. Accept the license agreement
   
   c. Click **Install**
   
   d. Wait for the installation to complete
   
   e. Click **Finish**

4. After installation, the Global Secure Access client icon will appear in the **System Tray** (notification area).

5. Click the system tray icon to verify the client status:
   - The client should show **Connected** status
   - You should see the Microsoft Entra tenant information

6. If prompted, sign in with your Microsoft Entra credentials.

   > **Verification:** The client should show a green connected status. If it shows disconnected, verify that the device is Entra joined and that you have the required licenses.

---

## Task 2: Configure Entra Private Access

**Estimated Duration: 20 minutes**

### Overview

Entra Private Access provides Zero Trust Network Access (ZTNA) to private resources without requiring a traditional VPN. You will set up a Private Access connector and configure access to an internal RDP server.

### Step 1: Deploy the Private Access Connector

1. In the Microsoft Entra admin center, navigate to **Global Secure Access** > **Connect** > **Connectors**.

2. Click **+ New connector group** to create a connector group:
   - **Name**: `Lab-Private-Access-Connectors`
   - **Region**: Select the region closest to your on-premises resources

3. Click **Save**.

4. In the connector group, click **Download connector service** to download the Private Access connector installer.

   ![Download connector](Images/ex2-task2-connector-download.png)

5. **On the server that will host the connector** (on-premises server or Azure VM):

   a. Transfer the connector installer to the server
   
   b. Run the installer as Administrator
   
   c. During installation, you will be prompted to authenticate to your Microsoft Entra tenant. Sign in with your Global Administrator account.
   
   d. Select the connector group **Lab-Private-Access-Connectors** when prompted.
   
   e. Complete the installation.

6. Back in the Microsoft Entra admin center, verify the connector appears under **Global Secure Access** > **Connect** > **Connectors** with a **Status** of **Active**.

   > **Note:** The connector must be installed on a Windows Server that has outbound HTTPS (port 443) connectivity to Microsoft services. It does not require inbound firewall rules.

   > **Lab Note:** If you are working in a lab environment with a pre-configured Azure VM, the connector may already be installed. Verify the status in the Connectors page.

### Step 2: Define Application Segments for Private Resources

1. Navigate to **Global Secure Access** > **Applications** > **Enterprise applications** (or **Private Access applications**).

2. Click **+ New application**.

3. On the **Create application** page:
   - **Name**: `Lab-RDP-Server`
   - **Connector group**: Select `Lab-Private-Access-Connectors`

4. Click **Add application segment** to define the private resource:

   - **Destination type**: IP address range
   - **IP address**: Enter the private IP address of your RDP server (e.g., `10.0.1.10`)
   - **Ports**: `3389`
   - **Protocol**: TCP

   ![Application segment configuration](Images/ex2-task2-app-segment.png)

5. Click **Apply** to add the segment.

6. Click **Save** to create the Private Access application.

7. In the application settings, navigate to **Users and groups** and assign access:
   - Click **+ Add user/group**
   - Select the test user or group (e.g., **Dynamic-Department-IT**)
   - Click **Assign**

### Step 3: Configure Quick Access Profile

1. Navigate to **Global Secure Access** > **Applications** > **Quick Access**.

2. Click **View Quick Access configuration** or **Configure Quick Access**.

3. Add the private IP range of your internal network:

   Click **Add Quick Access application segment** and configure:
   - **Destination type**: IP address range
   - **IP address**: `10.0.0.0`
   - **CIDR prefix**: `/16` (adjust to match your environment)
   - **Ports**: `3389, 445, 443` (RDP, SMB, HTTPS)
   - **Protocol**: TCP

4. Click **Apply** and then **Save**.

   > **Note:** Quick Access provides broader access to your private network ranges, whereas individual Private Access applications provide more granular control with per-app policies.

### Step 4: Test RDP Connectivity Through Global Secure Access

1. On your **Microsoft Entra joined Windows device** (with the Global Secure Access client installed):

2. Verify the client is showing **Connected** in the system tray.

3. Open **Remote Desktop Connection** (mstsc.exe).

4. In the **Computer** field, enter the **private IP address** of the RDP server (e.g., `10.0.1.10`).

5. Click **Connect**.

6. Enter the credentials for the RDP server when prompted.

7. Verify that the RDP session establishes successfully — this traffic is being routed through the Global Secure Access Private Access tunnel without a traditional VPN.

   > **Expected Result:** RDP should connect successfully using only the private IP address, even without a VPN, because the Global Secure Access client is tunneling the traffic to the Private Access connector on the corporate network.

---

## Task 3: Apply Conditional Access Policies to Private Access

**Estimated Duration: 10 minutes**

### Overview

You can apply Conditional Access policies specifically to Private Access traffic, requiring device compliance and MFA before users can connect to private resources.

### Step 1: Create a Conditional Access Policy for Private Access

1. In the Microsoft Entra admin center, navigate to **Protection** > **Conditional Access**.

2. Click **+ New policy**.

3. Set the **Name**: `Require Compliant Device for Private Access`

### Step 2: Configure Policy Assignments

#### Users
1. Under **Assignments**, click **Users**.
2. Select **Select users and groups** > **Users and groups**.
3. Add the group that has access to Private Access resources (e.g., **Dynamic-Department-IT**).
4. Click **Select**.

#### Cloud Apps or Actions
1. Click **Cloud apps or actions**.
2. Select **Cloud apps** > **Select apps**.
3. Search for and select **Microsoft Entra Private Access**.

   > **Note:** "Microsoft Entra Private Access" appears as a cloud app that represents all private resource access through the GSA tunnel.

4. Click **Select**.

#### Conditions
1. Click **Conditions** > **Device platforms**.
2. Set **Configure** to **Yes**.
3. Under **Include**, select **Any device**.
4. Click **Done**.

### Step 3: Configure Access Controls

1. Under **Access controls**, click **Grant**.

2. Select **Grant access** and check the following:
   - **Require multi-factor authentication**
   - **Require device to be marked as compliant**

3. Select **Require all the selected controls**.

4. Click **Select**.

   ![Private Access CA policy](Images/ex2-task3-ca-private-access.png)

### Step 4: Enable the Policy and Validate

1. Set **Enable policy** to **Report-only** first for testing.

2. Click **Create**.

3. To validate, attempt an RDP connection from the test device:

   a. Ensure the Global Secure Access client is connected.
   
   b. Open Remote Desktop Connection and connect to the private IP address.
   
   c. Observe if MFA or compliance prompts appear.

4. Review sign-in logs to verify the policy evaluation:
   - Navigate to **Identity** > **Monitoring & health** > **Sign-in logs**
   - Find the sign-in event for the Private Access application
   - Check the **Conditional Access** tab for policy results

5. Once validated, change the policy to **On** and save.

---

## Task 4: Configure Entra Internet Access

**Estimated Duration: 10 minutes**

### Overview

Microsoft Entra Internet Access provides a Secure Web Gateway (SWG) that filters internet-bound traffic, enforces web content filtering policies, and optimizes Microsoft 365 traffic.

### Step 1: Enable the Internet Access Traffic Profile

1. In the Microsoft Entra admin center, navigate to **Global Secure Access** > **Connect** > **Traffic forwarding profiles**.

2. You will see three traffic profiles:
   - **Microsoft traffic** (for Microsoft 365 optimization)
   - **Private access** (for private resources)
   - **Internet access** (for general internet traffic)

3. Toggle the **Internet access profile** to **Enabled**.

   ![Traffic forwarding profiles](Images/ex2-task4-traffic-profiles.png)

4. Review the traffic that will be forwarded — you can customize which traffic types are included.

5. Click **Save**.

### Step 2: Configure Web Content Filtering Policies

1. Navigate to **Global Secure Access** > **Secure** > **Web content filtering policies** (or **Internet Access** > **Policies**).

2. Click **+ New policy**.

3. On the **Create a web content filtering policy** page:
   - **Policy name**: `Block High-Risk Web Categories`
   - **Description**: `Block access to malicious and high-risk web content categories`

4. Under **Rules**, click **+ Add rule**:

   **Rule 1 – Block Malware:**
   - **Rule name**: `Block Malware Sites`
   - **Action**: Block
   - **Destination type**: Web category
   - **Categories**: Select:
     - Malware
     - Phishing
     - Botnets
     - Command and Control

   **Rule 2 – Block Gambling:**
   - **Rule name**: `Block Gambling Sites`
   - **Action**: Block
   - **Destination type**: Web category
   - **Categories**: Select:
     - Gambling

   **Rule 3 – Block Illegal Content:**
   - **Rule name**: `Block Illegal Content`
   - **Action**: Block
   - **Destination type**: Web category
   - **Categories**: Select:
     - Illegal content
     - Child exploitation

5. Click **Next: Assignments** and assign the policy to a group or all users.

6. Click **Next: Review + create** and then **Create**.

### Step 3: Create a Security Profile

1. Navigate to **Global Secure Access** > **Secure** > **Security profiles**.

2. Click **+ New security profile**.

3. Configure the security profile:
   - **Name**: `Standard Security Profile`
   - **Description**: `Standard security controls for all users`
   - **State**: Enabled

4. Under **Baseline policy**, select:
   - **Microsoft 365 traffic**: Enable (optimizes M365 traffic routing)

5. Under **Linked policies**, click **Link a policy** and add:
   - The **Block High-Risk Web Categories** web content filtering policy created above

6. Click **Next: Assignments**.

7. Assign the profile to all users or a specific group.

8. Click **Create**.

   ![Security profile configuration](Images/ex2-task4-security-profile.png)

### Step 4: Configure Microsoft 365 Traffic Optimization

1. Navigate to **Global Secure Access** > **Connect** > **Traffic forwarding profiles**.

2. Click on the **Microsoft traffic** profile.

3. Review the Microsoft 365 endpoints that are being routed through the GSA client:
   - Exchange Online
   - SharePoint Online
   - Microsoft Teams

4. Verify that **Microsoft 365 traffic** toggle is **Enabled**.

5. On the test device, verify the Global Secure Access client shows traffic being tunneled.

---

## Task 5: Validate Access and Review Logs

**Estimated Duration: 10 minutes**

### Step 1: Test End-User Access

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

2. Document the results of each test.

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

---

## Summary

In this exercise, you have:

- ✅ Enabled and configured Microsoft Entra Global Secure Access in your tenant
- ✅ Installed the Global Secure Access client on a Windows endpoint
- ✅ Deployed a Private Access connector for on-premises resource access
- ✅ Configured Private Access application segments for RDP access
- ✅ Applied Conditional Access policies to Private Access traffic
- ✅ Enabled Internet Access with web content filtering
- ✅ Created security profiles with linked content filtering policies
- ✅ Validated access and reviewed traffic logs and security insights

## Additional Resources

- [Microsoft Entra Global Secure Access Documentation](https://learn.microsoft.com/en-us/entra/global-secure-access/)
- [Microsoft Entra Private Access Overview](https://learn.microsoft.com/en-us/entra/global-secure-access/concept-private-access)
- [Microsoft Entra Internet Access Overview](https://learn.microsoft.com/en-us/entra/global-secure-access/concept-internet-access)
- [Configure Web Content Filtering](https://learn.microsoft.com/en-us/entra/global-secure-access/how-to-configure-web-content-filtering)
- [Global Secure Access Client for Windows](https://learn.microsoft.com/en-us/entra/global-secure-access/how-to-install-windows-client)
- [Deploy and Configure Private Access Connector](https://learn.microsoft.com/en-us/entra/global-secure-access/how-to-configure-connectors)
