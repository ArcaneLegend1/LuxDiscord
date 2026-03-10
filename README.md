LuxDiscord
LuxDiscord is a Paper plugin that connects your Minecraft server and Discord with linking, moderation tools, tickets, applications, and a built-in web dashboard.

This README is written for server owners and is suitable for Spigot/Modrinth project pages.

Compatibility
Platform: Paper
Minecraft: 1.21+
Java: 21+
Main Features
Minecraft <-> Discord chat mirror
Join/leave announcements in Discord
Account linking (/link in game + /verify in Discord)
Optional enforcement for unlinked players
Discord staff panel for punishments
Discord ticket system (with web replies)
Discord application system + web form
Web dashboard (status, moderation, tickets, applications, config)
Shared website branding/theme settings for all users
Installation
Put the plugin jar in plugins/.
Start server once.
Edit plugins/LuxDiscord/config.yml.
Restart server (or run /luxdiscord reload).
Build from source:

./gradlew build
Jar output:

build/libs/LuxDiscord-1.0-SNAPSHOT.jar
Discord Bot Setup
Create a bot in Discord Developer Portal.
Set your bot token in discord.token.
Enable intents:
Message Content Intent
Server Members Intent
Invite bot with scopes:
bot
applications.commands
Give the bot channel permissions:
View Channels
Send Messages
Read Message History
Embed Links
Use Slash Commands
Quick Start (Minimal)
In config.yml set at least:

discord.enabled: true
discord.token: "YOUR_TOKEN"
discord.channels.chat: "CHANNEL_ID"
Then run:

/luxdiscord reload
Commands
/link
Usage: /link
Description: Starts Discord account linking and gives a one-time verification code.
Example: /link
/ticket
Usage: /ticket [category]
Description: Opens a support ticket (optionally in a chosen category).
Examples:
/ticket
/ticket Bug Report
/luxdiscord reload
Usage: /luxdiscord reload
Description: Reloads LuxDiscord config and related services.
Example: /luxdiscord reload
/luxdiscord admin setup
Usage: /luxdiscord admin setup <username> <password>
Description: Creates or updates a web dashboard admin account.
Example: /luxdiscord admin setup cptrowan MySecurePass123!
Discord Slash Commands
/verify <code>
Finishes the link started with Minecraft /link.
Permissions
luxdiscord.chat.server
Default: true
Description: Allows in-game chat messages to be forwarded to Discord.
Example (LuckPerms): /lp group default permission set luxdiscord.chat.server true
luxdiscord.chat.discordreceive
Default: true
Description: Allows Discord chat messages to be shown in Minecraft.
Example (LuckPerms): /lp group default permission set luxdiscord.chat.discordreceive true
luxdiscord.verification.link
Default: true
Description: Allows players to use /link.
Example (LuckPerms): /lp group default permission set luxdiscord.verification.link true
luxdiscord.verification.bypass
Default: op
Description: Bypasses verification enforcement restrictions.
Example (LuckPerms): /lp group admin permission set luxdiscord.verification.bypass true
luxdiscord.tickets.open
Default: true
Description: Allows players to use /ticket.
Example (LuckPerms): /lp group default permission set luxdiscord.tickets.open true
luxdiscord.admin.reload
Default: op
Description: Allows /luxdiscord reload.
Example (LuckPerms): /lp group admin permission set luxdiscord.admin.reload true
luxdiscord.admin.setup
Default: op
Description: Allows /luxdiscord admin setup <username> <password>.
Example (LuckPerms): /lp group admin permission set luxdiscord.admin.setup true
Configurable Permission Paths
You can remap permission strings in config.yml:

permissions.chat.server-to-discord
permissions.chat.discord-to-server
permissions.verification.link
permissions.verification.bypass
permissions.admin.reload
permissions.admin.setup
features.tickets.notify-permission
Web Dashboard Setup
Required settings:

web.enabled: true
web.host and web.port (bind address)
web.public-host and/or publicip for public links
Open dashboard:

http://<server-host>:<web.port>/
Web Admin Login Setup
Join server with account that has luxdiscord.admin.setup.
Run /luxdiscord admin setup.
Enter password in chat.
Login with:
Username: your lowercase Minecraft name
Password: the password you entered
Stored credentials file:

plugins/LuxDiscord/web-admins.json (hashed)
Shared Website Branding / Theme
Website settings in dashboard are shared globally (visible to everyone), not browser-local.

Config keys:

web.ui.title
web.ui.subtitle
web.ui.theme-preset (gunmetal, ocean, forest, sunset)
Linking Flow
Player runs /link in Minecraft.
Player receives code.
Player runs /verify <code> in Discord.
Link is saved in plugins/LuxDiscord/links.json.
Tickets Flow
Enable features.tickets.enabled: true.
Set discord.channels.tickets.
Optional: discord.channels.tickets-review and features.tickets.staff-role-ids.
What happens:

Ticket panel message is posted in Discord.
User opens ticket from dropdown.
Staff can claim/close from Discord.
Web dashboard can view/reply/close tickets.
Applications Flow
Enable features.applications.enabled: true.
Enable web (web.enabled: true).
Set discord.channels.applications.
Set publicip or web.public-host for external form URL.
What happens:

Discord button gives user a secure application link.
User submits form on /apply?token=....
Staff review in dashboard and approve/reject.
URL Resolution Priority
Public application/dashboard base URL is resolved in this order:

publicip
web.public-host
web.host + web.port
Frontend Development (Optional)
Web UI source:

web-ui/
Build web assets:

cd web-ui
npm install
npm run build
Assets output to:

src/main/resources/web
Troubleshooting
Bot not posting messages/panels:

Check token, channel IDs, feature toggles, bot permissions.
/verify works but player still restricted:

Check features.verification.enforce-link.* and bypass permission.
Application links are wrong:

Set publicip or web.public-host explicitly.
Web login fails:

Re-run /luxdiscord admin setup and use lowercase username.
