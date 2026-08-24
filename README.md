# Risk-Based Identity Protection & Self-Remediation Pipeline

![Microsoft Entra](https://img.shields.io/badge/Microsoft%20Entra-ID%20P2-0078D4?style=for-the-badge\&logo=microsoft)
![Identity Protection](https://img.shields.io/badge/Identity-Protection-5E5E5E?style=for-the-badge)
![Conditional Access](https://img.shields.io/badge/Conditional-Access-107C10?style=for-the-badge)
![SC-300](https://img.shields.io/badge/SC--300-Lab-8A2BE2?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-%240-00C853?style=for-the-badge)

> An enterprise-inspired Microsoft Entra ID P2 lab demonstrating identity risk detection, risk-based Conditional Access, adaptive remediation, MFA enforcement, security-information protection, risk investigation, and audit verification.

---

## Project Overview

Identity Protection becomes valuable when detected risk leads to an automated security response.

This project builds and validates an end-to-end identity risk pipeline using **Microsoft Entra ID P2**.

The lab demonstrates how an organization can move from:

```text
Risk Detection
      ↓
Risk Evaluation
      ↓
Conditional Access
      ↓
MFA / Risk Remediation
      ↓
User Recovery
      ↓
Risk Verification
      ↓
Audit Evidence
```

Instead of simply configuring a risk policy, the project validates the complete lifecycle from **risk escalation to remediation and evidence collection**.

---

## Business Scenario

A user account is suspected of being compromised.

The security team needs to ensure that:

1. The identity can be classified as high risk.
2. High-risk users cannot simply continue accessing resources.
3. The legitimate user can prove their identity.
4. The account can be remediated without unnecessary administrator intervention.
5. The resulting activity is visible to security administrators.
6. The complete event can be investigated and audited.

The solution uses Microsoft Entra ID Protection and Conditional Access.

---

# Architecture

```text
                         Microsoft Entra ID
                                │
                                ▼
                  ┌─────────────────────────┐
                  │   Identity Protection   │
                  └────────────┬────────────┘
                               │
                 ┌─────────────┴─────────────┐
                 │                           │
                 ▼                           ▼
          ┌─────────────┐             ┌──────────────┐
          │  User Risk  │             │ Sign-In Risk │
          └──────┬──────┘             └──────┬───────┘
                 │                           │
                 └─────────────┬─────────────┘
                               ▼
                    ┌─────────────────────┐
                    │ Conditional Access  │
                    └──────────┬──────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
              ▼                ▼                ▼
             MFA       Risk Remediation       Block
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Secure Password     │
                    │ Change / Adaptive   │
                    │ Remediation         │
                    └──────────┬──────────┘
                               │
                               ▼
                       Risk Remediated
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Risk History        │
                    │ Risk Detections     │
                    │ Audit Logs           │
                    └─────────────────────┘
```

---

# Microsoft Entra Features Used

| Feature                           | Purpose                                    |
| --------------------------------- | ------------------------------------------ |
| Microsoft Entra ID P2             | Identity security platform                 |
| Identity Protection               | Identity and sign-in risk detection        |
| Risky Users                       | Investigate users with elevated risk       |
| Risky Sign-ins                    | Investigate risky authentication attempts  |
| Risk Detections                   | Investigate individual risk events         |
| Risk History                      | Track changes to user risk                 |
| Conditional Access                | Enforce adaptive access controls           |
| MFA                               | Strong authentication                      |
| Risk Remediation                  | Remediate high-risk identities             |
| Security Information Registration | Protect authentication-method registration |
| Audit Logs                        | Validate administrative activity           |

---

# Prerequisites

## Required

* Microsoft Entra ID tenant
* Microsoft Entra ID P2
* Administrative account
* Test user account
* Microsoft Authenticator or another supported MFA method
* Access to the Microsoft Entra admin center

## Recommended Lab Accounts

### Administrator

```text
Security Administrator
```

### Test User

```text
risk.test@<tenant>.onmicrosoft.com
```

### Test Group

```text
GRP-IAM-RiskProtection-Lab
```

> **Important:** Do not use your production Global Administrator account as the test user.

---

# Lab Objectives

By completing this project, the following objectives are achieved:

* [x] Verify Microsoft Entra Identity Protection
* [x] Create an isolated test group
* [x] Configure MFA for the test identity
* [x] Configure user-risk Conditional Access
* [x] Configure sign-in-risk Conditional Access
* [x] Protect security-information registration
* [x] Generate a controlled high-risk condition
* [x] Test risk remediation
* [x] Verify risk history
* [x] Investigate risk detections
* [x] Review audit logs
* [x] Document the complete identity-risk lifecycle

---

# Implementation

## 1. Verify Identity Protection

Navigate to:

**Microsoft Entra admin center → Protection → Identity Protection**

Confirm that the following areas are available:

* Overview
* Risky users
* Risky sign-ins
* Risk detections

### Evidence

```text
screenshots/
└── 01-identity-protection-dashboard.png
```

---

# 2. Create the Lab Security Group

Create:

```text
GRP-IAM-RiskProtection-Lab
```

Group type:

```text
Security
```

Add the test user.

The group isolates the lab policies from the rest of the tenant.

---

# 3. Configure MFA

Register Microsoft Authenticator for the test user.

Verify that the user can successfully complete MFA.

This is important because risk-based remediation can require strong authentication.

### Evidence

```text
screenshots/
└── 02-test-user-mfa.png
```

---

# 4. Configure User Risk Policy

Create a Conditional Access policy:

```text
CA-IAM-User-Risk-Remediation
```

### Users

```text
GRP-IAM-RiskProtection-Lab
```

### User Risk

```text
High
```

### Grant

Use:

```text
Require risk remediation
```

Where appropriate for the tenant's authentication configuration.

### Policy State

Start with:

```text
Report-only
```

Validate the policy before switching to:

```text
On
```

### Why?

User risk evaluates the likelihood that the **identity itself has been compromised**.

```text
User Risk = Risk associated with the identity
```

---

# 5. Configure Sign-In Risk

Create:

```text
CA-IAM-SignIn-Risk-MFA
```

### Users

```text
GRP-IAM-RiskProtection-Lab
```

### Sign-In Risk

```text
Medium
High
```

### Grant

```text
Require multifactor authentication
```

### Policy State

Initially:

```text
Report-only
```

After validation:

```text
On
```

Sign-in risk evaluates the risk associated with a **specific authentication event**.

```text
Sign-In Risk = Risk associated with the authentication attempt
```

---

# User Risk vs Sign-In Risk

This distinction is important for SC-300.

### User Risk

```text
"How likely is this identity to be compromised?"
```

Example:

```text
User Risk = High
```

Possible response:

```text
Require risk remediation
Block access
```

### Sign-In Risk

```text
"How risky is this particular authentication attempt?"
```

Example:

```text
Sign-In Risk = Medium
```

Possible response:

```text
Require MFA
```

---

# 6. Protect Security Information Registration

Create:

```text
CA-IAM-Protect-Security-Info-Registration
```

Navigate to:

**Conditional Access → Target resources → User actions**

Select:

```text
Register security information
```

### Grant

```text
Require multifactor authentication
```

### Why?

Security-information registration is itself a security-sensitive action.

Without protection, a compromised account could potentially be used to establish an attacker-controlled authentication method.

The intended security model is:

```text
Compromised Password
        ↓
Attacker Attempts Registration
        ↓
Conditional Access
        ↓
MFA Required
        ↓
Unauthorized Registration Blocked
```

---

# 7. Controlled Risk Escalation

For this lab, a deterministic risk condition is required.

Navigate to:

**Identity Protection → Risky users**

Select the test user and use:

```text
Confirm user compromised
```

This is an actual Microsoft Entra Identity Protection administrative action.

It is intentionally used instead of attempting to generate artificial malicious traffic.

The administrator is effectively telling Identity Protection:

> This account has been investigated and should be treated as compromised.

The user should then enter a high-risk state.

Expected detection:

```text
Admin confirmed user compromised
```

---

# Why Use "Confirm User Compromised"?

This lab does **not** attempt to simulate Microsoft's underlying machine-learning detection engine.

Instead, it uses a documented administrative risk-feedback capability to create a controlled test condition.

This allows the complete remediation pipeline to be tested without:

* VPNs
* Tor
* Credential attacks
* Malware
* External attack infrastructure
* Suspicious network tooling

The objective is to test:

```text
Risk
 ↓
Policy
 ↓
Remediation
 ↓
Verification
```

not to generate malicious traffic.

---

# 8. Test the Remediation Pipeline

Open a private/incognito browser session.

Sign in as the test user.

Expected flow:

```text
User Authentication
        ↓
Identity Protection Risk Evaluation
        ↓
Conditional Access
        ↓
Risk Remediation Required
        ↓
MFA / Secure Password Change
        ↓
Access Restored
```

The exact remediation experience depends on the configured policy and authentication method.

---

# 9. Verify Risk Remediation

Return to:

**Identity Protection → Risky users**

Locate the test user.

Review:

* Risk level
* Risk state
* Risk history
* Risk details

The expected lifecycle is:

```text
At Risk
   ↓
High Risk
   ↓
Remediation
   ↓
Remediated
```

---

# 10. Investigate Risk History

Open:

**Risky users → Test User → Risk history**

Look for evidence showing:

```text
Risk Raised
     ↓
High Risk
     ↓
Policy Enforcement
     ↓
User Remediation
     ↓
Risk Remediated
```

This provides the strongest evidence that the complete workflow worked.

---

# 11. Investigate Risk Detections

Navigate to:

**Identity Protection → Risk detections**

Locate the test user's detection.

Expected detection:

```text
Admin confirmed user compromised
```

Review:

* Detection type
* Risk level
* User
* Timestamp
* Detection state
* Additional information

---

# 12. Review Audit Logs

Navigate to:

**Entra ID → Monitoring & health → Audit logs**

Filter for the test user.

Review events related to:

* Administrative risk actions
* Password activity
* Authentication changes
* Security information
* User management

The objective is to demonstrate that the identity-security workflow produces an auditable trail.

---

# Testing Matrix

| Test                          | Expected Result                | Status |
| ----------------------------- | ------------------------------ | ------ |
| Identity Protection available | Dashboard loads                | ☐      |
| Test group created            | Group exists                   | ☐      |
| Test user configured          | User exists                    | ☐      |
| MFA configured                | Authentication succeeds        | ☐      |
| User risk policy              | Policy configured              | ☐      |
| Sign-in risk policy           | Policy configured              | ☐      |
| Security registration policy  | User action protected          | ☐      |
| Compromise confirmed          | User becomes high risk         | ☐      |
| Risk detection                | Detection appears              | ☐      |
| Conditional Access            | Policy evaluates               | ☐      |
| Remediation                   | User completes required action | ☐      |
| Risk history                  | Remediation visible            | ☐      |
| Risk state                    | Remediated                     | ☐      |
| Audit logs                    | Administrative evidence exists | ☐      |

---

# Evidence

The project should contain screenshots demonstrating the complete workflow.

```text
screenshots/
│
├── 01-identity-protection-dashboard.png
├── 02-test-user-mfa.png
├── 03-user-risk-policy.png
├── 04-signin-risk-policy.png
├── 05-security-info-registration-policy.png
├── 06-confirm-user-compromised.png
├── 07-high-risk-user.png
├── 08-risk-remediation.png
├── 09-risk-history.png
├── 10-risk-detection.png
└── 11-audit-logs.png
```

### Most Important Evidence

If only three screenshots are available, prioritize:

**1. High-risk user**

```text
06-confirm-user-compromised.png
07-high-risk-user.png
```

**2. Remediation**

```text
08-risk-remediation.png
```

**3. Risk history**

```text
09-risk-history.png
```

The risk-history screenshot is particularly valuable because it demonstrates the lifecycle rather than just a configuration screen.

---

# Security Design Decisions

## Dedicated Lab Group

Policies target:

```text
GRP-IAM-RiskProtection-Lab
```

rather than all users.

This reduces the blast radius.

---

## Break-Glass Exclusion

Emergency access accounts should be excluded from experimental Conditional Access policies.

```text
All Users
   |
   +---- Lab Users → Policy
   |
   +---- Break-Glass → Excluded
```

---

## Report-Only First

Policies should initially be deployed as:

```text
Report-only
```

After validation:

```text
Report-only
      ↓
Validate
      ↓
On
```

This reduces the chance of accidentally locking out users.

---

# SSPR vs Risk Remediation

These features should not be treated as identical.

### SSPR

```text
User cannot use password
        ↓
Identity verification
        ↓
Password reset
```

### Risk Remediation

```text
Identity becomes high risk
        ↓
Conditional Access
        ↓
Strong authentication
        ↓
Secure password change /
adaptive remediation
        ↓
Risk remediated
```

SSPR can also contribute to risk remediation in supported scenarios, but it is not the same feature as the risk-based Conditional Access remediation workflow.

---

# Zero Trust Alignment

This project maps directly to the three core Zero Trust principles.

## Verify Explicitly

```text
Identity
+
MFA
+
User Risk
+
Sign-In Risk
```

## Use Least Privilege

```text
Dedicated administrative roles
+
Scoped test groups
```

## Assume Breach

```text
Compromised Identity
        ↓
Risk Detection
        ↓
Conditional Access
        ↓
Remediation
```

---

# SC-300 Skills Demonstrated

This project reinforces:

* Microsoft Entra ID Protection
* Identity Protection
* User risk
* Sign-in risk
* Risk detections
* Risky users
* Risky sign-ins
* Conditional Access
* MFA
* Authentication methods
* Security information registration
* Risk remediation
* Audit logs
* Least privilege
* Zero Trust
* Identity incident response

---

# Interview Talking Points

### What did you build?

> I built an end-to-end Microsoft Entra ID P2 identity-risk remediation pipeline. I configured user-risk and sign-in-risk Conditional Access policies, protected security-information registration, created a controlled high-risk condition using the Confirm User Compromised administrative capability, and validated that the identity could be remediated and the resulting activity investigated through risk history and audit logs.

### What is the difference between user risk and sign-in risk?

> User risk represents the likelihood that an identity itself has been compromised, while sign-in risk represents the likelihood that a specific authentication attempt is risky. I can therefore apply different controls to each signal.

### Why did you use Confirm User Compromised?

> I wanted a deterministic and controlled way to test the remediation pipeline without generating artificial malicious traffic. Confirm User Compromised is an actual Identity Protection administrative risk-feedback capability, so it allowed me to validate the downstream controls without relying on VPNs, Tor, or attack tooling.

### Why protect security-information registration?

> Because authentication-method registration is security-sensitive. If an attacker compromises a user's credentials, they may attempt to register another authentication method and establish persistence. Conditional Access can require MFA when users attempt to register security information.

---

# Lessons Learned

### 1. Risk detection is only the beginning

A security team needs an enforcement and remediation strategy after detecting risk.

### 2. User risk and sign-in risk are different

The two signals represent different security questions and can require different responses.

### 3. Conditional Access is the enforcement layer

Identity Protection identifies risk while Conditional Access determines what happens next.

### 4. MFA is part of identity remediation

Strong authentication can allow legitimate users to prove their identity and remediate certain risk conditions.

### 5. Security-information registration must be protected

Authentication-method registration can become an attacker persistence mechanism if poorly protected.

### 6. Evidence matters

A working security configuration is useful.

A working security configuration with:

```text
Risk Detection
+
Policy Evaluation
+
Remediation
+
Risk History
+
Audit Logs
```

is much stronger evidence.

---

# Limitations

This project uses:

```text
Confirm user compromised
```

as a controlled administrative risk trigger.

It does **not** claim to reproduce Microsoft's machine-learning risk-detection engine.

The purpose is to validate the complete response pipeline:

```text
Risk
 ↓
Conditional Access
 ↓
Remediation
 ↓
Verification
 ↓
Audit
```

This makes the lab deterministic, repeatable, and safe.

---

# Final Result

The completed project demonstrates:

```text
                 IDENTITY RISK
                      │
                      ▼
               RISK DETECTION
                      │
                      ▼
              RISK CLASSIFICATION
                      │
                      ▼
             CONDITIONAL ACCESS
                      │
                      ▼
            MFA / REMEDIATION
                      │
                      ▼
                USER RECOVERY
                      │
                      ▼
              RISK REMEDIATED
                      │
                      ▼
             AUDIT & EVIDENCE
```

The result is an enterprise-inspired demonstration of how **Microsoft Entra ID P2 can transform identity risk signals into enforceable security controls and measurable remediation outcomes.**

---

# Microsoft Documentation

* [Microsoft Entra ID Protection](https://learn.microsoft.com/en-us/entra/id-protection/)
* [Remediate risks and unblock users](https://learn.microsoft.com/en-us/entra/id-protection/howto-identity-protection-remediate-unblock)
* [Configure risk policies](https://learn.microsoft.com/en-us/entra/id-protection/howto-identity-protection-configure-risk-policies)
* [Identity Protection risk feedback](https://learn.microsoft.com/en-us/entra/id-protection/howto-identity-protection-risk-feedback)
* [Conditional Access security information registration](https://learn.microsoft.com/en-us/entra/identity/conditional-access/policy-all-users-security-info-registration)

---

# Project Status

```text
Status: Completed / In Progress
Platform: Microsoft Entra ID
License: Microsoft Entra ID P2
Environment: Lab Tenant
Cost: $0
Focus: Identity Security / IAM / Zero Trust
Certification Alignment: SC-300
```

---

## Author

**Kananelo Mohale**

IAM / Cloud Security Portfolio

Focus areas:

```text
Microsoft Entra ID
Identity & Access Management
Conditional Access
Identity Protection
Privileged Identity Management
Zero Trust
Cloud Security
```
