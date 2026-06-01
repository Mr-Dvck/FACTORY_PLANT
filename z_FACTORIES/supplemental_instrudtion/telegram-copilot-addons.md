# Telegram Copilot Add-On Snippets

## Telegram bots platform add-on

Use Telegram’s official bot platform docs as the primary source of truth:

- <https://core.telegram.org/bots>
- <https://core.telegram.org/bots/api>
- <https://core.telegram.org/bots/payments-stars>
- <https://core.telegram.org/bots/payments>
- <https://core.telegram.org/bots/webapps>
- <https://core.telegram.org/bots/tutorial>

Use the Telegram bots overview as the architectural entry point for how bots are created, configured, and connected to a backend, then use the Bot API and Stars payments docs for implementation details. Telegram’s docs position bots as server-backed applications created through BotFather and connected through the Bot API, which is the model this implementation should follow.

## Telegram API / MTProto add-on

Use Telegram’s official platform docs as the source of truth, and distinguish clearly between the **Bot API** and the broader **Telegram API / MTProto API**:

- <https://core.telegram.org>
- <https://core.telegram.org/api>
- <https://core.telegram.org/bots>
- <https://core.telegram.org/bots/api>
- <https://core.telegram.org/bots/payments-stars>
- <https://core.telegram.org/bots/payments>
- <https://core.telegram.org/bots/webapps>
- <https://core.telegram.org/bots/tutorial>

Treat the Telegram Bot API as the default implementation path for this project. The Bot API is the HTTP-based interface intended for developers building Telegram bots, and it should be used for invoices, payments, updates, commands, and standard bot interactions.

Treat <https://core.telegram.org/api> as broader background documentation for Telegram’s full platform, including client development and lower-level capabilities that use `api_id` and `api_hash`. Do not switch this project to MTProto unless there is a specific requirement that cannot be handled by the HTTP Bot API.

## MTProto protocol add-on

Include the MTProto Mobile Protocol docs as advanced reference material for Telegram’s low-level, encrypted client protocol, not as the primary implementation surface for this bot or mini-app project.

- <https://core.telegram.org/mtproto>

Treat MTProto and its related pages as background for:

- message structure
- sessions
- authorization keys
- transport behavior
- secret chat and end-to-end concepts

Do **not** reimplement MTProto or use it directly for this bot monetization flow. Bots should continue to use the HTTP-based Telegram Bot API for commands, updates, Mini Apps, and Stars payments.

## Short implementation bias note

- Prefer a backend-first payment architecture.
- Treat Telegram Stars as the only valid in-app purchase currency for digital goods.
- Treat Telegram Wallet as separate from Stars monetization.
- Build for premium subscriptions and gated access first.
- Keep payout conversion as documentation or admin guidance, not an automated bank withdrawal feature.
