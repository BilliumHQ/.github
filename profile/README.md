# Billium

> Non-custodial crypto payments built for developers.

Billium lets merchants accept Bitcoin, Ethereum, and other cryptocurrencies with funds settling **directly to wallets they control**. No custodial intermediary, no KYC layer between merchant and customer, no third party holding funds — designed for developers who want crypto payments without the trade-offs of traditional payment processors.

---

## Get started

- 🌐 [**billium.to**](https://billium.to) — Product overview and merchant signup
- 📦 [**`@billium/node`**](https://www.npmjs.com/package/@billium/node) — Official Node.js SDK on npm

```bash
npm install @billium/node
```

```typescript
import { Billium } from '@billium/node';
import { randomUUID } from 'crypto';

const billium = new Billium({
  apiKey: process.env.BILLIUM_API_KEY,
  merchantId: process.env.BILLIUM_MERCHANT_ID,
  webhookSecret: process.env.BILLIUM_WEBHOOK_SECRET,
});

const invoice = await billium.invoices.create(
  { name: 'Order #1234', rawAmount: 99.99 },
  { idempotencyKey: randomUUID() },
);
```

---

## Open source

Public repositories live under this organization. Each ships independently with its own release cadence.

| Repository | Description |
|---|---|
| [**`billium-node`**](https://github.com/BilliumHQ/billium-node) [![npm](https://img.shields.io/npm/v/@billium/node.svg)](https://www.npmjs.com/package/@billium/node) | Official Node.js SDK — invoices, webhook signature verification, idempotency keys, automatic retries with exponential backoff. Zero runtime dependencies, dual ESM/CJS, npm provenance. |
| [**`.github`**](https://github.com/BilliumHQ/.github) | Org-wide community health files (`CONTRIBUTING.md`, etc.) inherited by every BilliumHQ repository. |

More language SDKs, example integrations, and tooling will land here over time.

---

## Supported chains

Bitcoin · Ethereum · Polygon · BNB Chain · Tron · Litecoin · Cronos

Each chain runs through its own confirmation policy and reorg-aware monitor — there is no "wrap and forward" layer pretending the funds are available before the underlying network agrees.

## Reliability

Webhook delivery is backed by a **transactional outbox pattern** with at-least-once semantics for terminal-state events (`invoice.paid`, `payment.confirmed`, `invoice.expired`, `invoice.cancelled`). Every critical event is durable across process crashes and pod restarts, so a backend hiccup never costs a merchant a missed payment notification. Real-time UI events (`*.updated`) are best-effort, optimized for sub-second latency.

---

## Why non-custodial

Most crypto payment processors take custody of funds before forwarding them to the merchant. That introduces:

- **Counterparty risk** — the processor can freeze, delay, or lose funds before they reach the merchant.
- **KYC/AML overhead** — merchants get treated like financial customers of the processor, with paperwork, hold periods, and limits.
- **Settlement lag** — funds settle T+1, T+2, or longer instead of arriving when the on-chain transaction confirms.

Billium uses [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) / BIP49 / BIP84 derivation paths from the merchant's own xpub. Every invoice gets a fresh address derived from that xpub, so the funds **land in the merchant's wallet directly the moment the on-chain transaction confirms**. Billium never holds the keys, never holds the funds, and can't freeze them. The only thing Billium does on the wire is detect the payment, verify confirmations, and tell the merchant via webhook.

---

## Contact

- **General inquiries** → [billium.to](https://billium.to)
- **Bug reports** for a specific SDK or service → open an issue in the matching repo
- **Security disclosures** → use the [GitHub Security Advisories form](https://github.com/BilliumHQ/billium-node/security/advisories/new) on the affected repository (private to maintainers)

---

<sub>This is the BilliumHQ organization page. The README you're reading lives at [`BilliumHQ/.github/profile/README.md`](https://github.com/BilliumHQ/.github/blob/main/profile/README.md) — open a PR there to suggest changes.</sub>
