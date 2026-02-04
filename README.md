# Jin Woo Hunter - Universal CMS & Framework Credentials and Keys Hunter (Complete User Guide)

## What Is This Tool?

**Jin Woo Hunter** is an automated credential scanner that finds exposed sensitive information on websites. It checks for:
- 📧 **SMTP credentials** (email servers)
- ☁️ **AWS keys** (cloud service credentials)
- 🗄️ **Database credentials** (MySQL, PostgreSQL, etc.)
- 🔑 **API keys** (SendGrid, OpenAI, payment gateways, etc.)
- 💳 **Payment gateway keys** (Stripe, PayPal, Razorpay, etc.)

## How It Works

The scanner checks websites for exposed `.env` files and other configuration files that often contain sensitive credentials. It uses:
- 🎭 **Rotating User-Agents** (looks like different browsers)
- 🛡️ **Proxy support** (hide your IP)
- 🔄 **Automatic retries** (bypasses temporary blocks)
- 🚀 **Multi-threading** (scans faster)

---

## Three Scan Modes

### Option 1: Regular Scan (SMTP Only - Fast)

**Purpose**: Quickly find SMTP email credentials

**What It Scans For**: SMTP server credentials only

**Paths Checked**: 100+ paths including:
```
/.env
/.env.backup
/.env.production
/.env.dev
/.env.local
/.env.old
... and 90+ more variations
```

**What It Finds**:
- Mail host
- Mail port  
- Username
- Password
- From address

**Output File**: `Results/Random_SMTPS.txt`

**Format**:
```
mailhost|port|username|password
```

**Example**:
```
smtp.gmail.com|587|user@gmail.com|mypassword123
```

**Telegram Notification**:
```
🔍 SMTP Found
🌐 Host: smtp.gmail.com
🔌 Port: 587
📧 Email: user@gmail.com
🔐 Password: mypassword123
```

**Use Case**: When you only need email credentials quickly

---

### Option 2: Deep Scan (Everything - Thorough)

**Purpose**: Find ALL types of credentials and API keys

**What It Scans For**:
- ☁️ AWS credentials
- 🗄️ Database credentials  
- 🔑 Email API keys (SendGrid, Mailgun, Brevo, etc.)
- 🤖 AI API keys (OpenAI, Gemini, Anthropic, etc.)
- 🧩 Captcha service keys (2Captcha, CapMonster, etc.)
- 📡 Other API keys (Twilio, Telegram bots, etc.)

**Paths Checked**: Same 100+ paths as Regular Scan

**How It Works**:
1. Checks `/.env` first (GET request)
2. If not found, tries POST exploit
3. If still not found, scans all 100+ paths
4. For each `.env` found, scans the content AND JavaScript files

**Output Files**:

**AWS Credentials** → `Results/AWS/{region}.txt`
```
URL: http://example.com/.env
ACCESS_KEY: AKIAIOSFODNN7EXAMPLE
SECRET_KEY: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
REGION: us-east-1
```

**Database Credentials** → `Results/DATABASE/DATABASE.txt`
```
URL: http://example.com/.env
DB_HOST: db.example.com
DB_PORT: 3306
DB_NAME: production_db
DB_USER: admin
DB_PASS: secretpassword
```

**Email API Keys** → `Results/API_KEYS/*.txt`
```
URL: http://example.com
API_KEY: SG.abc123xyz...
DOMAIN: example.com
FROM_EMAIL: no-reply@example.com
```

**AI/Captcha Keys** → `Results/API_KEYS/*.txt`
```
sk-proj-abc123...  (OpenAI)
AIza...  (Google Gemini)
abc123def456...  (2Captcha)
```

**Telegram Notifications**:
```
☁️ AWS Keys Found
🔗 URL: http://example.com
🔑 Key: AKIA...
🌍 Region: us-east-1

🗄️ Database Found
🌐 Host: db.example.com
👤 User: admin
🔐 Pass: password123

🔑 SendGrid API Found
🔐 Key: SG.abc...
🌐 Domain: example.com
```

**Use Case**: When you want complete credential extraction

---

### Option 3: Payment Scan (Payment Gateways)

**Purpose**: Find payment gateway credentials

**What It Scans For**: 40+ payment providers including:
- Stripe
- PayPal
- Razorpay
- Square
- Braintree
- Authorize.net
- Cashfree
- Checkout.com
- And 30+ more

**Paths Checked** (10 paths):
```
/.env
/.env.example
/.env.local
/.env.production
/.env.dev
```

**What It Finds**:
- Stripe keys (live AND test)
- PayPal Client ID + Secret
- Razorpay Key + Secret
- And complete key pairs for all providers

**Output Files**: `Results/PAYMENT_KEYS/{provider}.txt`

**Format**:
```
URL: http://example.com/.env
STRIPE_KEY: sk_live_...
STRIPE_PUBLISHABLE_KEY: pk_live_...
```

**Telegram Notification**:
```
💳 Payment Keys Found: 3 Providers
[Files attached]
```

**Smart Filtering**: Automatically skips test/sandbox keys

**Use Case**: Specifically hunting payment credentials

---

## Special Features

### Wildcard Detection

**What It Is**: Some misconfigured servers return "200 OK" for ANY URL, even fake ones.

**What The Script Does**:
1. Tests with a random garbage URL (e.g., `/abc123xyz.php`)
2. If it returns 200 OK → Site is flagged as "wildcard"
3. Site is **skipped** (would give false positives)
4. **Saved to**: `Scan_Logs/wildcard_sites.txt`

**Why Skip?**: Can't tell real files from fake responses

**Can You Retry?**: YES! Use `wildcard_sites.txt` as input later

### HTTP Error Handling

Sites that return errors (403, 404, 500, etc.) are:
- Printed as `[SKIP]`
- **Saved to**: `Scan_Logs/wildcard_sites.txt`
- Can be retried later (might be temporarily blocked)

### Session Resume

If scan is interrupted (Ctrl+C):
- Progress is **auto-saved**
- **Next run**: Asked if you want to resume
- Continues from where it stopped

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

*Note: Takes you straight to the proxy check and then resumes if a session file is found.*

### WAF/Firewall Bypass

Built-in bypass techniques:
✅ Rotating User-Agents (20+ browsers)
✅ Referer headers (looks internal)
✅ Browser fingerprint headers (Sec-Fetch-*, DNT, etc.)
✅ Proxy rotation (if configured)
✅ Auto-retry on 403/429 blocks

---

## Requirements

### Files Needed

**1. proxies.txt** (Optional but recommended)
```
http://proxy1:port
http://user:pass@proxy2:port
socks5://proxy3:port
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

## Tips & Best Practices

### Retrying Blocked Sites

Many sites in `wildcard_sites.txt` might actually work:

**To Retry**:
1. Copy `Scan_Logs/wildcard_sites.txt` to `retry.txt`
2. Run scan again using `retry.txt` as input
3. Use different proxies if available
4. Try lower thread count

### When To Use Each Mode

**Use Option 1** (Regular) when:
- You only need SMTP/email credentials
- Speed is priority
- Scanning large lists (10,000+ sites)

**Use Option 2** (Deep) when:
- You want everything
- Quality over quantity
- Smaller targeted lists

**Use Option 3** (Payment) when:
- Specifically hunting payment gateways
- Need Stripe, PayPal, etc.
- E-commerce focused

### Improving Success Rate

1. **Use quality proxies** (residential > datacenter)
2. **Lower thread count** (less aggressive = less blocks)
3. **Retry failed sites** from `wildcard_sites.txt`
4. **Run at different times** (avoid rate limits resetting)

---

## FAQ

**Q: Why are so many sites skipped?**  
A: Could be:
- WAF/firewall blocking
- Site is down
- Wrong proxy IP
- Rate limiting

→ **Retry** from `wildcard_sites.txt` later

**Q: No results found, but site looks vulnerable?**  
A: Try:
- Manual check with browser
- Different scan mode
- Lower thread count
- Better proxies

**Q: Getting "Connection timeout" errors?**  
A: Increase timeout values or use better proxies

**Q: Can I scan without proxies?**  
A: Yes, but your IP will be exposed and might get blocked/rate-limited

**Q: How to add more paths to scan?**  
A: Edit the script's path list (requires Python knowledge)

---

## Output Examples

### Regular Scan Console
```
🚀 Regular SMTP Scan
📊 Targets: 100 | Threads: 10

[SKIP] http://site1.com (HTTP 403)
🎯 SMTP Found - http://site2.com/.env
[WILDCARD] http://site3.com (Soft 404)
🔍 No SMTP - http://site4.com
```

### Deep Scan Console
```
🔍 Deep Scan
📊 Targets: 50 | Threads: 5

🎯 Found [AWS, DB, API Keys] - http://site1.com/.env
[SKIP] http://site2.com (Dead/Unreachable)
🔍 No Credentials - http://site3.com
```

### Payment Scan Console
```
💳 Payment Keys Scanner
📊 Targets: 30 | Threads: 5
🔍 Scanning: 40+ Payment Gateways

[FOUND] Keys at http://site1.com/.env
[SCANNED] http://site2.com (No Keys Found)
```

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
