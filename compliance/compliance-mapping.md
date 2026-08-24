# Compliance Mapping — 15: Risk-Based Identity Protection & Self-Remediation Pipeline

**Classification:** Internal Use - Portfolio Demonstration
**Control Owner:** Identity & Access Management (IAM) Engineering / Security Operations
**Testing Frequency:** Annual (or per the cadence stated in the governing policy, where more frequent)

## Control Objectives

| Requirement | Description |
|---|---|
| ISO 27001 A.9.4.3 | Password management system |
| ISO 27001 A.16.1 | Management of information security incidents |
| POPIA Condition 7 | Security safeguards |

## Implementation Narrative

- Enabled Self-Service Password Reset (SSPR) as the mechanism that allows a risky user to remediate their own account without requiring a manual admin action.
- Configured a User Risk Policy requiring a forced password change for any user reaching Low risk or above, and a Sign-in Risk Policy requiring step-up MFA for Medium risk or above sign-ins.
- Configured a Conditional Access policy that specifically gates the security-info registration action behind MFA, closing the loophole where a risky user could register new authentication methods without first proving an existing one.
- Verified the full loop end-to-end using Identity Protection's built-in admin escalation action, confirming risk state automatically transitions to Remediated once the required self-service action is completed - with the full timeline and both automated and manual actions independently visible in the audit trail.

## Control Testing Procedure

The following steps constitute the minimum testing procedure to be performed by the control owner, or by internal/external audit, to assess operating effectiveness:

1. Use the Identity Protection admin action to escalate a test user's risk state and confirm the corresponding risk policy enforces its configured control (forced password change) on next sign-in.
2. Complete the required remediation as the test user and confirm the user's risk state transitions to Remediated automatically, without a manual admin clearance step.
3. Attempt to register a new authentication method as a user who has not yet completed MFA and confirm the Conditional Access policy blocks the registration attempt until MFA is satisfied.
4. Review the Risk history timeline for the test user and confirm every stage (risk raised, policy enforced, remediation completed, risk cleared) is present with accurate timestamps.

## Evidence Artifacts

The following evidence shall be retained and made available on request to support control testing:

- SSPR configuration scoped to the in-scope user group
- User risk policy and sign-in risk policy configuration exports
- Conditional Access policy targeting the security-info registration user action
- Risk history timeline for a test user showing the full detect-to-remediate cycle
- Audit log entries distinguishing the automated system remediation action from the manual admin risk-confirmation action

## Risk Assessment

**Inherent risk:** Identity risk detections (compromised credentials, anomalous sign-in patterns) without an enforced, automatic response path allow a flagged account to retain full access indefinitely, dependent entirely on a human noticing the detection and acting on it manually.

**Residual risk (control in place):** Low. Both user risk and sign-in risk now trigger an automatic, proportionate control (forced password change or step-up MFA) without requiring admin intervention, and risk state provably clears once remediation completes. Residual risk relates to detection quality itself (false negatives in Microsoft's risk engine) which is outside the scope of this control and governed by Microsoft's own detection capability, not by policy configuration.

## Non-Conformance Handling

Any control testing result that fails one or more steps above shall be logged as a non-conformance, assigned to Identity & Access Management (IAM) Engineering / Security Operations, and remediated on a timeline commensurate with the residual risk rating. Non-conformances open longer than thirty (30) days shall be escalated per the organization's risk acceptance process.

---

[⬅ Back to project README](../README.md)
