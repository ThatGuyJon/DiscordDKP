# DiscordDKP Bot
[![Buy me a Coffee](https://github.com/user-attachments/assets/39b6b3e4-2a35-4e8a-9fb4-99da59ea615a)](https://buymeacoffee.com/jonathangauthier)

A Discord bot to manage Raid Points (DKP) for guild members, including raid attendance tracking, DKP management, and an auction system.

## 1. Invite Bot to Server
1.  [Click to invite the bot](https://discord.com/oauth2/authorize?client_id=1448790882387234837&permissions=8&integration_type=0&scope=bot)
2.  Invite the bot to your server.

---

## 2. Installation & Setup

### Initial Discord Setup (Automated)
1.  **Run the Setup Wizard**: 
    -   In any channel, type `!setup`.
    -   The bot will check for required permissions permissions.
    -   It will create a category **DiscordDKP**.
    -   It will create/link a **Text Channel** and **Voice Channel** (names customizable).
    -   It will create the **DKP Admin** role if missing.
2.  **Manual Check**: Ensure you have the **DKP Admin** role to run admin commands.

---

## 3. Multi-Server Support
This bot is capable of running on multiple Discord servers simultaneously.
-   **Data Isolation**: DKP, Raids, and Auctions are strictly separated by server. (e.g., A user can have 50 DKP on Server A and 0 DKP on Server B).
-   **Independent Configuration**: Run `!setup` on each server to configure unique channels and roles.
-   **Safety**: The `!reset_dkp` command will **ONLY** wipe data for the specific server where it is executed.

---

## 4. Usage & Features

### Setup
-   `/setup`: Interactive wizard to configure channels and roles. (Server Owner Only)
-   `/reset_dkp`: **FACTORY RESET**. Wipes all data and deletes channels. (Server Owner Only)
-   `/leave_server`: **FACTORY RESET & LEAVE**. Wipes all data, deletes channels, and the bot leaves the server. (Server Owner Only)

### Admin Management
-   `/add_admin <user>`: Grants "DKP Admin" role. (Server Owner Only)
-   `/remove_admin <user>`: Revokes "DKP Admin" role. (Server Owner Only)

### Event Scheduling
-   `/schedule name:<name> recurring:<bool>`: Interactive UI to create a new raid event and post a signup form. Generates timezone-dynamic countdowns and automatically ping attendees 1 hour before start. Recurring events will seamlessly clone themselves for next week! (Must be used in `#sign-ups` channel).
-   `/cancel <uuid>`: Cancels an active event, locking its embed and removing all signup interactions. (Admin Only).

### DKP Management
-   `/help`: Displays a list of available commands.
-   `/dkp`: Check your current DKP balance (Ephemeral).
-   `/add_dkp <user> <amount> [reason]`: Add DKP to a user. (Admin Only)
-   `/remove_dkp <user> <amount> [reason]`: Remove DKP from a user. (Admin Only)
-   `/decay percentage:<int>`: Reduces all positive DKP balances in the server by `X%`. (Admin Only)
-   `/streaks enable:<bool> [req_raids] [bonus_pct]`: Activates consecutive raid streak bonuses to automatically reward consistent players during attendance ticks. (Admin Only)
-   `/check_dkp <user>`: Check another user's DKP. (Admin Only)
-   `/list_dkp`: Display a list of all users and points. (Admin Only)
-   `/dkp_history <user>`: View the audit log for a user. (Admin Only)
-   `/award_dkp <amount> [reason]`: Awards DKP to specific amount to everyone currently in the Raid Voice channel. (Admin Only)

### Raid System
-   `/startraid [name]`: Starts a new raid session.
    -   Must be used in `dkp-commands` channel.
    -   Requires at least one non-bot user in Raid Voice channel.
    -   **Automatically renames the Voice Channel** to `🔴 LIVE: [name]`.
    -   **Awards 5 DKP** immediately to everyone in "Raid Voice" (plus active Streak Bonuses if enabled).
    -   **Awards 5 DKP** every hour to everyone in "Raid Voice".
    -   Lists all users who received DKP.
-   `/stopraid`: Stops the session.
    -   **Reverts the Voice Channel name** back to normal.
    -   **Awards 5 DKP** bonus to everyone in "Raid Voice" (plus active Streak Bonuses).
    -   Lists all users who received the bonus.

### Auction System
-   `/bid <item_name> [minutes] [quantity]`: Starts an auction. Duration defaults to 1 minute.
    -   **Rules**:
        -   Main Spec > Off Spec.
        -   Winner pays **Second Highest Bid + 1 DKP**.
        -   **Multi-Item**: Top `N` bidders win. Pricing cascades (1st pays 2nd's bid, 2nd pays 3rd's bid, etc.).
        -   If only 1 bidder (or uncontested), cost is **1 DKP**.
        -   Ties are decided by random roll.
-   `/need <uuid> <reason>`: Applies for **Loot Council**. User must bid first to use this. If Admins approve via `#council`, the Council Winner intercepts the item and cleanly pays "Highest Bid + 1".
-   `/bid_end <auction_uuid>`: **CANCEL** an ongoing auction. (Admin Only)
    -   Immediately ends the auction.
    -   No DKP awarded, no winner declared.
    -   Updates the auction message to show "Cancelled".
-   `/bid_history <auction_uuid>`: Review winner, price, and all bids for a past auction.
        -   If only 1 bidder, cost is **1 DKP**.
        -   Ties are decided by random roll.
