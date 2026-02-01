# Documentation Governance

> **Owner:** CTO / Engineering Lead  
> **Last Updated:** 2026-02-01

## Ownership

| Path | Owner | Reviewers |
|------|-------|-----------|
| `openapi/*` | @security @cto | Required |
| `security/*` | @security | Required |
| `billing/*` | @cto @finance | Required |
| `*` | @docs-team | Optional |

See [CODEOWNERS](.github/CODEOWNERS) for enforcement.

---

## Weekly Improvement Routine

**Cadence:** Every week, 20 minutes

1. **5 min** — Review analytics: top searches + zero-result queries
2. **10 min** — Pick 3 quick fixes:
   - Add FAQ block
   - Add code example
   - Add "next steps" link
3. **5 min** — Create PR, run CI, merge

**PR naming:** `docs: weekly improvements (YYYY-MM-DD)`

---

## OpenAPI Updates

Single source of truth: [`openapi/public.openapi.yaml`](openapi/public.openapi.yaml)

**Update process:**

1. Edit OpenAPI spec
2. Run `npm run validate:openapi`
3. Update changelog: `guide/changelog.mdx`
4. PR with prefix: `api: <description>`

---

## Error Code Updates

All public error codes in [`resources/errors.mdx`](resources/errors.mdx).

**When adding new errors:**

1. Add to gateway error handler
2. Add to docs with description + resolution
3. Ensure code examples show error handling

---

## Incident Response

See [SECURITY.md](SECURITY.md) for vulnerability disclosure.

**Docs incident process:**

1. Identify leaked content in [#docs-alerts]
2. Immediate: revert PR or emergency commit
3. Post-incident: update banlist `.leak-banlist.txt`

---

## Partner Portal Access

Partner docs require NDA. Access managed via:

- **MVP:** Password protection per partner
- **Target:** SSO via IdP with `partners` group

Audit: All access logged at CDN level.
