# Security policy

Billium handles money. We take security reports seriously and we'd rather hear about a vulnerability ten times than miss it once. This document explains how to disclose a vulnerability privately, what to expect from us, and what is and isn't in scope.

This file lives in [`BilliumHQ/.github`](https://github.com/BilliumHQ/.github) and is inherited as the default `SECURITY.md` by every BilliumHQ repository that doesn't have its own.

---

## Reporting a vulnerability

If you've found a security vulnerability in any Billium component — an SDK, the API, the dashboard, the checkout, the webhook signature scheme, the on-chain monitoring, anything — **please report it privately**, not as a public issue.

The fastest channel is the **GitHub Security Advisories form** on the affected repository:

- Node SDK and webhook verification → [Report a vulnerability on `BilliumHQ/billium-node`](https://github.com/BilliumHQ/billium-node/security/advisories/new)

If the affected component doesn't have its own public repository yet, or you're not sure where the issue lives, file the advisory against [`BilliumHQ/billium-node`](https://github.com/BilliumHQ/billium-node/security/advisories/new) and we'll route it internally.

GitHub Security Advisories give us an end-to-end encrypted channel that's also private to the maintainers — no need for PGP, no need for a side channel. Reports stay confidential until we publish a coordinated advisory.

## What to include

A good report makes the difference between a same-day fix and a multi-day investigation:

- **A clear description** of the vulnerability — what's wrong, what an attacker could do
- **Steps to reproduce** — code, curl commands, sequence of API calls. Copy-pasteable is best.
- **The affected component and version** — SDK version (`SDK_VERSION` or `npm list @billium/node`), API endpoint, dashboard URL, etc.
- **Your assessment of impact** — what data, funds, or merchants are at risk and under what conditions
- **Proof-of-concept code** if applicable. **Please don't run it against real merchants on production** — use a test merchant or describe the attack abstractly.

If the vulnerability involves cryptographic primitives (signatures, key derivation, comparison timing), please include enough mathematical detail for us to verify the attack independently.

## What to expect from us

| Stage | Target |
|---|---|
| Acknowledgment of receipt | Within **24 hours** |
| Initial triage and severity assessment | Within **3 business days** |
| Fix or clear remediation plan for high-severity issues | Within **7 days** |
| Fix or clear remediation plan for everything else | Within **30 days** |
| Coordinated public disclosure (after fix is deployed) | Negotiated with the reporter, typically 30 days from fix |

If we miss any of these targets, you should reach out via the [billium.to](https://billium.to) contact form referencing your advisory ID.

---

## Scope

### In scope

The following **are** valid targets for security research, and we want to hear about issues in any of them:

- **Authentication and authorization** on the public API (`api.billium.to`)
- **API key scoping and permission escalation** — public keys (`pk_*`) being able to call endpoints reserved for secret keys (`sk_*`), or any way to elevate scope beyond what was issued
- **Webhook signature verification** — bypasses, replay attacks, timing attacks on HMAC comparison, header parser confusion
- **Idempotency-Key handling** — replay/dedupe bypass, body-hash collision, lock leakage, race conditions
- **Invoice creation, cancellation, and state transitions** — ways to mutate an invoice you don't own, ways to put an invoice into an inconsistent state
- **On-chain payment detection** — confirmation counting bypasses, reorg handling, double-spend acceptance
- **Address derivation** from xpubs (BIP44 / BIP49 / BIP84) — anything that could leak the merchant's xpub or derive addresses they don't own
- **Cross-tenant data leakage** — one merchant being able to read another merchant's invoices, customers, payments, or webhook secrets
- **Cryptographic primitives** — HMAC comparison timing, signature parser fuzzing, secret comparison side channels
- **Dashboard session management** (`app.billium.to` or wherever it's hosted) — session fixation, CSRF on state-changing endpoints, account takeover
- **Checkout flow** — anything that could let an attacker complete a checkout for someone else, or redirect funds
- **Anything that could result in funds being misdirected, double-spent, lost, or held when they shouldn't be**

### Out of scope

These are **not** vulnerabilities we'll prioritize, even if technically valid:

- **Third-party services we don't control** — npm registry compromise, GitHub itself, DNS providers, CDN providers, blockchain RPC providers. If you find a vulnerability in `@nestjs/throttler` or `prisma` directly, please report it to those projects.
- **Denial of service via volume.** We have rate limiting at the API gateway and per-API-key. If you find a way to **bypass** the rate limit, that **is** in scope; if you can take Billium down by sending more requests than the rate limit allows, that's a capacity issue, not a security issue.
- **Theoretical attacks requiring physical access** to a merchant's machine, or that assume the attacker has already compromised the merchant's developer laptop.
- **Social engineering** of Billium staff, customers, or end users.
- **Issues in the underlying blockchains themselves.** A 51% attack on a small chain we support is not a Billium vulnerability. A way to exploit a confirmation policy we set is.
- **Self-XSS** that requires the victim to paste attacker-controlled content into their own browser console.
- **Missing security headers without a demonstrated impact.** "You should set `X-Frame-Options` on your marketing page" is not a security issue if there's no reason the page would ever be iframed maliciously.
- **Outdated dependencies without a working exploit.** If a `dependabot` alert says version X.Y.Z has a vulnerability, please check whether the vulnerable code path is actually reachable in our usage before reporting.
- **Best-practice deviations without a working exploit.** "You should use Argon2 instead of bcrypt" is not in scope unless you can show a practical attack that the Argon2 swap would prevent.

If you're not sure whether your finding is in scope, **err on the side of disclosing privately**. We'd rather investigate something that turns out to be a non-issue than have a real one filed in public.

---

## Safe harbor

Billium will **not pursue legal action** against security researchers who:

- Act in **good faith** and do not intentionally cause harm
- Do not access, modify, or destroy data belonging to other merchants or end users
- Do not disclose the vulnerability publicly before we've had a chance to investigate, fix, and coordinate a release
- Do not extort, blackmail, or otherwise exploit the vulnerability for personal gain
- Comply with applicable laws

This safe harbor applies to research conducted on **Billium's own infrastructure**. We can't grant safe harbor for testing against a third party's deployment of our SDK or against a merchant's installation — for that, you need permission from the affected party.

If at any point during your research you accidentally access data you shouldn't have, **stop, report what happened, and delete what you accessed**. We'll work with you in good faith on the assumption that your intent was to find and report the issue, not to exfiltrate data.

---

## Coordinated disclosure

We strongly prefer **coordinated disclosure**. The flow we'll follow:

1. You report the vulnerability privately via the GitHub Security Advisories form.
2. We acknowledge within 24 hours.
3. We investigate, validate, and develop a fix.
4. We coordinate a **disclosure timeline** with you. The default is **30 days** from a working fix; we'll extend it if the fix requires merchants to migrate (so they have time to upgrade before the issue is public).
5. After the fix is deployed and merchants have had time to upgrade, we publish a public security advisory crediting you (with your permission) and any other researchers who reported the same issue independently.

If you find a vulnerability that's **actively being exploited in the wild**, please tell us immediately — we'll prioritize a fix above coordinated disclosure timing and may publish before the standard 30-day window if doing so reduces ongoing harm.

---

## What we don't offer (yet)

- **Bug bounty rewards.** Billium does not currently pay cash for vulnerability reports. We acknowledge contributions in our public security advisories with the reporter's name (or anonymously, if preferred). A formal bounty program may come later as the platform matures.
- **PGP-encrypted email.** We use GitHub Security Advisories, which provides equivalent end-to-end privacy without the key management overhead.
- **A dedicated security@ inbox.** Same reason — GitHub Security Advisories is the canonical channel.

If you need to communicate something genuinely sensitive that doesn't fit the GitHub Security Advisories form, reach out via the [billium.to](https://billium.to) contact form and ask for a side channel.

---

## Past advisories

Public security advisories will be listed at https://github.com/BilliumHQ/billium-node/security/advisories once any are published. None at the time of writing.
