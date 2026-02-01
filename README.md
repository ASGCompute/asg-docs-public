# ASG Agent Cloud - Public Documentation

> **Live Site:** <https://docs.asgcompute.com>

## Quick Start

```bash
# Install dependencies
npm install

# Start local dev server
npm run dev

# Run all CI checks locally
npm run lint && npm run check:links && npm run check:leaks
```

## Making Changes

1. Create a branch from `main`
2. Make your changes
3. Run local checks: `npm run lint && npm run check:links && npm run check:leaks`
4. Open a PR
5. Wait for CI to pass + required reviews
6. Merge to `main` → auto-deploys to Mintlify

## Required Reviews

See [CODEOWNERS](.github/CODEOWNERS) for path-specific requirements.

**Always required for:**

- OpenAPI spec changes
- Security documentation
- Billing documentation
- CI/CD workflow changes

## Release Checklist

Before merging, complete the [Release Checklist](RELEASE_CHECKLIST.md).

## Structure

```
docs/
├── mint.json              # Mintlify configuration
├── openapi/               # OpenAPI specifications
│   └── public.openapi.yaml
├── quickstart/            # Getting started guides
├── concepts/              # Core concepts
├── auth/                  # Authentication docs
├── billing/               # Payments & receipts
├── api/                   # API reference
├── errors/                # Error handling
├── security/              # Security best practices
└── .github/
    ├── CODEOWNERS
    └── workflows/
        ├── docs-ci.yml
        ├── white-label-scan.yml
        └── secret-scan.yml
```

## Support

- **Internal:** #docs-eng Slack channel
- **Issues:** Open a GitHub issue in this repo
