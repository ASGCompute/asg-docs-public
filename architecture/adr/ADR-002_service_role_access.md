# ADR-002: Database Access Pattern — Service Role with RLS Enabled

**Status:** ACCEPTED  
**Date:** 2026-02-02  
**Author:** ASG Agent Cloud Team

---

## Context

The Supabase Security Advisor flagged **21 tables** without Row-Level Security (RLS) policies enabled. This raised concerns about data access controls.

However, our architecture uses a **service-to-database** pattern:

- All database access flows through trusted backend services (Gateway, Billing Plane, Control Plane)
- No direct client-to-database connections exist
- Public/Anonymous keys are **never exposed** to clients

## Decision

We adopt a **"RLS Enabled + Service Role Bypass"** pattern:

### 1. RLS is ENABLED on all tables

- This provides defense-in-depth
- Any accidental use of `anon` key returns **zero rows**

### 2. Service Role is used for API operations

- All backend services use `service_role` key (stored as secrets)
- Service Role **bypasses RLS** for performance (expected Postgres behavior)
- Application-layer AuthZ (middleware, scopes, budget guards) controls access

### 3. Anon Key is effectively disabled

- No routes or SDK calls use the `anon` key
- `anon` access is blocked by RLS policies (even if enabled)

## Evidence Required

To validate this ADR, the following must be verified and documented:

| Check | Command / Action | Expected Result |
|-------|------------------|-----------------|
| RLS Enabled | `SELECT tablename, rowsecurity FROM pg_tables WHERE schemaname='public'` | All tables show `rowsecurity = true` |
| Anon Blocked | Query any table using `anon` key | 0 rows returned OR permission denied |
| Secrets Secure | Run Gitleaks / Secrets Scan | No `service_role` key in source |

## Consequences

### Positive

- **Performance:** No RLS policy evaluation overhead for API calls
- **Security:** Defense-in-depth via RLS for accidental anon access
- **Simplicity:** No complex RLS policy maintenance per table

### Negative

- **Audit Warnings:** Supabase Advisor will still show "missing policies" (expected)
- **Operational Discipline:** All new services must use `service_role` key correctly

## Alternatives Considered

1. **Full RLS with per-table policies:** Rejected due to maintenance complexity and performance overhead for our server-only architecture
2. **Disable RLS entirely:** Rejected due to loss of defense-in-depth

## References

- [Supabase RLS Documentation](https://supabase.com/docs/guides/auth/row-level-security)
- ADR-001: Database Security Posture (if exists)
