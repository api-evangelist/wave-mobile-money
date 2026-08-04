# Wave Mobile Money (wave-mobile-money)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Wave is a Dakar, Senegal-headquartered mobile money company on a mission to "make Africa the first cashless continent." Founded by Drew Durbin and Lincoln Quirk (previously the founders of the Sendwave remittance service) and publicly launched in Senegal in 2018, Wave provides a smartphone- and agent-based wallet that lets users deposit and withdraw cash for free and send money domestically for a flat 1% fee — roughly 70% cheaper than the telco-led mobile money incumbents in the region. Wave operates today in Senegal, Côte d'Ivoire, Uganda, Mali, Burkina Faso, and The Gambia, working through a dense network of human agents alongside the Wave app and a separate Wave Business portal for merchants and enterprises. In September 2021, Wave raised a USD 200M Series A led by Sequoia Heritage, Founders Fund, Stripe, and Ribbit at a USD 1.7B valuation — the largest Series A ever raised on the African continent at the time.

For developers, Wave exposes a tier-1 B2B platform via `api.wave.com`, documented at [docs.wave.com](https://docs.wave.com/business), spanning Checkout (hosted payment sessions), Payout (single and batch disbursements with reversal and recipient verification), Balance & Reconciliation (wallet balance, transactions, refunds), Aggregated Merchants (sub-merchant CRUD for PSPs and aggregators), and Webhooks (HMAC-SHA256-signed events). API keys are scoped per-API, support IP allowlisting and optional request signing, and the platform enforces idempotency on POST mutations.

**URL:** [Visit APIs.json](https://raw.githubusercontent.com/api-evangelist/wave-mobile-money/refs/heads/main/apis.yml)

## Tags

 - Mobile Money, Payments, Fintech, Financial Inclusion, Africa, West Africa, Senegal, Cote d'Ivoire, Uganda, Mali, Burkina Faso, Wallets, Disbursements, Checkout, Payouts, Money Transfer

## Timestamps

- **Created:** 2026-05-24
- **Modified:** 2026-05-24

## APIs

### Wave Checkout API
Hosted payment session API for accepting one-off payments from Wave wallet users. Create a checkout session, redirect the customer to the returned `wave_launch_url`, then receive a `checkout.session.completed` webhook (or poll the session) once payment succeeds. Supports `success_url`/`error_url` redirects, `client_reference` correlation, `restrict_payer_mobile` phone-locked payments, and `aggregated_merchant_id` for PSPs. Includes session retrieval, search by transaction_id or client_reference, refund, and explicit expire.

**Human URL:** [https://docs.wave.com/checkout](https://docs.wave.com/checkout)
**Base URL:** `https://api.wave.com`

### Wave Payout API
Programmatic disbursement API for sending money from a business wallet to individual Wave wallets identified by E.164 mobile number. Supports single payouts, payout batches, payout reversal, payout lookup by id or client reference, and a `verify_recipient` pre-flight check. All POST requests require an `Idempotency-Key` header for safe retries. Used for bulk payroll, gig economy payouts, marketplace seller settlement, humanitarian cash transfer, and out-of-band refunds.

**Human URL:** [https://docs.wave.com/payout](https://docs.wave.com/payout)
**Base URL:** `https://api.wave.com`

### Wave Balance & Reconciliation API
Read-side API for inspecting a business wallet's current balance and enumerating transactions for accounting and reconciliation. `GET /v1/balance` returns the live wallet balance (optionally aggregated across subaccounts); `GET /v1/transactions` returns a paginated, time-ordered list of transactions for a given day; `POST /v1/transactions/:id/refund` reverses a received payment including any fees. Forward cursor pagination via `first`/`after`.

**Human URL:** [https://docs.wave.com/balance-api](https://docs.wave.com/balance-api)
**Base URL:** `https://api.wave.com`

### Wave Aggregated Merchants API
Sub-merchant management for aggregators, payment service providers, and platforms accepting Wave payments on behalf of many downstream businesses. Full CRUD over aggregated merchant records — list, create, retrieve, update, delete — with the resulting `aggregated_merchant_id` attached to Checkout and Payout calls so funds and reporting flow to the correct underlying merchant.

**Human URL:** [https://docs.wave.com/aggregated-merchants](https://docs.wave.com/aggregated-merchants)
**Base URL:** `https://api.wave.com`

### Wave Webhooks
Outbound event delivery channel for asynchronously notifying merchant systems when state changes on the Wave platform. Events include `checkout.session.completed`, `checkout.session.payment_failed`, `b2b.payment_received`, `b2b.payment_failed`, `merchant.payment_received`, and `test.test_event`. Every delivery carries a `Wave-Signature` header containing a Unix timestamp and one or more HMAC-SHA256 signatures over the raw body; receivers must validate the signature and reject deliveries older than five minutes to prevent replay. Event payloads use a Stripe-style envelope (`id`, `type`, `data`).

**Human URL:** [https://docs.wave.com/webhook](https://docs.wave.com/webhook)
**Base URL:** `https://api.wave.com`

## Common Resources

- [Website](https://www.wave.com)
- [Wave for Business Portal](https://business.wave.com/)
- [API Reference](https://docs.wave.com/business)
- [About](https://www.wave.com/en/about/)
- [Careers](https://www.wave.com/en/careers/)
- [Blog](https://www.wave.com/en/blog/)
- [Wave Engineering Blog](https://wave.engineering/)
- [Responsible Disclosure](https://www.wave.com/en/security/responsible_disclosure)
- [Terms of Service](https://www.wave.com/en/terms_and_conditions/)
- [Privacy Policy](https://www.wave.com/en/privacy/)
- [Y Combinator](https://www.ycombinator.com/companies/wave)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
