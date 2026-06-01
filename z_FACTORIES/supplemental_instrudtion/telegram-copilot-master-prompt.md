# Telegram Copilot Master Prompt

You are implementing a production-grade Telegram monetization layer for a bot and/or Telegram Mini App that targets crypto-adjacent use cases such as signals, analytics, automation, and premium communities, but it must follow Telegram’s official payment rules and architecture.

## Authoritative docs and scope

Use these official Telegram docs as primary references:

- Telegram platform overview: <https://core.telegram.org>
- Telegram API (MTProto, client-level API): <https://core.telegram.org/api>
- Telegram Bots hub: <https://core.telegram.org/bots>
- Telegram Bot API (HTTP interface for bots): <https://core.telegram.org/bots/api>
- Bot Payments API (general): <https://core.telegram.org/bots/payments>
- Bot Payments API for Digital Goods and Services (Telegram Stars): <https://core.telegram.org/bots/payments-stars>
- Telegram Mini Apps (Web Apps): <https://core.telegram.org/bots/webapps>
- From BotFather to “Hello World”: <https://core.telegram.org/bots/tutorial>

Additionally, treat the broader MTProto Mobile Protocol as background for low-level protocol behavior, not as the implementation surface for this project:

- MTProto Mobile Protocol hub: <https://core.telegram.org/mtproto>
- Related pages: description, auth key creation, binary serialization, TL language, TL schema, service messages, end-to-end encryption, PFS, and transport variants.

Assume the full TL-schema is available as a reference, but do not attempt to regenerate or replace it.

### Scope decision rule

- Default to the **Bot API (HTTP)** for bot creation, updates, webhooks or long polling, commands, Telegram Stars payments, Mini App integration, and entitlement management.
- Use the **Telegram API / MTProto** only as conceptual background or if an explicit requirement cannot be satisfied by the Bot API.
- Do **not** reimplement MTProto or hand-roll TL serialization for this project.

## Business goal: Stars monetization for crypto apps

The product is a Telegram bot and/or Mini App that offers crypto-related digital services such as:

- Premium trading signals or strategy alerts.
- Private VIP groups or paid research channels.
- Portfolio or whale alerts and on-chain analytics dashboards.
- AI or trading assistants and automation tools.
- Other digital-only services delivered via Telegram messages, content, access, or features.

All of these are **digital goods/services inside Telegram**. They must:

- Be sold in **Telegram Stars** with currency `"XTR"` when purchased inside a bot or Mini App.
- Use the Bot Payments Stars flow.
- Not use direct in-app BTC, TON, USDT, Stripe, or other processors for in-bot or in-app purchases of digital goods.

## Payment and entitlement architecture

Implement a backend-first architecture with clear separation between:

- Bot interface layer (updates, commands, messages, callback queries).
- Mini App frontend (if present; JS/WebApp API).
- Billing service (Stars invoices, payment confirmation, refunds).
- Entitlement service (who owns what and until when).
- Admin/operator view (Stars balances, approximate fiat, payout instructions).

### Core payment flow with Stars

Use this payment flow for digital goods:

1. Bot or Mini App presents product options, for example `signals_monthly`, `vip_group_monthly`, and `pro_terminal_weekly`.
2. Backend creates invoice using `sendInvoice` with:
   - `currency: "XTR"`
   - prices in Stars units as integers
   - `provider_token` blank where required by Stars docs for digital goods
3. Bot sends invoice, or Mini App opens invoice, to the user.
4. On payment attempt, backend handles `pre_checkout_query` and answers with `answerPreCheckoutQuery` within 10 seconds after validating payload.
5. On success, backend receives `successful_payment` update and verifies:
   - `telegram_payment_charge_id`
   - `total_amount`
   - `currency` equals `"XTR"`
   - `invoice_payload` maps to known SKU and entitlement
6. Backend creates or extends entitlements and logs the transaction atomically.
7. Bot or Mini App updates UI to unlock the purchased product.

### SKU configuration layer

Use a clearly defined product configuration structure. Example:

```ts
type ProductSku = {
  sku: string;
  title: string;
  description: string;
  starsAmount: number;
  entitlementType: 'subscription' | 'one_time' | 'access_pass';
  durationDays?: number;
  telegramPayload: string;
  groupAccessId?: string;
  featureFlags?: string[];
};
```

Example SKUs, which must remain configurable rather than hardcoded into business logic:

- `signals_monthly`: 500 Stars for 30 days of premium signals.
- `vip_group_monthly`: 900 Stars for 30 days of private group access.
- `pro_terminal_weekly`: 250 Stars for 7 days of analytics features.

### Entitlement rules

- Store entitlements in a persistent store such as Postgres, keyed by `telegram_user_id`, `sku`, and expiry where applicable.
- Every premium feature check must go through the entitlement service.
- Never trust the client or UI state alone.
- Implement renewal or extension logic for repeat purchases.
- Provide an admin query path to inspect entitlements and raw payments.

## Withdrawal and Stars to TON to fiat

Model operator payout logic as operational guidance, not as an in-app bank transfer.

- Creators earn Stars in their bot, channel, or mini-app account.
- Stars are not instantly withdrawable.
- Minimum withdrawal is typically 1,000 Stars.
- Newly earned Stars are usually locked for about 21 days before withdrawal.

### Withdrawal flow

1. Creator opens the bot, channel, or admin Stars panel.
2. Telegram redirects them to Fragment.
3. Creator connects a TON wallet such as TON Space, Tonkeeper, or Bitget Wallet.
4. Fragment converts Stars to TON at the effective rate.
5. TON is sent to the connected wallet.

### Converting to fiat or stablecoins

1. Move TON from wallet to a centralized exchange.
2. Sell TON for USDT or fiat.
3. Off-ramp via exchange P2P or fiat rails.

Inside the codebase or admin UI, track Stars-based revenue as:

- `stars_earned_total`
- `stars_pending_eligibility`
- `stars_eligible_for_withdrawal` (approximate, informational only)

Do **not** attempt to call Fragment or exchanges from the app for automatic off-ramping. Just document the process clearly for operators.

## Telegram Wallet vs Stars

Telegram also has **Telegram Wallet** (`@wallet`), a custodial wallet inside Telegram that supports buying, selling, sending, exchanging, and earning yield across assets like TON, USDT, and other tokens.

For this project:

- You may expose Telegram Wallet-related informational or optional flows, such as “Connect Telegram Wallet” or “View your wallet balance”.
- Do **not** mix Wallet balances with Stars balances in the UI or code.
- Do **not** treat Wallet as the monetization mechanism for in-app digital goods.
- Stars monetization is one system, and Wallet is another system. Keep them logically separated.

## Telegram API, MTProto, and TL schema context

The attached TL-schema file and MTProto docs describe how the core Telegram protocol works:

- High-level RPC API with methods, message types, and TL types.
- Cryptographic layer with auth keys, message keys, and AES-256 encryption.
- Transport layer with MTProto transports over TCP, HTTP/HTTPS, WebSocket, and UDP.
- Session semantics, message IDs, sequence numbers, containers, and acknowledgements.
- End-to-end encryption, secret chats, and PFS-related schemas.

Use this information only for understanding:

- How Telegram as a platform implements secure, session-based RPC.
- Why time synchronization, message IDs, and sessions matter.
- How bots can, in theory, use MTProto with bot authorization plus `api_id` and `api_hash`.

But for this project:

- Do **not** implement raw MTProto clients.
- Do **not** parse TL manually.
- Do **not** design the bot to depend on MTProto-level calls.
- Stick to the **Bot API (HTTP)** and official libraries or wrappers for all practical bot and mini-app functionality, including payments, Stars, and updates.

If you need to refer to TL types or MTProto behaviors for naming consistency or conceptual understanding, treat the existing TL-schema as a read-only reference and do not replicate or regenerate it.

## Concrete implementation tasks

Generate production-oriented code and structure for:

### 1. Bot backend

Default to Node.js + Telegraf or Python + aiogram if unspecified.

- Webhook or long-poll update handler.
- Command handlers such as `/start`, `/help`, `/pricing`, `/buy_*`.
- Callback query handling and state flows.
- `sendInvoice`, `pre_checkout_query`, and `successful_payment` handling.
- Logging, error handling, and minimal telemetry.

### 2. Billing module

- SKU registry or config.
- `createInvoice(sku, userCtx)` with validation.
- `handlePreCheckoutQuery(update)` with checks.
- `handleSuccessfulPayment(update)` that creates or extends entitlements atomically.
- Optional `initiateRefund` or `markRefunded` scaffolding.

### 3. Entitlement module

- Database schema or model definitions for users, products, entitlements, and payments.
- Idempotent upsert logic for entitlements on payment success.
- Check helpers such as `userHasActiveEntitlement(userId, feature)`.

### 4. Mini App bridge

- Endpoint(s) for Mini App to request invoice for a given SKU.
- Endpoint(s) for checking entitlement status and gating UI.
- Proper use of Telegram WebApp `initData` verification for auth.

### 5. Admin/operator helpers

- Basic CLI or admin endpoints to:
  - list payments and entitlements
  - inspect Stars revenue approximately based on Stars totals
  - output payout instructions for Stars to Fragment to TON to exchange to fiat

### 6. Security and robustness

- Never log full card or credential data.
- Never expose bot token to clients.
- Validate invoice payloads, amounts, and currencies before granting access.
- Consider replay protection and idempotency for payment handlers.
- Follow Telegram security guidelines and Bot API constraints.

## Output style

- Generate code as if this is going into a real repository with modules, types, and clear separation of concerns.
- Prefer explicit types or interfaces and dependency injection where useful.
- Include comments only where they clarify protocol or business rules.
- Where exact field values or formats are required by Telegram, follow the Bot API docs precisely.

## Final instruction

Generate:

- Core backend modules for bot, billing, and entitlements.
- Optional Mini App endpoints where appropriate.
- Any glue code necessary to have a fully working Stars-based monetization layer on Telegram that respects platform rules and keeps MTProto/TL at the background-reference level only.
