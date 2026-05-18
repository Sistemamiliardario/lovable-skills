# Supabase RLS Security Skill

## Role
You are the Supabase security guardrail for SistemaMiliardario.AI.

Use this skill whenever a request touches:

- Supabase tables
- RLS policies
- auth
- Edge Functions
- migrations
- admin dashboards
- user data
- audit logs
- financial or strategic profile data

## Non-negotiable rules

```txt
RLS must be enabled for user-facing tables.
No service role key in frontend code.
No secrets in client bundles.
No private user data in logs.
No broad public SELECT policies unless intentionally public.
No always-true WITH CHECK policies for user-owned data.
No destructive migrations without explicit request and rollback plan.
```

## RLS policy principles
Policies must be:

- least privilege
- role-aware
- user-owned where applicable
- admin-only where applicable
- explicit about INSERT, SELECT, UPDATE, DELETE
- compatible with authenticated and anonymous access boundaries

## Admin-only patterns
Admin features must verify authorization server-side or through trusted policy logic.

Do not rely only on hidden UI.

Admin screens should never expose:

- private financial data without purpose
- raw prompt logs with personal data
- secrets
- unrestricted user profiles

## Migration rules
Migrations should be:

- idempotent where possible
- reversible or low-risk
- named clearly
- scoped to one purpose
- safe to run once in production

Prefer:

```sql
CREATE TABLE IF NOT EXISTS
CREATE INDEX IF NOT EXISTS
CREATE OR REPLACE VIEW
```

Use caution with:

```sql
DROP
DELETE
TRUNCATE
ALTER COLUMN TYPE
SECURITY DEFINER
```

## SECURITY DEFINER rules
Use `SECURITY DEFINER` only when necessary.

If used:

```sql
SET search_path = public
```

or another explicit safe schema path must be set.

## Edge Function rules
Edge Functions must:

- validate authentication where needed
- validate input
- apply CORS deliberately
- avoid logging sensitive data
- return safe errors
- avoid unbounded runtime
- avoid uncontrolled external calls

## Data sensitivity
Treat these as sensitive:

- emails
- personal profiles
- quiz answers
- financial data
- business strategy details
- AI conversations
- API responses containing personal data
- customer uploaded files

## Review checklist
Before finalizing any Supabase-related change:

```txt
Is RLS enabled?
Can users only access their own data?
Are admin actions protected?
Are logs safe?
Are migrations safe?
Are secrets kept server-side?
Is there an audit trail where needed?
```
