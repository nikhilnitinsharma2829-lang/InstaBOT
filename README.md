<div align="center">

```
███╗   ██╗███████╗ ██████╗ ██╗  ██╗███████╗██╗  ██╗
████╗  ██║██╔════╝██╔═══██╗██║ ██╔╝██╔════╝╚██╗██╔╝
██╔██╗ ██║█████╗  ██║   ██║█████╔╝ █████╗   ╚███╔╝
██║╚██╗██║██╔══╝  ██║   ██║██╔═██╗ ██╔══╝   ██╔██╗
██║ ╚████║███████╗╚██████╔╝██║  ██╗███████╗██╔╝ ██╗
╚═╝  ╚═══╝╚══════╝ ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝

          ██╗██████╗  ██████╗ ████████╗    ██╗   ██╗ ██╗
          ██║██╔══██╗██╔═══██╗╚══██╔══╝    ██║   ██║███║
          ██║██████╔╝██║   ██║   ██║       ██║   ██║╚██║
          ██║██╔══██╗██║   ██║   ██║       ╚██╗ ██╔╝ ██║
          ██║██████╔╝╚██████╔╝   ██║        ╚████╔╝  ██║
          ╚═╝╚═════╝  ╚═════╝    ╚═╝         ╚═══╝   ╚═╝
```

![Version](https://img.shields.io/badge/version-1.0.0-blueviolet?style=for-the-badge)
![Platform](https://img.shields.io/badge/platform-Instagram-E1306C?style=for-the-badge&logo=instagram)
![Node](https://img.shields.io/badge/node-%3E%3D18-brightgreen?style=for-the-badge&logo=node.js)
![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)
![Commands](https://img.shields.io/badge/commands-33-orange?style=for-the-badge)
![Events](https://img.shields.io/badge/events-6-yellow?style=for-the-badge)

**A highly advanced Instagram chatbot built on `@neoaz07/nkxica` — modular, role-based, and production-ready.**

</div>

---

## ✨ Highlights

- **MQTT-based** real-time message listening via Instagram's private API
- **33 commands** across 7 categories with aliases and cooldowns
- **4-tier role system** — user → bot admin → premium → developer
- **Auto-reply** — every bot response threads back to the triggering message
- **Reaction feedback** — visual ⏳ / ✅ / ❌ / ✨ reactions instead of status spam
- **Meta AI integration** — multi-turn conversations with image support
- **Economy system** — persistent coin balances with daily rewards
- **Spam protection** — auto-ban threads that exceed command thresholds
- **SQLite / MongoDB** database with auto-save
- **Dynamic command loader** — load, unload, reload commands at runtime

---

## 🚀 Quick Start

### 1. Install dependencies
```bash
npm install
```

### 2. Add your Instagram cookies
Export your Instagram session cookies in **Netscape format** using a browser extension (e.g. *Get cookies.txt LOCALLY*) and save them to `account.txt`.

Minimum required cookies: `sessionid`, `csrftoken`

### 3. Configure
Edit `config/default.json` — set your admin IDs, prefix, timezone, etc.

```jsonc
{
  "prefix": "~",
  "noPrefix": true,          // admins & devs skip the prefix
  "adminBot": ["YOUR_USER_ID"],
  "devUsers": ["YOUR_USER_ID"],
  "nickNameBot": "InstaBOT"
}
```

### 4. Run
```bash
node index.js
```

---

## 📁 Project Structure

```
.
├── index.js                  ← Entry point
├── account.txt               ← Instagram cookies (Netscape format) — keep secret!
├── config/
│   ├── default.json          ← Main configuration file
│   └── index.js              ← Config loader (merges env vars + JSON)
├── bot/
│   └── InstagramBot.js       ← Core bot engine (login, api wrapper, loaders)
├── commands/                 ← Command modules (33 files)
├── events/                   ← Event handlers (6 files)
├── utils/
│   ├── banner.js             ← Startup banner & logging helpers
│   ├── logger.js             ← Winston logger
│   └── permissions.js        ← Role resolution logic
└── storage/
    ├── data/bot.sqlite        ← SQLite database
    └── logs/                 ← Log files (combined + error)
```

---

## 🔐 Role System

| Role | Name | Who |
|------|------|-----|
| `0` | Everyone | Any user |
| `2` | Bot Admin | IDs in `adminBot` config |
| `3` | Premium User | IDs in `premiumUsers` config |
| `4` | Developer | IDs in `devUsers` config — full access |

> Developers bypass all role checks up to level 3. Bot Admins cannot access role-4 commands.

---

## 📜 Commands

### 🤖 AI
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `ai` | `gpt`, `ask`, `chatgpt` | 0 | 10s | Ask AI anything (OpenAI) |
| `metaai` | `meta`, `llama` | 0 | — | Multi-turn Meta AI chat with image support. `metaai clear` resets history |

### 🎮 Fun
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `8ball` | `magic8ball`, `eightball`, `fortune` | 0 | 3s | Ask the magic 8-ball a yes/no question |
| `anisearch` | `anime`, `animeedit` | 0 | 10s | Search and send an anime edit video |
| `choose` | `pick`, `select`, `random` | 0 | 3s | Randomly choose between options separated by `\|` |
| `joke` | `j`, `funny`, `laugh` | 0 | 5s | Get a random joke |
| `quote` | `q`, `inspiration`, `motivate` | 0 | 5s | Get a random inspirational quote |
| `rps` | `rockpaperscissors`, `rock` | 0 | 3s | Play Rock Paper Scissors with the bot |

### 🎲 Games
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `coinflip` | `flip`, `coin`, `toss` | 0 | 2s | Flip a coin — Heads or Tails |
| `dice` | `roll`, `d6`, `rolldice` | 0 | 2s | Roll a dice (default 6-sided, or custom) |

### 🛠️ Utility
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `calc` | `calculate`, `math` | 0 | 3s | Evaluate a mathematical expression |
| `echo` | `say`, `repeat` | 0 | 3s | Repeat your message |
| `remind` | `reminder`, `remindme` | 0 | 5s | Set a reminder (bot DMs you after the delay) |
| `time` | `clock`, `datetime`, `worldtime` | 0 | 3s | Get current time in any timezone |
| `uid` | `userid`, `getuid`, `id` | 0 | 5s | Resolve an Instagram username → numeric User ID |
| `userinfo` | `uinfo`, `profile`, `iginfo` | 0 | 10s | Full Instagram profile info for a username |

### ℹ️ System
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `credits` | `author`, `creator` | 0 | 5s | Show bot credits |
| `help` | `menu`, `commands`, `h` | 0 | 3s | List all commands or get help for one |
| `info` | `about`, `botinfo` | 0 | 5s | Show bot info (version, uptime, stats) |
| `ping` | `p` | 0 | 3s | Check bot response latency |
| `stats` | `statistics`, `botstats` | 0 | 5s | View bot statistics |
| `dev` | `developer`, `owner` | 4 | — | Developer panel — system controls |
| `restart` | `reboot`, `reload` | 4 | — | Restart the bot process |

### 💰 Economy
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `economy` | `bal`, `balance`, `daily`, `pay`, `wallet` | 0 | 3s | Check balance · claim daily · transfer coins |

### 🛡️ Admin
| Command | Aliases | Role | Cooldown | Description |
|---------|---------|------|----------|-------------|
| `admin` | `botadmin`, `admins` | 2 | 5s | Add / remove / list bot admins |
| `ban` | `unban`, `blacklist` | 2 | 3s | Ban or unban a user from the bot |
| `cmd` | `command` | 2 | 5s | Load / unload / reload / install command files |
| `manage` | `autoresponse`, `trigger` | 2 | 5s | Manage auto-response triggers |
| `prefix` | `setprefix`, `changeprefix` | 2 | 3s | Change the prefix for this thread or globally |
| `selflisten` | `selfmode`, `listenself` | 2 | 3s | Toggle whether the bot listens to its own messages |
| `thread` | `gc`, `group` | 2 | 3s | Thread settings — info, ban, unban, prefix |
| `unsend` | `delete`, `remove`, `del` | 0 | 3s | Unsend a message (reply to the target message) |
| `whitelist` | `wl` | 2 | 3s | Manage user / thread whitelist |

---

## 📡 Events

| Event | Trigger | Description |
|-------|---------|-------------|
| `message` | Every incoming message | Main message router — runs prefix/no-prefix detection, spam protection, permission checks, and dispatches to commands |
| `ready` | Bot connects | Logs successful connection and prints startup info |
| `bot_added` | Bot is added to a group | Sends an introduction message to the new group |
| `gc_join` | User joins a group | Sends a welcome message |
| `gc_leave` | User leaves a group | Sends a farewell message |
| `error` | Any bot error | Logs and handles runtime errors gracefully |

---

## 🔧 Adding a Command

Create a new file in `commands/`:

```javascript
module.exports = {
  name: 'hello',
  aliases: ['hi', 'hey'],
  description: 'Say hello back',
  usage: 'hello [name]',
  cooldown: 3,
  role: 0,          // 0=all  2=admin  3=premium  4=dev
  author: 'YourName',
  category: 'fun',

  async run({ api, event, args, bot }) {
    const name = args[0] || 'friend';
    await api.sendMessage(`Hello, ${name}!`, event.threadId);
  }
};
```

**Available `api` methods:**

| Method | Description |
|--------|-------------|
| `sendMessage(text, threadId)` | Send a text message (auto-replies to triggering message) |
| `sendPhotoFromUrl(url, threadId, caption?)` | Send a photo from a URL |
| `sendVideoFromUrl(url, threadId, caption?)` | Send a video from a URL |
| `sendVoiceFromUrl(url, threadId)` | Send a voice note from a URL |
| `sendReaction(emoji, messageId)` | React to a message |
| `replyToMessage(threadId, text, messageId)` | Explicit threaded reply |
| `unsendMessage(threadId, messageId)` | Delete a message |
| `getUserInfo(userId)` | Fetch Instagram profile info |
| `getUidFromUsername(username)` | Resolve username → UID |

---

## 🎯 Adding an Event

Create a new file in `events/`:

```javascript
module.exports = {
  name: 'message_reaction',   // must match the MQTT event type
  description: 'Handle reactions on messages',

  async run({ api, event, bot }) {
    // event contains: threadId, senderId, messageId, reaction, ...
  }
};
```

---

## ⚙️ Configuration Reference

Key fields in `config/default.json`:

| Field | Default | Description |
|-------|---------|-------------|
| `prefix` | `~` | Command prefix |
| `noPrefix` | `true` | Admins & devs can skip the prefix |
| `adminBot` | `[]` | Array of bot admin user IDs |
| `premiumUsers` | `[]` | Array of premium user IDs |
| `devUsers` | `[]` | Array of developer user IDs |
| `antiInbox` | `false` | Ignore DMs (group-only mode) |
| `database.type` | `sqlite` | `sqlite` or `mongodb` |
| `spamProtection.commandThreshold` | `8` | Commands before auto-ban |
| `spamProtection.timeWindow` | `10` | Time window in seconds |
| `typingIndicator.enable` | `true` | Show typing before responses |
| `adminOnly.enable` | `false` | Restrict bot to admins only |

---

## 💎 Credits

<table>
<tr>
<td align="center"><b>Role</b></td>
<td align="center"><b>Name</b></td>
<td align="center"><b>Contribution</b></td>
</tr>
<tr>
<td>Author & Lead Developer</td>
<td><b>NeoKEX</b></td>
<td>Core bot engine, command system, event system, role system, database layer</td>
</tr>
<tr>
<td>Contributor</td>
<td><b>Vex_kshitiz</b></td>
<td><code>anisearch</code> command — TikTok anime edit video search</td>
</tr>
<tr>
<td>API</td>
<td><b>@neoaz07/nkxica</b></td>
<td>Instagram MQTT client (unofficial private API wrapper)</td>
</tr>
</table>

> **DO NOT remove or modify credits.** This project is protected by copyright. Unauthorized redistribution or credit removal may result in legal action.

---

## ⚠️ Disclaimer

This bot uses Instagram's **unofficial private API**. Instagram's Terms of Service prohibit automated access. Use at your own risk — the author is not responsible for account bans or restrictions.

---

<div align="center">
Made with ❤️ by <b>NeoKEX</b> &nbsp;·&nbsp; <a href="https://github.com/NeoKEX">github.com/NeoKEX</a>
</div>
