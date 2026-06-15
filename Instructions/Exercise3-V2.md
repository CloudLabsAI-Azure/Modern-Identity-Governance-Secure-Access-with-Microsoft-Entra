# Exercise 3: Microsoft Entra Verified ID

### Estimated Duration: 70 minutes

## Overview

In this hands-on exercise, you will set up a **Verified ID** using which we will register a **Decentralized identifier (DID)**, and verify the domain. You will create and issue a credential that will be stored in the **Authenticator app**. You then verify the credentials through a sample application. Finally, you will explore a **Help Desk scenario** to understand how Verified ID enables secure, passwordless identity verification.

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
- A smartphone with the Microsoft Authenticator app installed.

## Lab Objectives

In this lab, you will complete the following exercise:

- Task 1: Configure Verified ID Issuer
- Task 2: Issue Verifiable Credentials
- Task 3: Verify Credentials in Sample Application (Help Desk Scenario)

## Task 1: Configure Verified ID Issuer

In this task, you will set up Microsoft Entra Verified ID for your organization. You will register your DID, verify your domain, and create a verified credential that can be issued to users.

1. On the **Microsoft Entra admin center** left navigation pane, expand **Verified ID** and select **Overview (1)** then click on **Configure (2)**.

   ![](./Images/E3T1S1.png)

1. On the **Define organization settings** wizard, provide the below details and click on **Select keys (3)** for Key vault selection.

   - **Organization**: `VerifiesID` **(1)**.
   - **Trusted domain**: Open the **AzureCreds** file located on Desktop then copy and paste the endpoint **(2)**.

      ![](./Images/E3T1S2.png)

1. On the **Select keys from Azure Key Vault** wizard, leave the Subscription as **default (1)** then from the Key vault dropdown, select **kv-<inject key="DeploymentID" enableCopy="false" /> (2)** and click on **Select (3)**.

   ![](./Images/E3T1S3.png)

1. Click on **Save** to save the organization settings.

   ![](./Images/E3T1S4.png)

1. Now on the **Overview** page, Click on **Register** under the **Register decentralized ID**.

   ![](./Images/E3T1S5.png)

1. Click on **Download** to download a **did.json** file.

   ![](./Images/E3T1S6.png)

1. Open a new tab, paste the provided link and log in to the Azure portal using the below **credentials**.

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

1. Now, navigate back to **Microsoft Entra admin center**, and click on **Refresh registration status (1)**. Once the status is **Registered (2)**, click on **Close (3)**.

   ![](./Images/E3T1S15.png)

1. Now on the **Overview** page, Click on **Verify** under **Verify domain membership**.

   ![](./Images/E3T1S16.png)

1. Click on **Download** to download a did-configuration.json.

   ![](./Images/E3T1S17.png)

1. Navigate back to Azure portal, click on **Upload (1)** and select **Browse for files (2)**.

   ![](./Images/E3T1S18.png)

1. Click on **Downloads (1)** and select **did-configuration.json (2)** then click on **Open (3)**.

   ![](./Images/E3T1S19.png)

1. Click on **Upload** to upload the file to the directory.

   ![](./Images/E3T1S20.png)

1. Now, navigate back to **Microsoft Entra admin center**, and click on **Refresh registration status (1)**. Once the status is **Verified (2)**, click on **Close (3)**.

   ![](./Images/E3T1S21.png)

1. On the **Overview** page, now you can see the **Verified Domain**.

   ![](./Images/E3T1S22.png)

   >**Note**: if it is still showing **Domain may not be verified**, click on **Verify**, select **Refresh registration status**. If the registration is successful, click on **Close** and check again.
   >![](./Images/E3T1S22-1.png)

1. Now, click on **+ Create credential** to create a new credential type.

   ![](./Images/E3T1S23.png)

1. Select **Verified employee** in the Credentials type and then click on **Next**.

   ![](./Images/E3T1S24.png)

1. Provide below Information and click on **Update (4)** to verify the display card styling:

   - **Logo URL**: `https://experienceazure.blob.core.windows.net/modernidentitywithmicrosoftentra/VerifiedID.jpg` **(1)**
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

1. Navigate to  **Overview (1)** blade in **Verified ID** section, then click on **Try it now (2)** under **Get the new credentials**.

   ![](./Images/E3T2S3.png)

1. You will be redirected to the **MyAccount** portal. Scroll down and click on **Get your verified ID**

   ![](./Images/E3T2S4-2.png)

   > **Note**: If there is any popup wizard appears with **Feedback to Microsoft** click on **Skip**.

      ![](./Images/E3T2S4N.png)

1. A QR code is displayed **(1)**. Scan it from the Authenticator app on your mobile and **Add** the credentials to your Verified ID. Once it is completed click on **Done (2)**.

   ![](./Images/E3T2S5-1.png)

1. Now navigate back to **Microsoft Entra admin center**. On the **Overview** page of Verified ID, click on **Try it now** under **Use your credentials**.

   ![](./Images/E3T2S6.png)

1. It will redirect to a sample website to use your credentials. Click on **Access discounts**.

   ![](./Images/E3T2S7.png)

1. On the sign-in page, click on **Verify my Employee Credentials**.

   ![](./Images/E3T2S8.png)

1. A QR will be displayed. Scan it from the Authenticator App on your mobile and click on **Share**.

   ![](./Images/E3T2S9.png)

1. Once it is successful, you can see discounts are applied and the price of the laptops has been changed.

   ![](./Images/E3T2S10.png)

## Task 3: Account Recovery using Microsoft Entra

In this task, you will configure and validate the Microsoft Entra Account Recovery experience. This feature enables users to securely recover access to their accounts when they are unable to use their registered authentication methods.

1. Navigate back to the **Microsoft Entra Admin Center** portal, then select **Account Recovery** under **Microsoft Entra ID**. The **Getting started** page displays a setup checklist that guides you through the required configuration steps.

   ![](./Images/a1.png)

1. Click on **Set up passkeys after recovery** to configure passkey registration following a successful account recovery process.

   > **Note:** This configuration allows users to register a passkey after recovering their account, helping them use a stronger and more phishing-resistant authentication method in the future.

   ![](./Images/a2.png)

1. **Turn on (1)** the toggle and then navigate to the **Configure (2)** tab.

   ![](./Images/a3.png)

1. Ensure the **Allow self-service set-up (1)** checkbox is selected and click on **Save (2)** to allow users to register passkeys independently after account recovery.

   ![](./Images/a4.png)

1. Return to the **Account Recovery** page and click on **Select and subscribe for your first identity verification provider**.

   > **Note:** Account Recovery requires an identity verification provider to validate a user's identity before access can be restored. This helps prevent unauthorized account recovery attempts.

   ![](./Images/a5.png)

1. On the **Identity Verification Providers** page, click on **Get Solution** under **Identity Verification by 1Kosmos**.

   > **Note:** In this lab, 1Kosmos is used as the identity verification provider to perform document and identity validation during the recovery process.

   ![](./Images/a6.png)

1. A new browser tab opens. Click on **Get Solution** and provide the following details under the **Account Details** section:

   - **Billing Subscription**: Leave the default selection **(1)**
   - **Resource Group**: Select **ODL-Entra-<inject key="Deployment ID" enableCopy="false"></inject>-01 (2)**
   - **Resource Name**: accountrecovery<inject key="Deployment ID" enableCopy="false"></inject> **(3)**

   ![](./Images/a7.png)

1. Scroll down to the **Solution Details** section, click on **Choose Plan (1)**, and select **Pay As You Go with Free Trial**.

1. Select **1 Year Subscription (2)** as the billing term and **turn off Auto-renew (3)**.

   ![](./Images/a8.png)

1. Review the configuration and click **Next** twice to continue.

1. Click on **Place Order** to complete the subscription process with the identity verification provider.

   ![](./Images/a9.png)

1. Once the order is completed successfully, click on **Configure Account Now**.

   ![](./Images/a10.png)

1. If prompted to sign in, provide the following credentials:

   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
   - **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject>

1. On the **Permissions Required** page, review the requested permissions and click on **Accept**.

   ![](./Images/a11.png)

1. Once the setup is complete, navigate back to the **Security Store** page and click on **My Solutions**. Verify that both the **Subscription Status** and **Deployment Status** display **Succeeded**.

   ![](./Images/a12.png)

1. Navigate back to the **Microsoft Entra Admin Center**, select **Account Recovery (1)**, switch to the **Profile (2)** tab, and click **+ Add (3)** to create a new recovery profile.

   ![](./Images/a13.png)

1. On the **Profile Details** page, provide **Initial Profile Test (1)** as the profile name and click **Next (2)**.

   ![](./Images/a14.png)

1. On the **Recovery Mode** page, select **Evaluation (1)** and click **Next (2)**.

   > **Note:** Evaluation mode allows the account recovery workflow to be tested without enforcing recovery policies across the organization, making it ideal for validation and learning scenarios.

   ![](./Images/a15.png)

1. On the **User Group** page, select **All Users (1)** and click **Next (2)**.

   ![](./Images/a16.png)

1. On the **Identity Verification Provider (1)** page, select the provider subscribed earlier and click **Next (2)**.

   ![](./Images/a17.png)

1. On the **Account Validation** page, review the configured identity claim matching settings and click **Next**.

   ![](./Images/a18.png)

1. On the **Review and Finalize** page, verify the configuration details and click **Complete** to create the profile.

   ![](./Images/a19.png)

1. Open a **New InPrivate window**, navigate to the following URL, and sign in using the provided credentials.

   ```text
   https://portal.azure.com
   ```

   - **Username:** Paste the user principal name copied in **Exercise 1 - Task 1 - Step 15 (1)** and click **Next (2)**.

   ![](./Images/ETS1419.png)

1. On the password page, click **Sign in another way (1)** and then select **Recover your account (2)**.

   ![](./Images/a20.png)

   ![](./Images/a21.png)

1. A new browser tab opens and displays a QR code. Scan the QR code using a mobile device to begin the recovery process.

   > **Note:** The recovery workflow is completed on a mobile device because identity verification requires access to device capabilities such as camera and document scanning.

   > **Note:** An identity document, such as a passport or driver's license, is required to complete this account recovery demonstration.

   ![](./Images/a22.png)

1. On the mobile device, review the **Consent Required** page, select the checkbox, and click **Continue**.

   ![](./Images/mobile4.jpg)

1. On the **Verify Your Identity** page, select **Yes, I have Microsoft Authenticator**.

   ![](./Images/mobile3.jpg)

1. On the **Position the Document** page, click **Capture Document**.

   ![](./Images/mobile1.jpg)

1. Capture a supported identity document such as a passport or driver's license and review the captured images.

1. Click **Looks Good** to submit the captured identity document for verification. If prompted, complete the **Face Check** process by following the on-screen instructions and then click **Submit**. The verification process may return a failed result in this lab environment because the identity information on the submitted document does not match the details associated with the test user account.

   ![](./Images/mobile2.jpg)

1. This exercise is intended to demonstrate the Account Recovery workflow and user experience. For a successful validation scenario, create a user account with matching identity details and repeat the verification process.

## Summary

In this exercise, you configured Microsoft Entra Account Recovery by integrating an identity verification provider, creating an account recovery profile, and enabling users to securely recover access to their accounts. You also validated the account recovery workflow and learned how identity verification can be used to restore account access when users lose access to their authentication methods.

## Congratulations! You have successfully completed the lab.
