# Jin Woo Hunter - Universal CMS & Framework Credentials and Keys Hunter (Complete User Guide)

## What Is This Tool?

**Jin Woo Hunter** is an automated credential scanner that finds exposed sensitive information on websites. It checks for:
- 📧 **SMTP Credentials** (email servers)
- ☁️ **AWS keys** (cloud service credentials)
- 🗄️ **Database** (MySQL, PostgreSQL, etc.)
- 💬 **SMS Keys** - (Twilio, Nexmo, 5sim, etc.) 
- 🔑 **API keys** (SendGrid, OpenAI, payment gateways, etc.)
- 💳 **Payment gateway keys** (Stripe, PayPal, Razorpay, etc.)

## How It Works

The scanner checks websites for exposed `.env` files and other configuration files that often contain sensitive credentials. It uses:
- 🎭 **Rotating User-Agents** (looks like different browsers)
- 🛡️ **Proxy support** (hide your IP)
- 🔄 **Automatic retries** (bypasses temporary blocks)
- 🚀 **Multi-threading** (scans faster)
- 💾 **Auto-Saving Sessions** (separately for each mode)

---

## Two Scan Modes

### Option 1: Deep Scan (All Credentials - Thorough)

**Purpose**: The ultimate scan mode using revolutionary 3-Phase scanning. Finds **EVERYTHING** including SMTP, Database, AWS, API keys, and MORE.

**🎯 How It Works - 3-Phase System**:

**Phase 1: ENV & Config Scanning (Fast Path)**
**Phase 2: Deep Key Discovery (Comprehensive)**
**Phase 3: Advanced Fallback (Thorough)**

**📊 Live Progress Indicators**:
```
[CHECKING] http://example.com
[PHASE 1] ENV scanning...
[ENV-FILE] http://example.com/.env
[ENV-EXTRACT] Extracting all credentials...
[AWS-CHECK] Validating AKIA...
[PHASE 2] Scanning Keys, JS, and Directories...
[JS-SCAN] Found 5 JS files to scan
[DIR-SCAN] Scanning directories...
[PHASE 3] Deep Fallback Analysis...
[CRAWLER] Crawling http://example.com (depth=2)...
[DB-DUMPS] Scanning for database dumps...
[VULN-SCAN] Checking for vulnerabilities...
```

**What It Scans For**:
- 📧 **SMTP Credentials** (Gmail, Office365, SendGrid, etc.)
- ☁️ **AWS Credentials** (Access Key + Secret + SES SMTP)
- 🗄️ **Database Credentials** (MySQL, Postgres, etc. with all fields)
- 🔑 **Email API Keys** (SendGrid, Mailgun, Brevo, Postmark, etc.)
- 🤖 **AI API Keys** (OpenAI, Gemini, Anthropic, Claude)
- 🧩 **Captcha Keys** (2Captcha, CapMonster, AntiCaptcha, Capsolver)
- 💬 **SMS Keys** (Twilio, Vonage, 5sim, SMS-Activate, Nexmo)

**Paths Checked**: 300+ paths including:
```
/.env, /.env.backup, /.env.production, /.env.local
/config.php, /config.json, /web.config
/wp-config.php, /database.php
JavaScript files (automatically extracted)
/admin/, /phpmyadmin/, /cpanel/
/database.sql, /dump.sql, /backup.zip
... and 290+ more
```

**Output Files (Organized Folders)**:

**SMTP Credentials** → `Results/SMTP/{provider}.txt`
- `Results/SMTP/gmail.txt`
- `Results/SMTP/office.txt`
- `Results/SMTP/sendgrid.txt`
- `Results/SMTP/random_smtps.txt` (for others)

**AWS Credentials** → `Results/AWS/{region}.txt`
```
URL: http://example.com/.env
ACCESS_KEY: AKIA...
SECRET_KEY: wJalr...
REGION: us-east-1
```

**Database Credentials** →  `Results/DATABASE/database.txt`
```
URL: http://74.208.223.14/.env
DB_HOST: db5014281989.hosting-data.io
DB_PORT: 3306
DB_NAME: dbs11883978
DB_USER: dbu1634093
DB_PASS: S4_DrL-CkL(5G)1963SurF
```

---

### Option 2: Payment Merchant Scan

**Purpose**: Specifically hunt for payment gateway credentials only.

**What It Scans For**: 40+ payment providers including:
- Stripe (Live Keys)
- PayPal
- Razorpay
- Square
- Braintree
- Authorize.net
- Cashfree
- Checkout.com
- And 30+ more

**Paths Checked** (Optimized list):
```
/.env
/.env.example
/.env.local
/.env.production
/.env.dev
```

**What It Finds**:
- Stripe Keys (sk_live_...)
- PayPal Client ID + Secret
- Razorpay Key + Secret
- **Smart Filtering**: Automatically skips test/sandbox keys.

**Output Files**: `Results/PAYMENT_KEYS/{provider}.txt`
```
URL: http://example.com/.env
STRIPE_KEY: sk_live_...
```

**Use Case**: E-commerce focused hunting.

---

## 🆕 What's New in v3.0

### 🎯 3-Phase Scanning System
### 📊 Live Progress Indicators
### 🚫 Smart Duplicate Prevention
### 🧹 Enhanced Fake Key Filtering
### 💾 Complete .env Extraction
### 📦 300+ More Scanning Paths

---

## Special Features

### Separate Sessions
Each scan mode has its own session file, so you can run them independently without losing progress:
- **Deep Scan**: Saves to `.jinwoohunter_deep_session`
- **Payment Scan**: Saves to `.jinwoohunter_payment_session`

If you stop a scan (Ctrl+C), it auto-saves. When you restart with `--restore`, it resumes the correct session for the mode you select.

#### How to Restore
To resume a saved session, you need to use the `--restore` flag.

**For Powershell Executable (.exe):**
```powershell
.\Jin Woo Hunter.exe --restore
```

**For CMD Executable (.exe):**
```cmd
"Jin Woo Hunter.exe" --restore
```

### Telegram Notifications (Anti-Spam)
- **Alerts**: Sent immediately when a key is found.
- **Documents**: Result files are **queued** and sent in a single batch when:
  1. The scan completes
  2. You stop the scan (Ctrl+C)
  3. The terminal closes

### Wildcard Detection
Some servers return "200 OK" for everything. The script detects this by checking a random garbage URL. If it returns 200, the site is skipped to prevent false positives and logged to `Scan_Logs/wildcard_sites.txt`.

### WAF/Firewall Bypass
Built-in bypass techniques:
✅ Rotating User-Agents (20+ browsers)
✅ Referer headers (looks like internal traffic)
✅ Browser fingerprint headers (Sec-Fetch-*, DNT)
✅ Auto-retry on 403/429 blocks

---

## Requirements

### Files Needed

**1. proxies.txt** (Optional but recommended)
```
http://proxy:port
http://user:pass@proxy:port
```

**2. targets.txt** (Your scan list)
```
http://site1.com
https://site2.com
site3.com
```

**3. telegram_config.txt** (Optional - for notifications)
```
{bot_token}
{chat_id}
```

---

## FAQ

**Q: Why are only 2 modes now?**
A: The old "Regular Scan" (SMTP only) was merged into **Deep Scan**. Now Deep Scan finds SMTPs *plus* everything else, so you don't need a separate mode.

**Q: Where are my SMTP results?**
A: Look in `Results/SMTP/` folder. They are now sorted by provider (e.g., `gmail.txt`, `office.txt`).

**Q: Can I run both modes at once?**
A: Yes! Since they use separate session files, you can run two instances (one Deep, one Payment) and they will save/resume correctly.

**Q: Why are so many sites skipped?**  
A: Likely WAF blocking or dead sites. Check `Scan_Logs/wildcard_sites.txt` and try retrying them later with different proxies.

**Q: Can I scan without proxies?**  
A: Yes, but your IP will be exposed and you will get blocked quickly.

---

## Troubleshooting

### "FileNotFoundError: targets.txt"
→ Create `targets.txt` file with your targets

### All sites showing [SKIP]
→ Issue with proxies or IP being blocked
→ Try without proxies as a test

### No Telegram notifications
→ Check `telegram_config.txt` format
→ Verify bot token and chat ID

---

## Legal Disclaimer

This tool is for **educational purposes** and **authorized security testing only**.

- ✅ Use on your own websites
- ✅ Use with written permission
- ❌ Do NOT use on websites without authorization

**Unauthorized access to computer systems is illegal.**
