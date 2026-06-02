# Modern Identity Governance & Secure Access with Microsoft Entra

### Overall Estimated Duration: 4 hours

## Lab Scenario

You are part of the Cloud Identity team at Contoso, a growing organization that is facing challenges with **manual user onboarding and offboarding**, **excessive or outdated access permissions**, reliance on **traditional VPN** for internal resource access, and dependence on password-based authentication. These challenges have resulted in increased security risks, limited visibility into user access, and inefficient operational processes.

To solve this, Contoso plans to adopt **Microsoft Entra** to modernize its identity and access management by implementing **Identity Governance** for automated lifecycle management, enforcing Conditional Access policies for secure access control, and enabling centralized visibility through monitoring and access reviews.

To achieve this, you will configure dynamic groups and lifecycle workflows, deploy Global Secure Access to provide secure connectivity to private and internet resources without VPN, apply Conditional Access policies for enhanced security, and implement Verified ID to enable passwordless identity verification using decentralized digital credentials.

## Overview
In this hands-on lab, you will explore **Modern identity governance**, **secure access management**, and **decentralized identity solutions** using Microsoft Entra. You will configure Identity Governance features, implement secure access to private and internet resources with Global Secure Access, and enable passwordless identity verification using Verified ID.

Throughout the lab, you will configure Identity Governance capabilities, manage users and groups, implement Conditional Access policies, and automate identity lifecycle processes. You will also explore Global Secure Access to provide secure connectivity to private and internet resources, and work with Verified ID to enable passwordless and decentralized identity verification using verifiable credentials.

By completing this lab, you will gain a strong understanding of how Microsoft Entra helps organizations enhance security, simplify identity administration, and deliver secure access experiences for users and applications.

## Objective

After completing this lab, you will be able to:
- **Configure Identity Governance:** Implement Identity Governance features to manage access lifecycle, automate approvals, and enforce least-privilege access.
- **Manage Users and Groups:** Create and manage users, groups, and dynamic memberships using Microsoft Entra.
- **Automate Lifecycle Workflows:** Configure Lifecycle Workflows to automate onboarding, offboarding, and user access processes.
- **Implement Conditional Access and MFA:** Configure Conditional Access policies and Multi-Factor Authentication (MFA) to strengthen identity security.
- **Deploy Global Secure Access:** Configure Microsoft Entra Global Secure Access to securely connect users to private and internet resources without traditional VPN dependency.
- **Monitor and Review Access Activities:** Perform access reviews and monitor identity-related activities for improved visibility and governance.
- **Configure Microsoft Entra Verified ID:** Set up Verified ID to enable decentralized and passwordless identity verification.
- **Issue and Verify Verifiable Credentials:** Use Microsoft Authenticator to issue and verify verifiable credentials securely.
- **Understand Zero Trust Security:** Explore how Microsoft Entra services work together to support Zero Trust identity and access management across cloud and hybrid environments.

### Prerequisites

Before starting this lab, you should have:
- Basic understanding of Entra Identity fundamentals concepts.
- Familiarity with the Azure portal and Microsoft Entra admin center.
- Basic knowledge of authentication, authorization, and MFA concepts.
- Understanding of users, groups, and role-based access control (RBAC).
- Basic understanding of networking concepts such as private and internet access.
- General awareness of Zero Trust security principles.

## Architecture

The Modern Identity Governance & Secure Access architecture uses **Microsoft Entra** to provide centralized identity management, secure access control, and decentralized identity verification across cloud and hybrid environments.

**Microsoft Entra ID** manages identities, governance policies, and access controls using features such as **Lifecycle Workflows, Access Reviews, and Conditional Access**. Microsoft Entra Global Secure Access enables secure access to private and internet resources without traditional VPN solutions. **Microsoft Entra Verified ID** provides passwordless identity verification using verifiable credentials and Microsoft Authenticator.

Built-in **Monitoring** and **Auditing** capabilities help organizations track sign-in activity, access events, and governance operations across the environment.

## Architecture Diagram

![](./Images/Arch1.png)	

## Explanation of Components
The architecture for this lab involves several key components:

| Component | Description |
|---|---|
| **Microsoft Entra ID** | Central identity and access management platform. |
| **Dynamic Groups** | Automatically manage group memberships based on user attributes. |
| **Lifecycle Workflows** | Automate onboarding and offboarding processes. |
| **Access Reviews** | Periodically validate and review user access. |
| **Conditional Access** | Enforce security controls such as MFA and access policies. |
| **Global Secure Access** | Securely connect users to private and internet resources. |
| **Private Access** | Provides secure access to internal resources without VPN. |
| **Internet Access** | Applies secure web filtering and internet access controls. |
| **Global Secure Access Client** | Connects endpoint devices securely to resources. |
| **Private Access Connector** | Bridges private resources with Global Secure Access. |
| **Microsoft Entra Verified ID** | Enables decentralized identity verification using credentials. |
| **Microsoft Authenticator** | Stores and presents verifiable credentials securely. |
| **Help Desk Verification App** | Verifies user identity using Verified ID credentials. |
| **Monitoring and Auditing** | Tracks sign-ins, access events, and governance activities. |


# Getting Started with Lab

Welcome to the Modern Identity Governance & Secure Access with Microsoft Entra Workshop!. Let's begin by making the most of this experience:

## Accessing Your Lab Environment

Once you are ready to dive in, your virtual machine and guide will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](./Images/env.png)

## Lab Guide Zoom In/Zoom Out

To adjust the zoom level for the environment page, click the **A↕ : 100%** icon located next to the timer in the lab environment.

![](./Images/GS2.png)

## Virtual Machine & Guide
 
Your virtual machine is your workhorse throughout the workshop. The guide is your roadmap to success.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.
 
![Explore Lab Resources](./Images/envtab_newUI.png)
 
## Utilizing the Split Window Feature
 
For convenience, you can open the guide in a separate window by selecting the **Split Window** button from the top right corner.
 
![Use the Split Window Feature](./Images/GS4.png)
 
## Managing Your Virtual Machine
 
Feel free to **start, stop, or restart (2)** your virtual machine as needed from the **Resources (1)** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](./Images/GS5.png)	

## Lab Validation

After completing the task, hit the **Validate** button under the Validation tab integrated within your lab guide. If you receive a success message, you can proceed to the next task; if not, carefully read the error message and retry the step, following the instructions in the lab guide.

   ![Inline Validation](./Images/validation.png)
 
Now you're all set to explore the powerful world of technology. Feel free to reach out if you have any questions along the way. 

## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the Azure Portal icon as shown below:
 
    ![Launch Azure Portal](./Images/Azureportal.png)
 
2. You'll see the **Sign into Microsoft Azure** tab. Here, enter your credentials **(1)** and click **Next (2)**.
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
    ![Enter Your Username](./Images/GS6.png)
 
3. Next, provide your temporary password **(1)** and select **Sign in (2)**.
 
   - **Temporary Access Pass:** <inject key="AzureAdUserPassword"></inject>
 
      ![Enter Your Password](./Images/GS6-1.png)
 
4. If prompted to stay signed in, you can click **No**.

   ![](./Images/GS7.png)
 
   
Now you're all set to explore the powerful world of technology. Feel free to reach out if you have any questions along the way. Enjoy your workshop!

## Support Contact

The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

Learner Support Contacts:

* Email Support: cloudlabs-support@spektrasystems.com 
* Live Chat Support: https://cloudlabs.ai/labs-support

Now, click on Next from the lower right corner to move on to the next page.

   ![](./Images/Nextpage.png)

### Happy Learning!!
