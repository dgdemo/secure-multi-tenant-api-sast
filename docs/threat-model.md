# Threat Model (v1)

## Scope
- Service: secure-multi-tenant-api-sast
- Primary assets:
    - Tenant-scoped project data (projects)
    - Tenant identity context (tenantId, userId, role claims in JWT)
    - Service availability (basic uptime)
    - Logs (since errors/logs can leak info)
- Out of scope:
    - Real identity provider / OAuth/OIDC flows
    - Production infrastructure hardening (WAF, rate limiting, network controls)
    - Advanced detection/monitoring/SIEM
    - DAST (this repo is SAST iteration)

## Assumptions
- The API is internet-accessible and untrusted clients can send arbitrary requests
- JWTs are assumed to be cryptographically valid (not modeling signature bypass)
- The service runs in a trusted internal environment for this demo.
- The datastore is shared across tenants.
- Developers may accidentally introduce unsafe object lookups or verbose error handling

## Architecture (high level)
- Actors:
    - External client (tenant user)
    - API service
    - Datastore
    - Internal operator (admin/debug context)
- Trust boundaries:
    - Between external client and API service
    - Between API service and datastore
- Data store:
    - Shared multi-tenant database (logical isolation via tenantId)

## Key security objectives
- Tenant isolation:
- Authentication:
- Authorization:
- Error handling:

## Threats (prioritized)
### T1: Broken Object Level Authorization (BOLA) / Insecure Direct Object Reference (IDOR) - (cross-tenant data access)
- What could go wrong:
    - An authenticated user accesses a project belonging to another tenant by supplying a valid projectId that is not scoped to their tenantId.
- Impact:
    - Cross-tenant data exposure, loss of confidentiality, potential compliance violations.
- Mitigations:
    - Enforce tenant scoping in all object lookups
    - Use (**projectId** AND **tenantId**) in data access patterns
    - Add automated SAST checks to detect unsafe lookup patterns

### T2: Excessive error detail (information disclosure)
- What could go wrong:
    - The API returns stack traces or detailed exception messages to clients, exposing internal implementation details.
- Impact:
    - Information disclosure that could assist attackers in crafting targeted exploits or understanding system internals.
- Mitigations:
    - Standardize error responses with generic client-safe messages.
    - Log detailed errors server-side only.
    - Disable debug modes in production configurations.

## Security requirements (v1)
- All object access must be tenant-scoped
- No stack traces or internal errors returned to clients.
- SAST must run on every pull request.
- Vulnerable patterns must fail CI once gating is enabled

## Open questions / follow-ups
-