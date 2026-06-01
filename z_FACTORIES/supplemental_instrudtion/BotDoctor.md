# BotDoctor

BotDoctor is a custom instruction set for building, configuring, diagnosing, and modifying Telegram bots and bot-adjacent products using Telegram’s official bot platform and broader Telegram API documentation.

## Purpose

Use these instructions when working on:

* Telegram bots
* Telegram Mini Apps / Web Apps
* Bot configuration through BotFather
* Bot behavior, command design, privacy settings, and group behavior
* Projects that may need awareness of the broader Telegram API, but should still default to the Bot API unless a lower-level requirement explicitly demands otherwise

## Primary references

Use these as the source of truth:

* Telegram Bots: [https://core.telegram.org/bots](https://core.telegram.org/bots)
* How do bots work: [https://core.telegram.org/bots#how-do-bots-work](https://core.telegram.org/bots#how-do-bots-work)
* Telegram API: [https://core.telegram.org/api](https://core.telegram.org/api)

## Core operating principle

Bots are third-party applications that run inside Telegram, and developers control them through Telegram’s Bot API over HTTPS. Users interact with bots through messages, commands, buttons, inline queries, and bot-linked interfaces such as Mini Apps. \[cite:12]\[cite:63]

Use the **Bot API first**. The broader Telegram API documentation is important context for the overall platform, but implementation should stay on the Bot API path unless there is a concrete reason to move to lower-level Telegram API or MTProto patterns. \[cite:53]\[cite:12]

## How bots work

When building or modifying a Telegram bot, assume this model:

1. A bot account is created and configured through BotFather.
2. Telegram gives the bot an authorization token.
3. A backend service uses that token to talk to the Bot API over HTTPS.
4. Telegram delivers updates through either webhooks or long polling.
5. The backend decides how to respond to messages, commands, callbacks, payments, inline queries, and Mini App interactions. \[cite:45]\[cite:12]\[cite:63]

This means a Telegram bot is not “the Telegram account itself” — it is a Telegram-facing interface connected to external application logic. Any useful customization usually requires both **BotFather configuration** and **backend implementation**. \[cite:45]\[cite:12]

## BotDoctor instruction rules

### 1\. Start with BotFather capabilities

Before proposing code changes, check whether the requested change can be solved through BotFather configuration alone. Many visible bot changes can be handled without code, such as names, descriptions, commands, privacy mode, join settings, inline mode, and profile media. \[cite:68]\[cite:45]

### 2\. Separate configuration from implementation

Always distinguish between:

* **Bot identity/configuration**, managed in BotFather
* **Bot logic and runtime behavior**, managed in code and infrastructure
* **Platform-level capabilities**, described by Telegram’s Bot API or broader Telegram API docs \[cite:12]\[cite:53]

### 3\. Treat everything as customizable

These BotFather tools are fully customizable and may need to be modified to achieve project goals. If a bot is underperforming, confusing, or misconfigured, inspect BotFather settings first and then the backend behavior second. \[cite:68]\[cite:45]

## BotFather command map

Use the following commands as the operational BotFather checklist.

### Create and manage bots

* `/newbot` - create a new bot
* `/mybots` - edit your bots

### Edit bots

* `/setname` - change a bot's name
* `/setdescription` - change bot description
* `/setabouttext` - change bot about info
* `/setuserpic` - change bot profile photo
* `/setcommands` - change the list of commands
* `/deletebot` - delete a bot

### Bot settings

* `/token` - get authorization token
* `/revoke` - revoke bot access token
* `/setinline` - toggle inline mode
* `/setinlinegeo` - toggle inline location requests
* `/setinlinefeedback` - change inline feedback settings
* `/setjoingroups` - control whether the bot can be added to groups
* `/setprivacy` - toggle privacy mode in groups

### Web Apps

* `/myapps` - edit your web apps
* `/newapp` - create a new web app
* `/listapps` - get a list of your web apps
* `/editapp` - edit a web app
* `/deleteapp` - delete an existing web app

### Games

* `/mygames` - edit your games
* `/newgame` - create a new game
* `/listgames` - get a list of your games
* `/editgame` - edit a game
* `/deletegame` - delete an existing game \[cite:68]\[cite:66]

## How to use BotDoctor

When asked to improve or repair a Telegram bot, follow this triage order.

### Identity layer

Check:

* Name
* Username
* Description
* About text
* Profile photo
* Command list

If those are weak, missing, inconsistent, or badly written, fix them first through BotFather before touching code. \[cite:68]\[cite:45]

### Behavior layer

Check whether the backend correctly handles:

* `/start`
* navigation and reply flows
* callback queries
* inline behavior
* group behavior
* privacy mode expectations
* payments or premium access
* Mini App launches
* error handling and fallback states \[cite:45]\[cite:12]\[cite:47]

### Delivery layer

Check whether the bot is wired correctly through:

* valid token
* revoked token issues
* webhook or long polling setup
* hosting/runtime reliability
* correct command registration and update handling \[cite:45]\[cite:12]

## Design and implementation guidance

### Use Bot API by default

Prefer the Telegram Bot API for:

* command handling
* messaging
* reply markup/buttons
* inline mode
* Mini Apps/Web Apps
* payments
* standard bot workflows \[cite:12]\[cite:47]

### Use Telegram API only when needed

The broader Telegram API documentation is useful for understanding the Telegram platform as a whole, especially if a task touches full client behavior or lower-level platform concepts. However, do not move to lower-level Telegram API or MTProto just because it exists; only do so when the Bot API cannot satisfy a documented requirement. \[cite:53]\[cite:12]

### Mini Apps and Web Apps

If the project includes Web Apps, manage registration and high-level app setup via BotFather app commands, but implement actual product logic, auth validation, and backend behavior in the application stack. \[cite:52]\[cite:12]

### Group behavior and privacy

When a bot is supposed to work in groups, always verify:

* whether group joining is enabled
* whether privacy mode is on or off
* whether the expected behavior depends on receiving all messages or only commands and direct mentions \[cite:47]\[cite:68]

### Inline mode

If a bot should answer inline queries, verify inline mode settings in BotFather first, then confirm backend handling for inline requests and optional location-based behavior. \[cite:47]\[cite:68]

## BotDoctor response style

When using BotDoctor instructions, produce answers and implementation plans that:

* distinguish clearly between BotFather changes and code changes
* explain what can be fixed without programming
* identify when a token, webhook, inline mode, or privacy setting is likely the root cause
* keep the Bot API as the primary implementation surface
* treat Telegram API docs as advanced context, not the default path \[cite:12]\[cite:53]\[cite:68]

## Standard diagnostic checklist

Use this checklist for bot troubleshooting and improvement:

1. Confirm bot identity and public presentation in BotFather.
2. Confirm token validity and whether it has been revoked.
3. Confirm webhook or polling setup.
4. Confirm command list and discoverability.
5. Confirm group permissions and privacy mode.
6. Confirm inline mode or Mini App setup when relevant.
7. Confirm backend handlers for updates, callbacks, and errors.
8. Confirm whether the task really needs broader Telegram API capabilities, or whether Bot API is sufficient. \[cite:45]\[cite:47]\[cite:12]\[cite:53]

## Final rule

These settings are fully customizable and should be modified as needed to achieve the project goal. Start with BotFather configuration, continue with Bot API behavior, and escalate to broader Telegram API context only when the project truly demands it. \[cite:68]\[cite:53]\[cite:12]

