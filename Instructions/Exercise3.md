# Exercise 3: Microsoft Entra Verified ID

## Estimated Duration: 70 minutes

## Overview

In this hands-on exercise, you will set up a **Verified ID** using which we will register a **Decentralized identifier (DID)**, and verify the domain. You will create and issue a credential which would stored in the **Authenticator app**. You then verify the credential through a sample application. Finally, you will explore a **Help Desk scenario** to understand how Verified ID enables secure, passwordless identity verification.

**Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Issuer** | An organization that creates and issues verifiable credentials to users. |
| **Holder** | The user who receives and stores credentials in their digital wallet (Microsoft Authenticator). |
| **Verifier** | An organization or application that requests and verifies credentials from the holder. |
| **DID (Decentralized Identifier)** | A globally unique identifier anchored to a blockchain or DID registry. |
| **Verifiable Credential (VC)** | A tamper-evident credential containing claims about the holder, signed by the issuer. |

## Prerequisites

The following are prerequisites to complete this exercise which are already configured in this lab environment:

- Microsoft Entra ID P1 or P2 license (Verified ID is included).
- Global Administrator role.
- A custom domain configured in your Microsoft Entra tenant.
- A smartphone with **Microsoft Authenticator** app installed.

## Lab Objectives

In this lab, you will complete the following exercise:

- Task 1: Configure Verified ID Issuer
- Task 2: Issue Verifiable Credentials
- Task 3: Verify Credentials in Sample Application (Help Desk Scenario)

## Task 1: Configure Verified ID Issuer

In this task, you will set up Microsoft Entra Verified ID for your organization. You will register your DID, verify your domain, and create a verified credential that can be issued to users.

1. On the Microsoft Entra admin center left navigation pane, expand **Verified ID** and select **Overview (1)** then click on **Configure (2)**.

   ![](./Images/E3T1S1.png)

1. On the **Define organization settings** wizard, Provide the below details and click on **Select keys (3)** for Key vault selection.

   - **Organization**: `VerifiedID` **(1)**.
   - **Trusted domain**: Open the **AzureCreds** file located on Desktop then copy and paste the endpoint **(2)**.

      ![](./Images/E3T1S2.png)

1. On the **Select keys from Azure Key Vault** wizard, leave the Subscription as **default (1)** then from the Key vault dropdown, select **kv-<inject key="DeploymentID"></inject> (2)** and click on **Select (3)**.

   ![](./Images/E3T1S3.png)

1. Click on **Save** to save the organization settings.

   ![](./Images/E3T1S4.png)

1. Now on the **Overview** page, Click on **Register** under the Register decentralized ID.

   ![](./Images/E3T1S5.png)

1. Click on **Download**.

   ![](./Images/E3T1S6.png)

1. Open a new tab, paste the provided link, and log in into Azure portal using below **credentials**.

   ```
   https://portal.azure.com
   ```

   - **Username:** Paste the username  **<inject key="AzureAdUserEmail"></inject> (1)** then click on **Next (2)**.

      ![](./Images/GS6.png)

   - **Password:**  Paste the password **<inject key="AzureAdUserPassword"></inject> (1)** and click on **Sign in (2)**.

      ![](./Images/GS6-1.png)

   >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.  

1. Search for **storage account (1)** and select  **Storage accounts (2)**.

   ![](./Images/ETS3109.png)

1. Select **stweb<inject key="DeploymentID" enableCopy="false" />** storage account.

   ![](./Images/E3T1S9.png)

1. Go to **Containers (1)** under Data storage and select **$web (2)** container.

   ![](./Images/E3T1S10.png)

1. Click on **+ Add directory (1)** then provide the name as **.well-known (2)** and click on **Ok (3)**.
   
   ![](./Images/E3T1S11.png)

   ![](./Images/E3T1S11i.png)

1. Click on **Upload (1)** and select **Browse for the files (2)**.

   ![](./Images/E3T1S12.png)

1. Click on **Downloads (1)** and select **did.json (2)** then click on **Open (3)**.

   ![](./Images/E3T1S13.png)

1. Click on **Upload** to upload the file to the directory.

   ![](./Images/E3T1S14.png)

1. Now, naviagte back to Microsoft Entra admin center, and click on **Refresh registration status (1)**. Once the status is **Registered (2)**, click on **Close (3)**.

   ![](./Images/E3T1S15.png)

1. Now on the overview page, Click on **Verify** under Verify domain membership.

   ![](./Images/E3T1S16.png)

1. Click on **Download (1)**.

   ![](./Images/E3T1S17.png)

1. Navigate back to Azure portal, click on **Upload (1)** and select **Browse for files (2)**.

   ![](./Images/E3T1S18.png)

1. Click on **Downloads (1)** and select **did-configuration.json (2)** then click on **Open (3)**.

   ![](./Images/E3T1S19.png)

1. Click on **Upload** to upload the file to the directory.

   ![](./Images/E3T1S20.png)

1. Now, naviagte back to **Microsoft Entra admin center**, and click on **Refresh registration status (1)**. Once the status is **Verified (2)**, click on **Close (3)**.

   ![](./Images/E3T1S21.png)

1. On the **Overview** page, now you can see the **Verified Domain**.

   ![](./Images/E3T1S22.png)

   >**Note**: if it is still showing **Domain may not be verified**, click on **Verify**, select **Refresh registration status**. If the registration is successful, click on **Close** and check again.
   >![](./Images/E3T1S22-1.png)

1. Now, click on **+ Create credential** to create a new credential type.

   ![](./Images/E3T1S23.png)

1. Click **Verified employee** and then **Next**.

   ![](./Images/E3T1S24.png)

1. Provide below Information and click on **Update (4)** and verify the display card styling:

   - **Logo URL**: `https://avd233.blob.core.windows.net/avdtest/VerifiedID.jpg` **(1)**
   - **Text color**: #FFFFFF **(2)**
   - **Background color** : #0068cd **(3)**.

      ![](./Images/E3T1S25.png)

1. Scroll down and click on **Create**.

   ![](./Images/E3T1S26.png)

## Task 2: Issue Verifiable Credentials

In this task, you will configure the credential issuance process and issue a verifiable credential to a test user who will receive it in their Microsoft Authenticator app.

1. Navigate to **Verified employee** page, click on **Issue a credential (1)**. Ensure **Allow all Microsoft Entra ID users within the tenant (2)** is selected and check the box **Issue credentials through My Account (3)**.

   ![](./Images/E3T2S1-1.png)

1. Scroll down and then click on **Save**.

   ![](./Images/E3T2S2.png)

1. Navigate to  **Overview (1)** blade in **Verified ID** section, then click on **Try it now (2)** under Get the new credentials.

   ![](./Images/E3T2S3.png)

1. You will be redirected to the **MyAccount** portal. To continue with the lab using the classic experience, click on **Use previous version**.

   ![](./Images/E3T2S4-1.png)

   > **Note**: If there is any popup wizard appears with **Feedback to Microsoft** click on **Skip**.

      ![](./Images/E3T2S4N.png)

 1. Click on **Get my Verified ID**. 

    ![](./Images/E3T2S4.png)

      >**Note**: If you haven't get the option **Get my Verified ID**, wait for few minutes and click on refresh.

1. A QR code is displayed **(1)**. Scan it from the Authenticator app in your mobile and **Add** the credentials to your Verified ID. Once it is completed click on **Done (2)**.

   ![](./Images/E3T2S5.png)

1. Now navigate back to Microsoft Entra admin center. On the **Overview** page of Verified ID, click on **Try it now** under use your credentials.

   ![](./Images/E3T2S6.png)

1. It will redirect to a sample website to use your credentials. Click on **Access discounts**.

   ![](./Images/E3T2S7.png)

1. On the sign-in page, Click on **Verify my employee credentials**.

   ![](./Images/E3T2S8.png)

1. A QR will be displayed. Scan it from the Authenticator App in your mobile and click on **Share**.

   ![](./Images/E3T2S9.png)

1. Once it is successful, you can see discount is applied and price of the latops has been changes.

   ![](./Images/E3T2S10.png)

1. Navigate back to **Microsoft Entra Admin Centre** portal, then **Credenitals (1)** under Verified ID. Select **Verified employee(2)**. 

   ![](./Images/E3T2S11.png)

1. Copy the **Manifest URL** and Paste the value into Notepad for later use.

   ![](./Images/E3T2S12.png)

## Task 3: Verify Credentials in Sample Application (Help Desk Scenario)

In this task, you will simulate a real-world help desk identity verification scenario using Microsoft Entra Verified ID. Instead of traditional methods like passwords or security questions, the user will present a verified credential to securely prove their identity.

### Task 3.1: Deploy sample verification application

1. Over the Microsoft Entra admin center, in the left navigation click on **Organization settings** under **Verified ID**, then copy the **Decentralized identifier (DID)** and paste the value into Notepad for later use.

   ![](./Images/E3T3-1S1.png)

1. Navigate back to the Azure portal and search for **Microsoft Entra ID**.

   ![](./Images/E3T3-1S2.png)

1. From the left navigation pane, select **App registrations (1)** and then click on **All applications (2)**. Select the application with name **odl_user_sp_<inject key="DeploymentID" enableCopy="false" /> (3)**.

   ![](./Images/E3T3-1S3.png)


1. From the **Overview** page of the application, copy the following values and paste them into Notepad:

   - **Application (client) ID**
   - **Directory (tenant) ID**

      ![](./Images/E3T3-1S4.png)

1. From the left navigation pane, select **Certificates & secrets** from **Manage** Section. Under **Client secrets** tab, click on **+ New client secret**.

   ![](./Images/E3T3-1S5.png)

1. On the **Add a client secret** page, provide the following details and Click on **Add (3)**.

   - **Description**: `authentication` **(1)**.
   - **Expires**: Keep the default value **(2)**.

      ![](./Images/E3T3-1S6.png)

1. Once the client secret is created, copy the **Value** and paste it into Notepad. This value will be required during the web application deployment.

   ![](./Images/E3T3-1S7.png)

   > **Note**: Once you navigate from this page the secret value will be encrypted and will not able to copy it again. 

1. Now from the **Manage** section, select **API permissions (1)** and then click on **+ Add a permission (2)**.

   ![](./Images/E3T3-1S8.png)

1. On the **Request API permissions** page, select **APIs my organization uses (1)**. Search for and select **Verifiable Credentials Service Request (2) (3)**.

   ![](./Images/E3T3-1S9.png)

1. Select **Application permissions (1)**. Check the box for **VerifiableCredential.Create.All (2)** and then click on **Add permissions (3)**.

   ![](./Images/E3T3-1S10.png)

1. Back on the **Configured permissions** page, click on **Grant admin consent for...**.

   ![](./Images/E3T3-1S11.png)

1. On the **Grant admin consent confirmation** pop-up, click on **Yes**.

   ![](./Images/E3T3-1S12.png)

1. Open a new browser tab and paste the following link to deploy the **Help Desk verification application** using Azure App Service.

   ```
   https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Factive-directory-verifiable-credentials-node%2Fmain%2F1-node-api-idtokenhint%2FARMTemplate%2Ftemplate.json
   ```

1. If Prompted to sign in, provide the below credentials:

   - Username: Paste the username  **<inject key="AzureUserEmail"></inject>** **(1)** then click on **Next (2)**.

      ![](./Images/GS6.png)

   - Password:  Paste the password **<inject key="AzureUserEmail"></inject> (1)** and click on **Sign in (2)**.

      ![](./Images/GS6-1.png)

      >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

1. Provide the below detail, click on **Review + create (10)** and then **Create**.

   - **Subscription**: Leave it as default **(1)**.
   - **Resource group**: ODL-Entra-<inject key="DeploymentID"></inject>-01 **(2)**.
   - **Web App name**: helpdesk<inject key="DeploymentID"></inject> **(3)**.
   - **Az Tenant Id:** Paste the tenant ID copied in Step 4 **(4)**.
   - **Az Client Id:** Paste the application ID copied in Step 4 **(5)**.
   - **Az Client Secret:** Paste the value copied in Step 7 **(6)**.
   - **DID Authority**: Paste DID Authority copied in Step 1 **(7)**.
   - **Credential Manifest:** Paste the value copied in Task 2 Step 12 **(8)**.
   - **Credential Type:** ``VerifiedEmployee`` **(9)**.

      ![](./Images/E3T3-1S15.png)

1. Once the deployment is succeeded then click on **Go to resource group**.

   ![](./Images/E3T3-1S16new.png)

1. Click on **helpdesk<inject key="DeploymentID"></inject>** app service that is listed in the resource group.

   ![](./Images/E3T3-1S17.png)

1. From the left navigation pane of the App Service, under **Settings** select **Environment variables (1)**. Locate the setting named **issuancePinCodeLength (2)** and select it.

   ![](./Images/E3T3-1S18.png)

1. Clear the value of the setting, ensuring that the value field is completely empty. Click on **Apply** to save the changes.

   ![](./Images/E3T3-1S19.png)

1. From the left navigation pane, under **Settings**, select **Configuration (1)** and then navigate to the **Stack settings (2)** tab. Under **Startup command (3)**, replace the existing value with the following command. Click on **Apply (4)** to save the configuration changes.

   ```
   node app.js
   ```
   ![](./Images/E3T3-1S20.png)

1. On the **Overview** page, click on **Default domain** to open the application page.

   ![](./Images/E3T3-1S21.png)

### Task 3.2: Configure Verification Request with Required Claims

1. Navigate to the sample verification application deployed in the previous step.

1. On the application homepage, review the available verification options Click on **Issue Credential**, then again click on **Issue Credential**.

      ![](Images/E3T3-2S2.png)

      ![](Images/E3T3-2S2-1.png)

1. A QR code will be displayed, on your mobile phone, scan the QR code. It will open a Authenticator app. 

   ![](Images/E3T3-2S3.png)

1. On Authenticator, click on Sign in to your account, add an account by providing below mentioned credentials and click on **Next**.
   
      - Enter **Username/Email: <inject key="AzureAdUserEmail"></inject>** in the **Sign in** field. Click **Next** to continue.

      - Enter **Password: <inject key="AzureAdUserPassword"></inject>** and click **Sign in**.

1. Once the credentials are added, on the sample application, you will get a message stating **Issuance completed**. 

   ![](Images/E3T3-2S5.png)

### Task 3.3: Present Credential from Authenticator to Verify Identity

1. Navigate back to Azure portal, click on Default Domain to launch the sample application again.

1. On the application page, click **Verify Credential**.

   ![](Images/E3T3-3S2.png)

1. On the **Presentation of VerifiedEmployee** page, click on **Verify Credential**.

   ![](Images/E3T3-3S3.png)

1. A QR code will be displayed on screen (or a deep link if accessing on mobile).

   ![](Images/E3T3-3S4.png)

1. On your smartphone with Microsoft Authenticator:
   
   - Open the **Microsoft Authenticator** app, scan the QR code.
   
   - It will navigate to the **Verified ID** section.
   
   - Tap on the **Verified Employee** credential and then **confirm**. Select **Next**.

1. After presenting the credential, return to the browser displaying the verification application.

### Task 3.4: Review Verification Results and Extracted Claims

1. Review the verification result page. The page should display a successful credential presentation message similar to **VerifiedEmployee Presentation received**.

   ![](Images/E3T3-4S1.png)

1. Observe the extracted claims displayed by the application, including:
   - `displayName`
   - `givenName`
   - `surname`
   - `mail`
   - `revocationId`
   - `photo` (if configured)

      ![](Images/E3T3-4S2.png)

1. Review the verifiable credential details displayed on the page, such as:
   - **Issuer DID**
   - **Credential type**
   - **Credential state**
   - **Issuance date**
   - **Expiration date**
   - **Domain validation details**

      ![](Images/E3T3-4S3.png)
   
1. Verify that:
   - The credential type is displayed as `VerifiedEmployee`.
   - The credential revocation status shows `VALID`.
   - The credential contains the expected user identity claims.

      ![](Images/E3T3-4S4.png)

3. In the Microsoft Entra admin center, navigate to **Verified ID** > **Overview** section, scroll down to see the **Activity log**. Review the recent verification activity displayed in the activity log.

   ![](Images/E3T3-4S5.png)

### Task 3.5: Review Verification Logs in Microsoft Entra

1. Navigate to **Entra Admin center** portal, under the **Monitoring & health** click on **Audit logs**.

1. Apply the following filers:
   - **Service**: Verified ID.
   - **Date range**: Last 24 hours.
      
      ![](Images/E3T3-5S1.png)

1. Review the audit log entries displayed on the page. Observe the different Verified ID activities recorded in the audit logs, including:
   - **Verifiable Credential Issued**
   - **Create authority**
   - **Create contract**
   - **Update MyAccount settings**

      ![](Images/E3T3-5S3.png)

1. Click on a verification or issuance activity entry to review the detailed audit information.

   ![](Images/E3T3-5S4.png)

1. Observe the audit log details, including:
   - **Activity Type** (for example, Verifiable Credential Issued)
   - **Date and time** of the operation
   - **Status** of the request
   - **Correlation ID**
   - **Initiated by (actor)** details
   - **Application information**
   - **Service principal details**

      ![](Images/E3T3-5S5.png)

1. Verify that the activity status is displayed as **Success**, confirming that the verifiable credential operation was completed successfully.

   ![](Images/E3T3-5S6.png)

# Task 3.6: Review Face Check Verification Flow in Help Desk Application (Read-Only Task)

In this task, you will review how Face Check can be integrated into the Help Desk verification application deployed in this lab. Although Face Check is not configured in the current lab environment, the following steps demonstrate how the verification flow would work in a real-world implementation.

> **Note:** This is a read-only task intended for learning purposes only. Face Check is not configured in the current lab environment and cannot be fully validated in this test environment.

1. In the Verified ID overview page, scroll down to the **Add-ons** section and verify whether the **Face Check** add-on is enabled.

   ![](Images/E3T3-6S1.png)

1. Navigate back to the deployed Help Desk verification application by opening the **Default domain** of the App Service created in the previous task.

   ![](Images/E3T3-1S21.png)

1. On the application homepage, click on **Issue Credential**.

   ![](Images/E3T3-2S2.png)

1. Observe the credential issuance page displaying the **Take Selfie** option.

   ![](Images/E3T3-6S4.png)

1. Review the information displayed on the page. Observe that:

   - The credential includes a photo claim.
   - The photo can later be used for biometric Face Check verification during credential presentation.
   - The user can either:
     - Take a live selfie.
     - Upload an existing portrait image.

1. Click on **Take Selfie**. Observe that a QR code is generated for camera access.

   ![](Images/E3T3-6S6.png)

1. In a production environment, the user would perform the following steps on a mobile device:

   - Open a QR code reader application.
   - Scan the displayed QR code.
   - Select **Open Camera**.
   - Capture a live selfie using the mobile device camera.
   - Select **Use photo** to attach the image to the credential.

1. Once the photo **(1)** is uploaded, click on **Issue Credential (2)**. You will get a message stating **Issuance completed (3)**. Observe that the captured photo is not permanently stored by the application and is only embedded into the issued credential stored in the user’s wallet. 

   ![](Images/E3T3-6S8.png)

   ![](Images/E3T3-6S8-1.png)

1. Navigate back to **Azure Portal**, click on Default Domain to launch the sample application. On the application page, click **Verify Credential**. 

   ![](Images/E3T3-6S9.png)

1. On the Presentation of VerifiedEmployee page, click on **Verify Credential**.

   ![](Images/E3T3-3S3.png)
 
1. A QR code would then appear for credential issuance.

   ![](Images/E3T3-3S4.png)

1. The user would scan the QR code using **Microsoft Authenticator** to receive the credential containing the embedded photo claim.

1. During a future credential verification request, Microsoft Entra Verified ID Face Check could compare:

   - The live selfie captured during verification.
   - The photo stored inside the credential.

1. Once the verification is completed, review the verification result page. The page should display a successful credential presentation message similar to **VerifiedEmployee Presentation received**.

   ![](Images/E3T3-4S1.png)

1. This additional verification layer helps confirm that the person presenting the credential is the legitimate credential owner.

1. Since Face Check requires additional licensing, configuration, and verification setup, the end-to-end biometric validation is not enabled in the current lab environment. This task is provided to help you understand the workflow and integration experience within the deployed Help Desk application.
 
### Task 3.7: Understanding Real-World Help Desk Use Cases

Consider a scenario where an employee contacts the IT help desk requesting access to a sensitive internal application or requesting an account unlock.

Traditionally, the help desk agent might verify the employee’s identity using security questions, employee IDs, or manual checks against HR records. These methods can be time-consuming and may still be vulnerable to impersonation or social engineering attacks.

With Microsoft Entra Verified ID, the employee can securely present a Verified Employee credential from Microsoft Authenticator. The help desk application verifies the credential and validates claims such as the employee’s name and job title before granting access or processing the request.

In this exercise, the verification application simulated the help desk system, while Microsoft Authenticator was used to securely present the employee credential.

The help desk scenario demonstrates several key benefits of Verified ID:

| Traditional Help Desk | With Verified ID |
|----------------------|------------------|
| User answers security questions (easily guessed) | User presents cryptographically signed credential |
| Help desk manually looks up user in HR system | Claims are automatically extracted from the credential |
| Identity verification can take 5-10 minutes | Verification happens in seconds |
| No audit trail of how identity was verified | Full audit trail of every credential presentation |
| Vulnerable to social engineering attacks | Resistant to social engineering — credential cannot be forged |

Observe how:
- The credential was verified cryptographically.
- Claims were securely shared from Microsoft Authenticator.
- Identity verification was completed within seconds.
- Verification activity was recorded for auditing purposes.

**Additional Use Cases for Verified ID:**

1. **Partner/Vendor Onboarding**: Third-party companies can issue credentials to their employees, which your organization can verify without directly relying on the partner’s identity provider.

2. **Age Verification**: Verify that a user meets an age requirement without collecting unnecessary personal information.

3. **Professional Certification**: Validate professional certifications issued by trusted organizations.

4. **Alumni Access**: Universities can issue digital credentials to graduates for verification by employers or other institutions.

5. **High-Value Transaction Authorization**: Require employees to present a Verified ID credential before approving sensitive or high-value transactions.
   
## Summary

In this exercise, you configured Microsoft Entra Verified ID by setting up the issuer, registering a decentralized identifier (DID), and verifying domain ownership. You created and issued a verified credential, stored it in the Authenticator app, and successfully used it for identity verification. Finally, you simulated a help desk scenario to understand how Verified ID can replace traditional authentication methods with a more secure and user-friendly approach.

## Congratulations! You have successfully completed the lab.
