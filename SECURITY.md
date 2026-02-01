# Security Policy

## Reporting Security Vulnerabilities

We take security seriously at ASG. If you discover a security vulnerability, please report it responsibly.

### How to Report

**Email:** <security@asgcompute.com>

**Please include:**

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Your contact information

### What to Expect

| Timeline | Action |
|:---------|:-------|
| **24 hours** | Acknowledgment of your report |
| **72 hours** | Initial assessment and triage |
| **7 days** | Status update with remediation plan |
| **90 days** | Public disclosure (coordinated) |

### Scope

**In Scope:**

- ASG Gateway (`agent.asgcompute.com`)
- ASG Console (`console.asgcompute.com`)
- Payment and billing systems
- Authentication mechanisms

**Out of Scope:**

- Third-party services
- Social engineering attacks
- Physical security
- Denial of service attacks

### Safe Harbor

We will not pursue legal action against researchers who:

- Report vulnerabilities in good faith
- Avoid privacy violations and data destruction
- Give us reasonable time to respond
- Do not exploit vulnerabilities beyond proof-of-concept

### Recognition

We maintain a Hall of Fame for researchers who contribute to our security. Significant findings may be eligible for bounties (at our discretion).

---

## Security Practices

### Data Protection

- All traffic encrypted via TLS 1.3
- No private keys stored server-side
- Minimal data retention policy

### Infrastructure

- Isolated execution environments
- Rate limiting and abuse prevention
- Continuous security monitoring

### Compliance

- SOC2 Type II (in progress)
- GDPR compliant data handling

---

**Last Updated:** 2026-02-01
