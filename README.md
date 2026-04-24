# Hands-On Lab: Modern Identity Governance & Secure Access with Microsoft Entra

## Lab Overview

This hands-on lab provides practical experience with Microsoft Entra's modern identity governance and secure access capabilities. Through three focused exercises, you will configure and validate enterprise-grade identity governance, Zero Trust network access, and decentralized identity verification.

**Total Estimated Duration: ~3.5 hours**

---

## Prerequisites

Before starting the lab, ensure you have:

- An active **Microsoft Azure subscription**
- A **Microsoft Entra ID** tenant (Azure Active Directory)
- A user account with **Global Administrator** role
- **Microsoft Entra ID P2** license or **Microsoft Entra Suite** license
- A **Windows 10/11 device** (Microsoft Entra joined) for Exercise 2
- A **smartphone** with **Microsoft Authenticator** app installed for Exercise 3

---

## Lab Exercises

### [Exercise 1: Microsoft Entra ID Governance (IGA)](Instructions/Exercise1.md)

**Estimated Duration: 90 minutes**

Learn how to manage identities throughout their lifecycle using Microsoft Entra ID Governance. Configure dynamic groups, automate onboarding and offboarding workflows, set up access reviews, and implement Conditional Access policies.

| Task | Topic | Duration |
|------|-------|----------|
| Task 1 | Manage Users and Groups (Dynamic Groups) | 15 min |
| Task 2 | Entra ID Lifecycle Management | 60 min |
| Task 2.1 | Configure Onboarding Workflow | 15 min |
| Task 2.2 | Configure Provisioning and License Handling | 15 min |
| Task 2.3 | Implement Custom Extensions | 15 min |
| Task 2.4 | Configure Offboarding Workflow | 15 min |
| Task 3 | Setting Up Access Reviews | 20 min |
| Task 4 | Implementing Conditional Access Policies | 10 min |
| Task 5 | Monitoring and Auditing (Optional/Read Only) | 5 min |

---

### [Exercise 2: Microsoft Entra Global Secure Access](Instructions/Exercise2.md)

**Estimated Duration: 60 minutes**

Configure Microsoft's Security Service Edge (SSE) solution for Zero Trust network access. Deploy Private Access connectors, configure Internet Access with web content filtering, and apply Conditional Access to network traffic.

| Task | Topic | Duration |
|------|-------|----------|
| Task 1 | Enable Microsoft Entra Global Secure Access | 10 min |
| Task 2 | Configure Entra Private Access | 20 min |
| Task 3 | Apply Conditional Access to Private Access | 10 min |
| Task 4 | Configure Entra Internet Access | 10 min |
| Task 5 | Validate Access and Review Logs | 10 min |

---

### [Exercise 3: Microsoft Entra Verified ID](Instructions/Exercise3.md)

**Estimated Duration: 60 minutes**

Implement decentralized identity verification using Microsoft Entra Verified ID. Configure an issuer, create credential types, issue credentials to a mobile wallet, and verify credentials in a simulated help desk scenario.

| Task | Topic | Duration |
|------|-------|----------|
| Task 1 | Configure Verified ID Issuer | 20 min |
| Task 2 | Issue Verifiable Credentials | 20 min |
| Task 3 | Verify Credentials in Sample Application | 20 min |

---

## Repository Structure

```
├── README.md                          # Lab overview and navigation (this file)
└── Instructions/
    ├── Exercise1.md                   # Exercise 1: Entra ID Governance (IGA)
    ├── Exercise2.md                   # Exercise 2: Global Secure Access
    ├── Exercise3.md                   # Exercise 3: Verified ID
    └── Images/                        # All screenshots and diagrams
```

---

## Key Technologies Covered

| Technology | Description |
|-----------|-------------|
| **Microsoft Entra ID Governance** | Lifecycle Workflows, Access Reviews, Entitlement Management |
| **Entra Lifecycle Workflows** | Automated joiner/mover/leaver workflows with custom extensions |
| **Microsoft Entra Global Secure Access** | Security Service Edge with Private and Internet Access |
| **Entra Private Access** | Zero Trust replacement for traditional VPN |
| **Entra Internet Access** | Secure Web Gateway with web content filtering |
| **Microsoft Entra Verified ID** | Decentralized identity with verifiable credentials |
| **Conditional Access** | Identity-based access control policies |
| **Azure Logic Apps** | Custom workflow automation and extensions |

---

## Additional Resources

- [Microsoft Entra Documentation](https://learn.microsoft.com/en-us/entra/)
- [Microsoft Entra ID Governance](https://learn.microsoft.com/en-us/entra/id-governance/)
- [Microsoft Entra Global Secure Access](https://learn.microsoft.com/en-us/entra/global-secure-access/)
- [Microsoft Entra Verified ID](https://learn.microsoft.com/en-us/entra/verified-id/)
- [Microsoft Security Documentation](https://learn.microsoft.com/en-us/security/)
