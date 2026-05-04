# Exercise 3: Microsoft Entra Verified ID

**Estimated Duration: 60 minutes**

## Overview

In this exercise, you set up Verified ID, registered a DID, and verified your domain. You created and issued a credential and stored it in the Authenticator app. You then verified the credential through a sample application. Finally, you explored a help desk scenario to understand how Verified ID enables secure, passwordless identity verification.

**Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Issuer** | An organization that creates and issues verifiable credentials to users |
| **Holder** | The user who receives and stores credentials in their digital wallet (Microsoft Authenticator) |
| **Verifier** | An organization or application that requests and verifies credentials from the holder |
| **DID (Decentralized Identifier)** | A globally unique identifier anchored to a blockchain or DID registry |
| **Verifiable Credential (VC)** | A tamper-evident credential containing claims about the holder, signed by the issuer |

## Prerequisites

- Microsoft Entra ID P1 or P2 license (Verified ID is included)
- Global Administrator role
- A custom domain configured in your Microsoft Entra tenant.
- A smartphone with **Microsoft Authenticator** app installed

## Task 1: Configure Verified ID Issuer

In this task, you will set up Microsoft Entra Verified ID for your organization. You will register your DID, verify your domain, and create a verified credential that can be issued to users.

1. On the Microsoft Entra admin center left navigation pane, expand **Verified ID** and click **Overview (1)** then click on **Configure (2)**.

   ![](./Images/ETS3101.png)

1. Provide the below details and click on **Select keys (3)** for Key vault selection.

   - **Organization**: VerifiedID **(1)**
   - **Trusted domain**: <inject key="Trusteddomain"></inject> **(2)**
      ![](./Images/ETS3102.png)

1. Leave the Subscription as **default (1)**. From the Key vault dropdown, select **AKV-<inject key="DeploymentID"></inject> (2)** and click on **Select (3)**.

   ![](./Images/ETS3103.png)

1. Click on **Save**

   ![](./Images/ETS3104.png)

1. Now on the overview page, Click on **Register** under the Register decentralized ID.

   ![](./Images/ETS3105.png)

1. Click on **Download (1)** then select the file location as **Desktop (2)** and click on **Save (3)**

   ![](./Images/ETS3106.png)

1. Open a new tab, paste the provided link, and log in into Azure portal using below **credentials**.

   ```
   portal.azure.com
   ```

   - Username: Paste the username  **<inject key="AzureUserEmail"></inject>** then click on **Next**.
      ![](./Images/ETS3107.png)

   - Password:  Paste the password **<inject key="AzureUserEmail"></inject> (1)** and click on **Sign in (2)**.
      ![](./Images/ETS3108.png)

   >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

      ![](./Images/ETS1421.png)   


1. Search for **storage account (1)** and select  **Storage accounts (2)**.

   ![](./Images/ETS3109.png)

1. Select **verifiedid<inject key="DeploymentID"></inject>** storage account.

   ![](./Images/ETS3110.png)

1. Go to **Containers (1)** under Data storage and select **$web (2)**.

   ![](./Images/ETS3111.png)

1. Click on **+ Add directory (1)** then provide the name as **.well-known (2)** and click on **Ok (3)**.
   
   ![](./Images/ETS3112.png)
   ![](./Images/ETS3113.png)

1. Click on **Upload (1)** and select **Browse for the files (2)**.

   ![](./Images/ETS3114.png)

1. Click on **Desktop (1)** and select **did.json (2)** then click on **Open (3)**.

   ![](./Images/ETS3115.png)

1. Click on **Upload**.

   ![](./Images/ETS3116.png)

1. Now, naviagte back to Microsoft Entra admin center, and click on **Refresh registration status (1)**. If the registration is successful **(2)**, click on **Close (3)**.

   ![](./Images/ETS3117.png)

1. Now on the overview page, Click on **Verify** under Verify domain membership.

   ![](./Images/ETS3118.png)

1. Click on **Download (1)** then select the file location as **Desktop (2)** and click on **Save (3)**

   ![](./Images/ETS3119.png)

1. Navigate back to Azure portal and click on **Upload (1)** and select **Browse for the files (2)**.

   ![](./Images/ETS3120.png)

1. Click on **Desktop (1)** and select **did-configuration.json (2)** then click on **Open (3)**.

   ![](./Images/ETS3122.png)

1. Click on **Upload**.

   ![](./Images/ETS3123.png)

1. Now, naviagte back to Microsoft Entra admin center, and click on **Refresh registration status (1)**. If the registration is successful **(2)**, click on **Close (3)**.

   ![](./Images/ETS3124.png)

1. On the Overview page, now you can see the **Verified Domain**

   ![](./Images/ETS3125.png)
   >**Note**: if it is still showing **Domain may not be verified**, click on **Verify**,click on **Refresh registration status**. If the registration is successful, click on **Close** and check again.
   ![](./Images/ETS3126.png)

1. Now, click on **+ Add credential** to create a new credential type.

   ![](./Images/ETS3127.png)

1. Click **Verified credential** and then **Next**.

   ![](./Images/ETS3128.png)

1. Provide below Information and click on **Update (4)** and verify the display card styling:

   - **Logo URL**: https://avd233.blob.core.windows.net/avdtest/VerifiedID.jpg **(1)**
   - **Text color**: #FFFFFF **(2)**
   - **Background color** : #0068cd **(3)**.

   ![](./Images/ETS3129.png)

1. Scroll down and click on **Create**.

   ![](./Images/ETS3130.png)

## Task 2: Issue Verifiable Credentials

In this task, you will configure the credential issuance process and issue a verifiable credential to a test user who will receive it in their Microsoft Authenticator app.

1. Now on the **Verified employee** page, click on **Issue a credential**.

   ![](./Images/ETS3131.png)

1. Ensure **Allow all Microsoft Entra ID users within the tenant (1)** is selected and check the box **Issue credentials through My Account (2)**.

   ![](./Images/ETS3132.png)

1. Scroll down and click on **Save**.

   ![](./Images/ETS3133.png)

1. Now click on **Overview (1)** under Verified ID and click on **Try it now (2)** under Get the new credentials.

   ![](./Images/ETS3134.png)

1. It will be redirect to the **MyAccount** portal. Click on **Get my Verified ID**. 

   ![](./Images/ETS3136.png)
   >**Note**: if you haven't get the option **Get my Verified ID**, wait for few minutes and click on refresh.

1. A QR code is displayed. Scan it from the Authenticator app in your mobile and **Add** the credentials to your Verified ID. Once it is completed click on **Done**

   ![](./Images/ETS3137.png)

1. Now navigate back to Microsoft Entra admin center. on the Overview page of Verified ID, click on **Try it now** under use your credentials.

   ![](./Images/ETS3135.png)

1. It will redirect to a sample website to use your credentials. Click on **Access discounts**.

   ![](./Images/ETS3138.png)

1. On the sign-in page, Click on **Verify my employee credentials**

   ![](./Images/ETS3139.png)

1. A QR will be displayed. Scan it from the Authenticator App in your mobile and click on **Share**.

1. Once it is successful, you can see discount is applied and price of the latops has been changes.

   ![](./Images/ETS3140.png)

## Task 3: Verify Credentials in Sample Application (Help Desk Scenario)

In this task, you will simulate a real-world help desk identity verification scenario using Microsoft Entra Verified ID. Instead of traditional methods like passwords or security questions, the user will present a verified credential to securely prove their identity.

1. Now navigate back to Microsoft Entra admin center. click on **Organization setting** under Verified ID and copy the **DID Authority** and paste it in the notepad.

   ![](./Images/ETS3201.png)

1. Open a new tab and Paste the below link to deploy a helpdesk app using App services in Azure portal.

   ```
   https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Factive-directory-verifiable-credentials-dotnet%2Fmain%2F6-woodgrove-helpdesk%2FARMTemplate%2Ftemplate.json
   ```

1. If Prompted to sign in, provide the below credentials:

   - Username: Paste the username  **<inject key="AzureUserEmail"></inject>** then click on **Next**.
      ![](./Images/ETS3107.png)

   - Password:  Paste the password **<inject key="AzureUserEmail"></inject> (1)** and click on **Sign in (2)**.
      ![](./Images/ETS3108.png)

      >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

1. provide the below information and click on **Review + create (5)**.

   - **Subscription**: Leave it as default **(1)**
   - **Resource group**: ODL-<inject key="DeploymentID"></inject> **(2)**
   - **Webapp name**: Appservices<inject key="DeploymentID"></inject> **(3)**
   - **DID Authority**: Paste DID Authority (copied in task 3 step) **(4)**.

   ![](./Images/ETS3202.png)

1. Click on **Create**.

   ![](./Images/ETS3203.png)

1. Once the deployment is succeeded then click on **Go to resource group**>

   ![](./Images/ETS3204.png)

1. Click on **Appservice<inject key="DeploymentID"></inject>**. 

   ![](./Images/ETS3205.png)

1. On the overview page, click on **Default domain**.

   ![](./Images/ETS3206.png)

### Step 2: Configure Verification Request with Required Claims

1. In the Microsoft Entra admin center, navigate to **Verified ID** > **Overview**.

2. Review the **Verifier configuration** — the verification request specifies:
   - Which credential type to request
   - Which claims to extract from the presented credential
   - The purpose/reason for the verification

3. In the sample verification application, look for a **Settings** or **Configuration** page.

4. Verify that the application is configured to request:
   - **Credential type**: `VerifiedEmployee`
   - **Accepted issuers**: Your tenant's DID (e.g., `did:web:contoso.com`)
   - **Required claims**: `displayName`, `jobTitle`

   ![Verification configuration](Images/ex3-task3-verification-config.png)

### Step 3: Present Credential from Authenticator to Verify Identity

1. On the verification application page, click **Verify My Identity** or **Verify Employee Credential**.

2. A QR code will be displayed on screen (or a deep link if accessing on mobile).

   ![Verification QR code](Images/ex3-task3-verification-qr.png)

3. On your smartphone with Microsoft Authenticator:
   
   a. Open the **Microsoft Authenticator** app
   
   b. Navigate to the **Verified ID** or **Credentials** section
   
   c. Tap on the **Verified Employee** credential
   
   d. Tap **Present** or use the scan function to scan the QR code
   
   e. The app will display what claims will be shared and with whom

4. Review the presentation request:
   - **Requestor**: The verification application (help desk)
   - **Claims requested**: Display name, job title
   - **Purpose**: Identity verification for help desk

5. Tap **Share** to present the credential.

### Step 4: Review Verification Results and Extracted Claims

1. After presenting the credential, return to the browser showing the verification application.

2. The verification result page should display:
   - **Verification status**: ✅ Verified
   - **Credential type**: VerifiedEmployee
   - **Issuer**: Contoso Corporation (your DID)
   - **Extracted claims**:
     - Name: Alex Johnson
     - Job Title: IT Manager
   - **Credential validity**: Valid (within expiry date)
   - **Credential not revoked**: True

   ![Verification result](Images/ex3-task3-verification-result.png)

3. In the Microsoft Entra admin center, navigate to **Verified ID** > **Credentials** > **VerifiedEmployee**.

4. Review the **Verification activity** section to see the recent verification event logged.

### Step 5: Review Verification Logs in Microsoft Entra

1. Navigate to **Identity** > **Monitoring & health** > **Audit logs**.

2. Filter by:
   - **Service**: Verified ID (or Decentralized Identity)
   - **Date**: Today

3. Review the audit log entries for:
   - Credential issuance events
   - Credential verification events

4. Click on a log entry to see detailed information including:
   - The request ID
   - The verifier application
   - The credential type presented
   - Whether verification was successful

### Step 6: Understanding Real-World Help Desk Use Cases

The help desk scenario demonstrates several key benefits of Verified ID:

| Traditional Help Desk | With Verified ID |
|----------------------|------------------|
| User answers security questions (easily guessed) | User presents cryptographically signed credential |
| Help desk manually looks up user in HR system | Claims are automatically extracted from the credential |
| Identity verification can take 5-10 minutes | Verification happens in seconds |
| No audit trail of how identity was verified | Full audit trail of every credential presentation |
| Vulnerable to social engineering attacks | Resistant to social engineering — credential cannot be forged |

**Additional Use Cases for Verified ID:**

1. **Partner/Vendor Onboarding**: Third-party companies can issue credentials to their employees, which your organization can verify without needing to trust the partner's identity provider directly.

2. **Age Verification**: Verify a user is over 18 without collecting unnecessary personal data.

3. **Professional Certification**: Verify a user holds a specific professional certification (e.g., issued by a professional body).

4. **Alumni Access**: Universities can issue credentials to graduates that employers or other organizations can verify.

5. **High-Value Transaction Authorization**: Require employees to present a Verified ID credential before approving high-value financial transactions.

## Summary

In this exercise, you configured Microsoft Entra Verified ID by setting up the issuer, registering a decentralized identifier (DID), and verifying domain ownership. You created and issued a verified credential, stored it in the Authenticator app, and successfully used it for identity verification. Finally, you simulated a help desk scenario to understand how Verified ID can replace traditional authentication methods with a more secure and user-friendly approach.
