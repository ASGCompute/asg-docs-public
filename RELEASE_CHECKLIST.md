# Docs Release Checklist

Use this checklist before merging any documentation changes to `main`.

## Pre-Merge Checklist

### Content Quality

- [ ] Spelling and grammar checked
- [ ] Code examples tested and working
- [ ] Links validated (internal and external)
- [ ] Screenshots up-to-date (if applicable)

### API Changes

- [ ] OpenAPI spec (`openapi/public.openapi.yaml`) updated if API changed
- [ ] Error codes documented in `/errors` section
- [ ] Breaking changes clearly marked with warnings

### Security & Compliance

- [ ] White-label scan passed (no vendor leaks)
- [ ] Secret scan passed (no keys, tokens, internal URLs)
- [ ] No internal architecture details exposed
- [ ] No internal URLs or IP addresses

### Navigation & Discovery

- [ ] New pages added to `mint.json` navigation
- [ ] Page has appropriate frontmatter (title, description)
- [ ] Canonical URLs correct

### Changelog

- [ ] `CHANGELOG.md` updated for user-facing changes
- [ ] Version bump if significant changes

## Post-Merge Verification

- [ ] Mintlify build succeeded
- [ ] Pages render correctly on production
- [ ] No 404s on new/moved pages
- [ ] Search index includes new content (~5 min delay)

---

## Quick Commands

```bash
# Run all checks locally
npm run lint && npm run check:links && npm run check:leaks

# Preview locally
npm run dev

# Validate OpenAPI
npm run lint:openapi
```

---

## Escalation

If a security issue is found in production docs:

1. **Immediately** remove sensitive content via direct push (bypass PR)
2. Notify security team in #security-incidents
3. Create post-mortem issue
