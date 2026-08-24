# Risk-Based Identity Protection & Self-Remediation Pipeline

**Classification:** Internal Use - Portfolio Demonstration
**Last Reviewed:** 2026-08-24

A single project demonstrating a complete detect-to-remediate identity risk pipeline, built entirely on Microsoft Entra ID P2 — no Entra ID Governance add-on, no Defender, no Intune, no infrastructure beyond the identity platform itself.

## What This Demonstrates

- **Risk Detection** — a flagged user (via Identity Protection's real detection engine, or the built-in admin escalation action used for controlled testing)
- **Risk Policy Enforcement** — User Risk and Sign-in Risk policies gate access automatically based on the detected risk level
- **Self-Remediation** — the user clears their own risk through a forced password reset via Self-Service Password Reset, with no administrator action required
- **Risk Clearance & Audit** — risk state transitions to Remediated automatically once the required action completes, with the full timeline and the distinct system vs. admin actions both independently visible in the audit trail

## Why This One Matters

Most Conditional Access and risk-policy demonstrations stop at "here's the policy that blocks access." This project goes one step further and proves the *response* actually closes the loop — a risky user isn't just denied, they're walked through a defined remediation path, and the system verifiably confirms that path was completed before restoring trust.

## Contents

- [`15-identity-protection-self-remediation/`](./15-identity-protection-self-remediation/) — full documentation set: README, architecture diagram, ADR, compliance mapping, formal access-control policy, and an evidence capture checklist.

## Disclaimer

This is a personal lab project built on a Microsoft Entra ID P2 tenant. It is not a record of any employer's environment.
