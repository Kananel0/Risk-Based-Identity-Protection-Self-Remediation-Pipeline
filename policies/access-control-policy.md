# Access Control Policy — 15: Risk-Based Identity Protection & Self-Remediation Pipeline

**Classification:** Internal Use - Portfolio Demonstration
**Policy Owner:** Identity & Access Management (IAM) Engineering / Security Operations
**Effective Date:** 2026-08-24
**Review Cycle:** Annual
**Version:** 1.0

## 1. Purpose

This policy establishes the mandatory controls governing the architecture described in "Risk-Based Identity Protection & Self-Remediation Pipeline", in support of the compliance obligations listed in Section 6 and the risk position documented in the associated Compliance Mapping.

## 2. Scope

This policy applies to all personnel, systems, and third parties with access to the in-scope environment: Entra ID P2, Entra ID Identity Protection, Self-Service Password Reset. It applies regardless of employment classification (employee, contractor, or third party) where such access is technically possible.

## 3. Policy Statements

1. Any user reaching the defined user risk threshold shall be required to complete a password change before further access is granted; access shall not be silently permitted to continue at that risk level.
2. Any sign-in reaching the defined sign-in risk threshold shall require step-up multi-factor authentication before the session is established.
3. Registration of new authentication methods (security info) shall itself require multi-factor authentication, preventing a risky or compromised session from registering a new method as a persistence mechanism.
4. Self-service password reset shall be enabled and available to all users in scope of the risk policies, so remediation does not depend on administrator availability.
5. Any manual admin risk-escalation action (e.g., Confirm user compromised) shall be logged with the acting administrator's identity and shall be reviewed against actual justification during the next access certification cycle.

## 4. Roles & Responsibilities

| Role | Responsibility |
|---|---|
| Identity & Access Management (IAM) Engineering / Security Operations | Owns this policy, approves exceptions, and is accountable for control testing outcomes. |
| System/Application Owner | Ensures the in-scope system is configured in accordance with this policy at all times. |
| Requesting Individual | Complies with the access request and justification process defined in this policy. |
| Internal Audit / Compliance | Independently verifies control operating effectiveness per the testing procedure in `compliance/compliance-mapping.md`. |

## 5. Exceptions

Any deviation from the policy statements in Section 3 requires a documented, time-bound exception, approved in writing by Identity & Access Management (IAM) Engineering / Security Operations, with a defined expiry date and a compensating control where applicable. Exceptions shall be logged and reviewed at each policy review cycle.

## 6. Related Standards & Requirements

- ISO 27001 A.9.4.3 - Password management system
- ISO 27001 A.16.1 - Management of information security incidents
- POPIA Condition 7 - Security safeguards

## 7. Enforcement

Non-compliance with this policy is treated as a control deficiency and shall be logged, tracked, and remediated per the Non-Conformance Handling process defined in `compliance/compliance-mapping.md`. Repeated or willful non-compliance may result in access revocation pending review.

## 8. Document Control

| Version | Date | Author | Change Summary |
|---|---|---|---|
| 1.0 | 2026-08-24 | Identity & Access Management (IAM) Engineering / Security Operations | Initial approved version. |

---

[⬅ Back to project README](../README.md)
