# secure-multi-tenant-api-sast

A backend API reference project demonstrating how to prevent and automatically detect common OWASP API security failures in a multi-tenant SaaS-style service.

## Purpose

This repository simulates a realistic backend service used by multiple customer organizations (“tenants”) sharing a single API and datastore. It demonstrates how an embedded Application Security engineer can:

- Identify critical API security risks
- Guide secure implementation patterns
- Integrate SAST into CI/CD workflows
- Reduce regressions through automation

The focus of this first iteration is **SAST-driven detection** of:

- Broken Object Level Authorization (BOLA / IDOR)
- Security Misconfiguration (excessive error detail)

A future iteration will demonstrate complementary DAST integration.

---

## Repository Structure

secure-multi-tenant-api-sast/
- vulnerable/ — intentionally insecure implementation
- fixed/ — remediated implementation
- detected/ — Semgrep configuration and CI integration
- docs/ — threat model, attack walkthroughs, remediation guidance
- .github/workflows/ — SAST pipeline
- scripts/ — local scan documentation

---

## Security Focus (v1)

### 1. Broken Object Level Authorization (BOLA)

Demonstrates improper tenant isolation where object access is not constrained by caller context.

### 2. Security Misconfiguration

Demonstrates excessive error detail exposure and improper production-safe error handling.

---

## Philosophy

This project is not a vulnerability playground.

It models:

- Realistic backend API patterns
- Developer-friendly remediation guidance
- Practical security automation integration
- Clear separation of vulnerable vs fixed implementations

The goal is to demonstrate senior-level AppSec thinking:
security that enables product teams while preventing regression.

---

## Status

Initial scaffold. Implementation in progress.
