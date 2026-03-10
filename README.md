# LuxDiscord

LuxDiscord is a **Paper plugin** that connects your Minecraft server and Discord with linking, moderation tools, tickets, applications, and a built-in web dashboard.

This README is written for **server owners** and is suitable for **Spigot / Modrinth project pages**.

---

# Compatibility

| Component | Requirement |
| --------- | ----------- |
| Platform  | Paper       |
| Minecraft | 1.21+       |
| Java      | 21+         |

---

# Main Features

* Minecraft ↔ Discord **chat mirror**
* **Join/leave announcements** in Discord
* **Account linking** (`/link` in game + `/verify` in Discord)
* Optional **enforcement for unlinked players**
* Discord **staff panel for punishments**
* Discord **ticket system** (with web replies)
* Discord **application system + web form**
* **Web dashboard**

  * server status
  * moderation tools
  * tickets
  * applications
  * configuration
* **Shared website branding/theme settings** for all users

---

# Installation

1. Put the plugin jar in:

```
plugins/
```

2. Start the server once.

3. Edit:

```
plugins/LuxDiscord/config.yml
```

4. Restart the server or run:

```
/luxdiscord reload
```

---

# Build From Source

```
./gradlew build
```

Jar output:

```
build/libs/LuxDiscord-1.0-SNAPSHOT.jar
```

---

# Discord Bot Setup

1. Create a bot in the **Discord Developer Portal**.

2. Set your bot token in config:

```yaml
discord.token: "YOUR_TOKEN"
```

3. Enable intents:

* Message Content Intent
* Server Members Intent

4. Invite the bot with scopes:

```
bot
applications.commands
```

5. Give the bot channel permissions:

* View Channels
* Send Messages
* Read Message History
* Embed Links
* Use Slash Commands

---

# Quick Start (Minimal)

In `config.yml` set at least:

```yaml
discord:
  enabled: true
  token: "YOUR_TOKEN"

  channels:
    chat: "CHANNEL_ID"
```

Then run:

```
/luxdiscord reload
```

---

# Commands

## /link

Usage:

```
/link
```

Description:

Starts Discord account linking and gives a one-time verification code.

Example:

```
/link
```

---

## /ticket

Usage:

```
/ticket [category]
```

Description:

Opens a support ticket (optionally in a chosen category).

Examples:

```
/ticket
/ticket Bug Report
```

---

## /luxdiscord reload

Usage:

```
/luxdiscord reload
```

Description:

Reloads LuxDiscord config and related services.

---

## /luxdiscord admin setup

Usage:

```
/luxdiscord admin setup
```

Description:

Creates or updates a web dashboard admin account.

Example:

```
/luxdiscord admin setup cptrowan MySecurePass123!
```

---

# Discord Slash Commands

## /verify

Finishes the link started with the Minecraft `/link` command.

---

# Permissions

## luxdiscord.chat.server

Default: `true`

Allows in-game chat messages to be forwarded to Discord.

Example:

```
/lp group default permission set luxdiscord.chat.server true
```

---

## luxdiscord.chat.discordreceive

Default: `true`

Allows Discord chat messages to be shown in Minecraft.

Example:

```
/lp group default permission set luxdiscord.chat.discordreceive true
```

---

## luxdiscord.verification.link

Default: `true`

Allows players to use `/link`.

Example:

```
/lp group default permission set luxdiscord.verification.link true
```

---

## luxdiscord.verification.bypass

Default: `op`

Bypasses verification enforcement restrictions.

Example:

```
/lp group admin permission set luxdiscord.verification.bypass true
```

---

## luxdiscord.tickets.open

Default: `true`

Allows players to use `/ticket`.

Example:

```
/lp group default permission set luxdiscord.tickets.open true
```

---

## luxdiscord.admin.reload

Default: `op`

Allows `/luxdiscord reload`.

Example:

```
/lp group admin permission set luxdiscord.admin.reload true
```

---

## luxdiscord.admin.setup

Default: `op`

Allows `/luxdiscord admin setup`.

Example:

```
/lp group admin permission set luxdiscord.admin.setup true
```

---

# Configurable Permission Paths

You can remap permission strings in `config.yml`.

```
permissions.chat.server-to-discord
permissions.chat.discord-to-server
permissions.verification.link
permissions.verification.bypass
permissions.admin.reload
permissions.admin.setup
features.tickets.notify-permission
```

---

# Web Dashboard Setup

Required settings:

```yaml
web:
  enabled: true
  host: 0.0.0.0
  port: 8080
  public-host: example.com
```

Open the dashboard:

```
http://<server-ip>:<web.port>/
```

---

# Web Admin Login Setup

1. Join the server with a player that has:

```
luxdiscord.admin.setup
```

2. Run:

```
/luxdiscord admin setup
```

3. Enter password in chat.

Login with:

```
Username: your lowercase Minecraft name
Password: the password you entered
```

Stored credentials file:

```
plugins/LuxDiscord/web-admins.json
```

(passwords are hashed)

---

# Shared Website Branding / Theme

Website settings in the dashboard are **shared globally** (not browser-local).

Config keys:

```
web.ui.title
web.ui.subtitle
web.ui.theme-preset
```

Theme presets:

```
gunmetal
ocean
forest
sunset
```

---

# Linking Flow

1. Player runs `/link` in Minecraft.
2. Player receives code.
3. Player runs `/verify <code>` in Discord.
4. Link is saved in:

```
plugins/LuxDiscord/links.json
```

---

# Tickets Flow

Enable:

```yaml
features.tickets.enabled: true
```

Set:

```
discord.channels.tickets
```

Optional:

```
discord.channels.tickets-review
features.tickets.staff-role-ids
```

Flow:

1. Ticket panel message posted in Discord
2. User opens ticket from dropdown
3. Staff claim/close from Discord
4. Web dashboard can view/reply/close tickets

---

# Applications Flow

Enable:

```yaml
features.applications.enabled: true
web.enabled: true
```

Set:

```
discord.channels.applications
```

Set public URL:

```
publicip
```

or

```
web.public-host
```

Flow:

1. Discord button gives user a secure application link
2. User submits form at:

```
/apply?token=...
```

3. Staff review in dashboard and approve/reject

---

# URL Resolution Priority

Public application/dashboard base URL is resolved in this order:

1. `publicip`
2. `web.public-host`
3. `web.host + web.port`

---

# Frontend Development (Optional)

Web UI source:

```
web-ui/
```

Build web assets:

```
cd web-ui
npm install
npm run build
```

Assets output to:

```
src/main/resources/web
```

---

# Troubleshooting

## Bot not posting messages or panels

Check:

* bot token
* channel IDs
* feature toggles
* bot permissions

---

## /verify works but player still restricted

Check:

```
features.verification.enforce-link.*
```

and bypass permission.

---

## Application links incorrect

Set explicitly:

```
publicip
```

or

```
web.public-host
```

---

## Web login fails

Re-run:

```
/luxdiscord admin setup
```

Use **lowercase Minecraft username** when logging in.
