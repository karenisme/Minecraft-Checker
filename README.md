# MSMC — Minecraft Account Checker

```
██╗  ██╗ █████╗ ██████╗ ███████╗███╗   ██╗██╗███████╗███╗   ███╗███████╗
██║ ██╔╝██╔══██╗██╔══██╗██╔════╝████╗  ██║██║██╔════╝████╗ ████║██╔════╝
█████╔╝ ███████║██████╔╝█████╗  ██╔██╗ ██║██║███████╗██╔████╔██║█████╗  
██╔═██╗ ██╔══██║██╔══██╗██╔══╝  ██║╚██╗██║██║╚════██║██║╚██╔╝██║██╔══╝  
██║  ██╗██║  ██║██║  ██║███████╗██║ ╚████║██║███████║██║ ╚═╝ ██║███████╗
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝╚═╝╚══════╝╚═╝     ╚═╝╚══════╝
```

> A powerful, multi-threaded Microsoft/Minecraft account checker with rich capture support including Hypixel stats, Optifine capes, email access, ban status, name change availability, and Discord webhook notifications.

---

## ✨ Features

- ✅ Microsoft account authentication via Xbox Live OAuth flow
- 🎮 Minecraft ownership detection: `Normal`, `Xbox Game Pass`, `Xbox Game Pass Ultimate`, `Other` (Bedrock, Legends, Dungeons)
- 📧 Valid mail detection (accounts with no Minecraft but valid Microsoft login)
- 🔐 2FA / locked account detection
- 🌐 Hypixel stats capture: name, level, first/last login, Bedwars stars, Skyblock coins, **ban status** (via direct server connection)
- 🎨 Optifine cape detection
- 📬 Email full-access check (SFA / MFA)
- 🔄 Name change availability & last name change date
- 🪝 Discord webhook notifications per hit with fully customizable message template
- 🔁 Proxy support: HTTP, SOCKS4, SOCKS5, proxyless, or **auto-scrape** from public sources
- ⚡ Multi-threaded with configurable thread count
- 📊 Two display modes: **CUI** (live stats dashboard) or **Log** (per-combo output)
- 📁 Auto-organized result files by category

---

## 📁 Project Structure

```
├── main.py
├── config.ini              # Auto-generated on first run
└── results/
    └── {combo_filename}/
        ├── Hits.txt
        ├── Capture.txt
        ├── Bad.txt (implicit — bad combos not written)
        ├── 2fa.txt
        ├── Valid_Mail.txt
        ├── XboxGamePass.txt
        ├── XboxGamePassUltimate.txt
        ├── Other.txt
        ├── SFA.txt
        ├── MFA.txt
        ├── Banned.txt
        ├── Unbanned.txt
        └── Capture.txt
```

---

## ⚙️ Configuration

`config.ini` is **auto-generated** on first run. You can edit it afterward.

### `[Settings]`

| Key | Description | Default |
|-----|-------------|---------|
| `Webhook` | Discord webhook URL for hit notifications | *(paste your webhook)* |
| `Max Retries` | Max retry attempts per combo before marking bad | `5` |
| `Proxyless Ban Check` | Use no proxy for Hypixel ban checking | `False` |
| `WebhookMessage` | Fully customizable Discord message template | *(see below)* |

### `[Scraper]`

| Key | Description | Default |
|-----|-------------|---------|
| `Auto Scrape Minutes` | How often to re-scrape proxies (if using auto scraper) | `5` |

### `[Captures]`

All toggles are `True` / `False`:

| Key | Description |
|-----|-------------|
| `Hypixel Name` | Fetch Hypixel username/status |
| `Hypixel Level` | Fetch Hypixel network level |
| `First Hypixel Login` | Fetch first Hypixel login date |
| `Last Hypixel Login` | Fetch last Hypixel login date |
| `Optifine Cape` | Check if account has an Optifine cape |
| `Minecraft Capes` | List all MC capes on the account |
| `Email Access` | Check if email login is accessible (SFA/MFA) |
| `Hypixel Skyblock Coins` | Fetch Skyblock net worth |
| `Hypixel Bedwars Stars` | Fetch Bedwars star count |
| `Hypixel Ban` | Connect to Hypixel alpha server to check ban status |
| `Name Change Availability` | Check if name change is allowed |
| `Last Name Change` | Fetch date of last name change |

---

## 🪝 Webhook Message Template

The `WebhookMessage` field in `config.ini` supports the following placeholders:

| Placeholder | Value |
|-------------|-------|
| `<email>` | Account email |
| `<password>` | Account password |
| `<n>` | Minecraft username |
| `<type>` | Account type (Normal, XGP, etc.) |
| `<hypixel>` | Hypixel description |
| `<level>` | Hypixel level |
| `<firstlogin>` | First Hypixel login |
| `<lastlogin>` | Last Hypixel login |
| `<ofcape>` | Optifine cape (Yes/No) |
| `<capes>` | All MC capes |
| `<access>` | Email access (True/False) |
| `<skyblockcoins>` | Skyblock net worth |
| `<bedwarsstars>` | Bedwars stars |
| `<banned>` | Hypixel ban status |
| `<namechange>` | Name change allowed (True/False) |
| `<lastchanged>` | Last name change date |

---

## 🚀 Installation & Usage

### Requirements

- Python 3.8+

### Install dependencies

```bash
pip install requests colorama console readchar urllib3 pysocks minecraft-launcher-lib
```

> **Note:** The `minecraft` package used for ban checking is `mcstatus` or a pyCraft-based library. Make sure `minecraft.networking` is available.

### Run

```bash
python main.py
```

---

## 🖥️ Startup Flow

When launched, the tool will prompt you through the following steps interactively:

1. **Threads** — Enter number of concurrent threads (recommended: `100`, max `5` if proxyless)
2. **Proxy Type** — Choose one:
   - `[1]` HTTP/S
   - `[2]` SOCKS4
   - `[3]` SOCKS5
   - `[4]` None (proxyless)
   - `[5]` Auto Scraper (fetches free proxies automatically)
3. **Screen Mode** — Choose one:
   - `[1]` CUI — Live stats dashboard
   - `[2]` Log — Per-combo colored output
4. **Load Combo File** — File picker dialog (`.txt`, `email:password` format)
5. **Load Proxy File** — File picker dialog (if not proxyless or auto-scrape)
6. **Load Ban-Check SOCKS5 Proxies** — Separate proxy file for Hypixel ban checking (if enabled)

---

## 📊 Console Output (Log Mode)

| Color | Status |
|-------|--------|
| 🟢 Green | Hit (valid Minecraft account) |
| 🔴 Red | Bad combo |
| 🟣 Magenta | 2FA / locked account |
| 🟡 Yellow | Other (Bedrock/Dungeons/Legends) |
| 🔵 Light Blue | Valid Mail (Microsoft account, no MC) |
| 🟢 Light Green | Xbox Game Pass / Ultimate |

---

## 📤 Output Files

| File | Contents |
|------|----------|
| `Hits.txt` | Valid Minecraft accounts (`email:password`) |
| `Capture.txt` | Full capture details for all hits |
| `2fa.txt` | Accounts with 2FA or identity verification |
| `Valid_Mail.txt` | Valid Microsoft login, no Minecraft |
| `XboxGamePass.txt` | Xbox Game Pass accounts |
| `XboxGamePassUltimate.txt` | Xbox Game Pass Ultimate accounts |
| `Other.txt` | Bedrock / Legends / Dungeons only |
| `SFA.txt` | No email access (Single Factor Auth) |
| `MFA.txt` | Email accessible (Multi Factor Auth) |
| `Banned.txt` | Hypixel-banned accounts |
| `Unbanned.txt` | Hypixel-clean accounts |

---

## ⚠️ Disclaimer

This tool is provided for **educational purposes only**. Using this tool may violate [Microsoft's Terms of Service](https://www.microsoft.com/en-us/servicesagreement) and [Mojang's Terms of Service](https://www.minecraft.net/en-us/eula). The author is not responsible for any misuse or consequences arising from the use of this software. Use at your own risk.

---

## 👤 Author

**KillinMachine** / **fufuyaunn**
- Telegram: [@swllette](https://t.me/swllette)
- Website: [karenhoyoshi.asia](https://karenhoyoshi.asia)
- Discord: `fufuyaunn`
