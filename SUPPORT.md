# Getting help with Billium

This page covers where to go for different kinds of help across every BilliumHQ repository. Pick the channel that best matches your situation — using the right one upfront will get you a faster, more useful answer.

This file lives in [`BilliumHQ/.github`](https://github.com/BilliumHQ/.github) and is inherited as the default `SUPPORT.md` by every BilliumHQ repository that doesn't have its own.

---

## I need help integrating Billium

Start with the official documentation:

- 🌐 [**billium.to**](https://billium.to) — Product overview, REST API reference, SDK quickstarts, webhook event reference
- 📦 [**`@billium/node` README**](https://github.com/BilliumHQ/billium-node#readme) — Node.js SDK quickstart, configuration, retry/idempotency guide

If the docs don't answer your question, **open an issue** on the repository whose SDK or service you're integrating, using the "question" or "feature request" template:

- Node SDK questions → [BilliumHQ/billium-node/issues](https://github.com/BilliumHQ/billium-node/issues/new/choose)

We try to respond within a few business days. If your question is urgent or affects production traffic, see [**I think Billium is broken in production**](#i-think-billium-is-broken-in-production) below.

---

## I think I found a bug

**Open a GitHub issue** on the repository where the bug lives, using the bug report template:

- SDK bugs → [`BilliumHQ/billium-node`](https://github.com/BilliumHQ/billium-node/issues/new/choose) → "Bug report"

When filing a bug, please include:

- A **minimal reproduction** — copy-pasteable code is gold and gets fixed first
- The **SDK version** (`SDK_VERSION` from the imported package, or `npm list @billium/node`)
- Your **runtime version** (`node --version`, or equivalent for Deno / Bun / Workers)
- **What you expected** to happen vs **what actually** happened
- Any relevant **logs** with secrets redacted (`x-api-key` headers, webhook secrets, full invoice IDs of real customers, etc.)

The bug report template walks you through these. Filling them in **before** asking for help saves at least one round-trip and gets your issue resolved much faster.

---

## I think Billium is broken in production

If your invoices aren't being created, payments aren't being detected on-chain, or webhooks aren't arriving, this is an incident — not a regular bug report. Reach out through the [billium.to](https://billium.to) contact form so we can investigate immediately.

When reporting a production incident, include:

- Your **merchant ID** (the `mer_*` value from your dashboard)
- The **timeframe** of the incident (e.g. "started failing around 2026-04-08 14:30 UTC")
- An **example invoice ID** that's affected (if applicable)
- Whether the issue is **ongoing or resolved**
- The **HTTP status code** and **response body** of the failing request, if any

We prioritize incident reports above general issues and discussions.

---

## I have a question about my account, billing, or pricing

These aren't technical questions, so they don't belong on GitHub. Reach out through the [billium.to](https://billium.to) contact form and the team will route you to the right place.

---

## I think I found a security vulnerability

**Don't open a public issue.** Use the GitHub Security Advisories form on the affected repository:

- Node SDK → [Report a vulnerability](https://github.com/BilliumHQ/billium-node/security/advisories/new)

Reports stay **private to the maintainers** until a coordinated disclosure is ready. We acknowledge receipt within 24 hours and aim to ship a fix and a public advisory within a reasonable timeline depending on severity.

For payment-processor-style threats specifically (signature bypasses, replay attacks, idempotency gaps, key handling, on-chain reorg issues, etc.), please err on the side of disclosing privately even if you're not sure it's exploitable. We'd rather investigate something that turns out to be a non-issue than have a real one filed in public.

---

## I want to suggest a feature

Open an issue with the "feature request" template on the repository the feature would live in. Concrete proposals — code snippets, API shapes, real use cases you've hit — get further than abstract ideas. The best feature requests describe the **problem** you're trying to solve first, and the proposed solution second; sometimes there's a simpler answer than the one you came in with.

---

## Response times

Billium is operated by a small team. Best-effort response targets, by category:

| Category | Target |
|---|---|
| Production incident affecting payments | Within hours |
| Security disclosure (private advisory) | Within 24 hours |
| Bug report (non-incident) | Within a few business days |
| Integration question / general inquiry | Within a few business days |
| Feature request / discussion | When we get to it — no SLA |

We don't have a paid support tier today. If your business needs guaranteed response times for production-critical integrations, reach out via [billium.to](https://billium.to) and we'll figure out an arrangement that works.

---

## Where Billium is **not** the right place to ask

A few things we can't help with directly, just so you don't waste time waiting on a response:

- **General Bitcoin / Ethereum / blockchain questions** — try [Bitcoin Stack Exchange](https://bitcoin.stackexchange.com/) or [Ethereum Stack Exchange](https://ethereum.stackexchange.com/). We can answer questions about how Billium *uses* a chain, not about the chain itself.
- **Wallet recovery, lost seed phrases, lost keys** — Billium is non-custodial and never holds your keys. We literally cannot recover funds for you. Contact your wallet vendor (Ledger, Trezor, MetaMask, etc.).
- **Tax / accounting / regulatory questions** — talk to a qualified accountant or lawyer in your jurisdiction. We can give you the data (transaction history, invoice records, settlement reports), but we can't tell you how to file it.
- **Help building someone else's product** — we're happy to answer questions about integrating Billium specifically, but we can't do general consulting on your unrelated codebase.
