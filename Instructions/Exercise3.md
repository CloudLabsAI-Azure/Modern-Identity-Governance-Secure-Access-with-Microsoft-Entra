# Exercise 3: Microsoft Entra Verified ID

**Estimated Duration: 60 minutes**

> **Note:** Advanced users may complete this exercise in 20–30 minutes.

## Overview

Microsoft Entra Verified ID is a decentralized identity solution built on open standards (W3C Verifiable Credentials and DID). It allows organizations to issue digitally-signed credentials that users store in their Microsoft Authenticator wallet, and to verify those credentials without relying on a central identity provider database.

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
- A custom domain configured in your Microsoft Entra tenant (or a pre-configured lab domain)
- A smartphone with **Microsoft Authenticator** app installed (version 6.6.8 or later)
- For Task 3: Access to the Azure portal to deploy a sample application

---

## Task 1: Configure Verified ID Issuer

**Estimated Duration: 20 minutes**

### Step 1: Enable Verified ID Service

1. Sign in to the [Microsoft Entra admin center](https://entra.microsoft.com) using your Global Administrator credentials.

2. In the left navigation pane, expand **Verified ID** and click **Overview** (or navigate to **Verified ID** > **Setup**).

   ![Verified ID overview](Images/ex3-task1-verified-id-overview.png)

3. Click **Get started** to begin the Verified ID setup wizard.

4. Review the **Before you begin** information:
   - Custom domain requirement
   - Azure Key Vault requirement
   - Licensing requirements

5. Click **Next** to proceed.

### Step 2: Configure Azure Key Vault

1. In the Verified ID setup wizard, you will be prompted to configure Azure Key Vault.

2. Click **Create a key vault** (or select an existing Key Vault if available):

   a. Navigate to the [Azure portal](https://portal.azure.com) in a new tab
   
   b. Search for **Key vaults** and click **+ Create**
   
   c. Configure:
      - **Subscription**: Your lab subscription
      - **Resource group**: `rg-identity-lab`
      - **Key vault name**: `kv-verifiedid-lab` (must be globally unique)
      - **Region**: Select your preferred region
      - **Pricing tier**: Standard
   
   d. Click **Review + create**, then **Create**
   
   e. After deployment, copy the **Key Vault URI** (e.g., `https://kv-verifiedid-lab.vault.azure.net/`)

3. Return to the Microsoft Entra admin center Verified ID setup.

4. Enter or select the Key Vault details.

### Step 3: Configure the Issuer Authority and DID Registration

1. Back in the Verified ID setup wizard, configure the **Organization details**:
   - **Organization name**: `Contoso Corporation` (or your organization name)
   - **Trusted domain**: Your custom domain (e.g., `contoso.com`)

   > **Lab Note:** The trusted domain must be a verified domain in your Microsoft Entra tenant. In a lab environment, this is typically pre-configured.

2. Review the **DID (Decentralized Identifier)** that will be created:
   - The DID will be anchored using the **Web DID method** (did:web)
   - Example: `did:web:contoso.com`

3. Click **Save** or **Register**.

4. The system will:
   a. Create the DID document
   b. Instruct you to publish the DID document to your web domain at `https://yourdomain.com/.well-known/did.json`
   
   > **Important:** For production environments, you must publish the DID document to your domain. In a lab environment, this step may be pre-automated.

5. Follow the instructions to publish the DID document:
   - Download the `did.json` file
   - Upload it to your website at the path `/.well-known/did.json`
   - Verify it is publicly accessible

6. Click **Verify** to confirm the DID document is accessible.

   ![DID registration](Images/ex3-task1-did-registration.png)

### Step 4: Create Credential Type and Display Definition

1. Navigate to **Verified ID** > **Credentials** in the Microsoft Entra admin center.

2. Click **+ Add credential** to create a new credential type.

3. Select **Custom credential** (or select from available templates if present).

   > **Available templates include:** Verified Employee, Verified Customer, Custom credential

4. Click **Custom credential** and then **Next**.

5. On the **Credential name** page:
   - **Credential name**: `VerifiedEmployee`
   - **Description**: `Verified employee credential for Contoso Corporation`

6. Click **Next: Display**.

7. On the **Display definition** page, configure how the credential will appear in the user's wallet:

   Paste the following JSON for the display definition:
   ```json
   {
     "locale": "en-US",
     "card": {
       "title": "Verified Employee",
       "issuedBy": "Contoso Corporation",
       "backgroundColor": "#000000",
       "textColor": "#ffffff",
       "logo": {
         "uri": "https://raw.githubusercontent.com/Azure-Samples/active-directory-verifiable-credentials/main/1-node-api-idtokenhint/public/images/did_logo.png",
         "description": "Contoso Logo"
       },
       "description": "This credential verifies your employment at Contoso Corporation"
     },
     "consent": {
       "title": "Do you want to accept the Verified Employee credential?",
       "instructions": "Sign in and receive your credential."
     },
     "claims": [
       {
         "claim": "vc.credentialSubject.displayName",
         "label": "Name",
         "type": "String"
       },
       {
         "claim": "vc.credentialSubject.givenName",
         "label": "First Name",
         "type": "String"
       },
       {
         "claim": "vc.credentialSubject.surname",
         "label": "Last Name",
         "type": "String"
       },
       {
         "claim": "vc.credentialSubject.jobTitle",
         "label": "Job Title",
         "type": "String"
       },
       {
         "claim": "vc.credentialSubject.preferredLanguage",
         "label": "Preferred Language",
         "type": "String"
       }
     ]
   }
   ```

8. Click **Next: Rules**.

9. On the **Rules definition** page, configure what claims will be included in the credential:

   Paste the following JSON for the rules definition:
   ```json
   {
     "attestations": {
       "idTokenHints": [
         {
           "mapping": [
             {
               "outputClaim": "displayName",
               "required": true,
               "inputClaim": "$.displayName",
               "indexed": false
             },
             {
               "outputClaim": "givenName",
               "required": true,
               "inputClaim": "$.givenName",
               "indexed": false
             },
             {
               "outputClaim": "surname",
               "required": true,
               "inputClaim": "$.surname",
               "indexed": true
             },
             {
               "outputClaim": "jobTitle",
               "required": false,
               "inputClaim": "$.jobTitle",
               "indexed": false
             },
             {
               "outputClaim": "preferredLanguage",
               "required": false,
               "inputClaim": "$.preferredLanguage",
               "indexed": false
             }
           ],
           "required": false
         }
       ]
     },
     "validityInterval": 2592000,
     "vc": {
       "type": [
         "VerifiedEmployee"
       ]
     }
   }
   ```

   > **Note:** `validityInterval` is in seconds. 2592000 seconds = 30 days.

10. Click **Next: Review** and then **Create credential**.

   ![Credential created](Images/ex3-task1-credential-created.png)

11. Note the **Credential issuer DID** and the **Credential manifest URL** shown on the credential details page. You will need these in Task 2.

---

## Task 2: Issue Verifiable Credentials

**Estimated Duration: 20 minutes**

### Overview

In this task, you will configure the credential issuance process and issue a verifiable credential to a test user who will receive it in their Microsoft Authenticator app.

### Step 1: Review the Credential Issuance Request Configuration

1. In the Microsoft Entra admin center, navigate to **Verified ID** > **Credentials**.

2. Select the **VerifiedEmployee** credential type created in Task 1.

3. Review the **Credential manifest URL** — this is the endpoint that issuance apps call to initiate credential issuance.

4. Click **Issue credential** to open the quick issuance interface.

   ![Issue credential button](Images/ex3-task2-issue-credential.png)

### Step 2: Use the Sample Issuance Application

Microsoft provides a sample application to demonstrate credential issuance. In this step, you will deploy or use the pre-deployed sample.

> **Lab Note:** In a lab environment, the sample issuance app may already be deployed. If so, navigate to the provided URL and skip steps 3–8 below.

#### Option A: Use the Microsoft Quick Issue Feature (Recommended for Lab)

1. In the Microsoft Entra admin center, navigate to **Verified ID** > **Credentials**.

2. Select **VerifiedEmployee** and click **Issue credential**.

3. In the **Issue credential** panel, fill in the credential data:
   - **Display Name**: Enter the test user's full name (e.g., `Alex Johnson`)
   - **Given Name**: `Alex`
   - **Surname**: `Johnson`
   - **Job Title**: `IT Manager`
   - **Preferred Language**: `en-US`

4. Click **Issue** to generate a QR code.

5. A **QR code** will be displayed on screen.

#### Option B: Deploy the Sample Application (Advanced)

1. Open the [Microsoft Entra Verified ID samples repository](https://github.com/Azure-Samples/active-directory-verifiable-credentials-dotnet).

2. Follow the README to deploy the **1-asp-net-core-api-idtokenhint** sample:
   
   a. Clone the repository or use the Azure "Deploy to Azure" button
   
   b. Configure the app settings with your tenant ID, client ID, and credential manifest URL
   
   c. Deploy to Azure App Service

3. Navigate to the deployed sample application URL.

4. Click **Issue Credential** on the sample app.

5. A QR code will be displayed on screen.

### Step 3: Receive the Credential in Microsoft Authenticator

1. On your smartphone, open the **Microsoft Authenticator** app.

   > **Requirements:**
   > - Microsoft Authenticator version 6.6.8 or later
   > - Available on iOS (App Store) and Android (Google Play)

2. In the Authenticator app:
   - Tap the **+** icon or the menu
   - Select **Add account**
   - Select **Other account** or **Verifiable credential** (depending on version)
   - Or: Tap **More** (three dots) > **Verified ID**

   > **Alternative:** Some versions of Authenticator will prompt you to scan a QR code when you tap on a notification or directly from the wallet section.

3. Tap **Scan QR code** and scan the QR code displayed in the browser.

4. Review the credential offer details:
   - **Issuer**: Contoso Corporation
   - **Credential type**: Verified Employee
   - **Claims**: Your name, job title, etc.

5. Tap **Accept** to add the credential to your wallet.

6. The credential will now appear in the **Credentials** section of the Microsoft Authenticator app.

   ![Credential in Authenticator](Images/ex3-task2-credential-wallet.png)

### Step 4: Verify Credential Storage and Details

1. In the Microsoft Authenticator app, navigate to the **Credentials** or **Verified ID** section.

2. Tap on the **Verified Employee** credential.

3. Review the credential details:
   - **Issuer**: Contoso Corporation (or your organization)
   - **Issued date**: Today's date
   - **Expiry date**: 30 days from today
   - **Claims**: Display name, job title, etc.

4. Review the credential design (background color and logo as configured in the display definition).

5. Back in the Microsoft Entra admin center, navigate to **Verified ID** > **Credentials** > **VerifiedEmployee** and review:
   - **Issuance activity**: Number of credentials issued
   - **Recent issuances**: The test issuance should appear

---

## Task 3: Verify Credentials in Sample Application (Help Desk Scenario)

**Estimated Duration: 20 minutes**

### Overview

In this task, you will simulate a help desk identity verification scenario. A user needs to contact the IT help desk, and instead of relying on knowledge-based authentication (passwords, security questions), they will present their Verified ID credential to prove their identity.

### Step 1: Deploy the Sample Verification Application

> **Lab Note:** In the lab environment, the sample verification application should already be deployed. If so, navigate to the provided URL and proceed to Step 2.

#### Option A: Use Pre-Deployed Lab Application

Navigate to the URL provided in your lab guide for the pre-deployed verification application.

#### Option B: Deploy the Sample Verification App

1. Navigate to the [Verified ID Woodgrove Demo application](https://aka.ms/vcdemo) or use the [GitHub sample](https://github.com/Azure-Samples/active-directory-verifiable-credentials-dotnet).

2. To deploy using the Azure portal:

   a. Open the Azure portal and navigate to **App Services**.
   
   b. Click **+ Create** > **Web App**.
   
   c. Configure:
      - **App name**: `verifiedid-helpdesk-lab` (must be globally unique)
      - **Runtime stack**: .NET 7 (or the version used by the sample)
      - **Region**: Your preferred region
   
   d. Click **Review + create**, then **Create**.

3. After deployment, configure the application settings:
   - Navigate to **Configuration** > **Application settings**
   - Add the following settings (from your Verified ID configuration):
     - `AppSettings__TenantId`: Your tenant ID
     - `AppSettings__ClientId`: Your app registration client ID
     - `AppSettings__ClientSecret`: Your app registration client secret
     - `AppSettings__VCServiceScope`: `3db474b9-6a0c-4840-96ac-1fceb342124f/.default`
     - `AppSettings__IssuerAuthority`: Your Verified ID issuer DID
     - `AppSettings__VerifierAuthority`: Your Verified ID verifier DID

4. Deploy the application code from the GitHub repository.

5. Navigate to the deployed application URL.

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

---

## Summary

In this exercise, you have:

- ✅ Enabled the Microsoft Entra Verified ID service in your tenant
- ✅ Configured Azure Key Vault for credential signing key storage
- ✅ Registered a DID (Decentralized Identifier) for your organization
- ✅ Created a custom **VerifiedEmployee** credential type with display and rules definitions
- ✅ Issued a verifiable credential to a test user's Microsoft Authenticator wallet
- ✅ Deployed and configured a verification application simulating a help desk scenario
- ✅ Presented the credential from Authenticator to verify identity
- ✅ Reviewed verification results and audit logs
- ✅ Explored real-world use cases for decentralized identity verification

## Additional Resources

- [Microsoft Entra Verified ID Documentation](https://learn.microsoft.com/en-us/entra/verified-id/)
- [Verified ID Overview and Architecture](https://learn.microsoft.com/en-us/entra/verified-id/decentralized-identifier-overview)
- [Set Up Verified ID Tenant Configuration](https://learn.microsoft.com/en-us/entra/verified-id/verifiable-credentials-configure-tenant)
- [Issue Verifiable Credentials](https://learn.microsoft.com/en-us/entra/verified-id/verifiable-credentials-configure-issuer)
- [Verify Verifiable Credentials](https://learn.microsoft.com/en-us/entra/verified-id/verifiable-credentials-configure-verifier)
- [GitHub: Verified ID Code Samples](https://github.com/Azure-Samples/active-directory-verifiable-credentials-dotnet)
- [Microsoft Authenticator App](https://www.microsoft.com/en-us/security/mobile-authenticator-app)
- [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/)
