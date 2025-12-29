# Security Guidelines

## 🔒 Protecting Your Credentials

This project requires sensitive information to function. **NEVER** commit real credentials to version control.

## Environment Variables

### Required Variables

Create a `.env` file in the project root (this file is gitignored):

```env
# MetaTrader 5 Account
MT5_ACCOUNT=your_account_number
MT5_PASSWORD=your_secure_password
MT5_SERVER=your_broker_server

# Azure ML (Optional)
AZURE_SUBSCRIPTION_ID=your_subscription_id
AZURE_RESOURCE_GROUP=your_resource_group
AZURE_WORKSPACE_NAME=your_workspace_name

# Trading Parameters
LOT_SIZE=0.01
RISK_PERCENTAGE=2
```

### Loading Environment Variables in Code

**Python Script**:
```python
import os
from dotenv import load_dotenv

# Load variables from .env file
load_dotenv()

# Access variables
account = int(os.getenv('MT5_ACCOUNT'))
password = os.getenv('MT5_PASSWORD')
server = os.getenv('MT5_SERVER')
lot_size = float(os.getenv('LOT_SIZE', '0.01'))
```

**Jupyter Notebook**:
```python
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# Configuration from environment
account = int(os.getenv('MT5_ACCOUNT'))
password = os.getenv('MT5_PASSWORD')
server = os.getenv('MT5_SERVER')
```

## Files to NEVER Commit

Add these to your `.gitignore` (already included):

### Sensitive Files
- `.env` - Your actual credentials
- `config.json` - If it contains real subscription IDs
- Any file with hardcoded passwords

### Trading Data
- `*.csv` - May contain account information
- `x_*.csv` - Historical data with timestamps
- `*.pkl` - Trained models may contain identifiable patterns

### Personal Files
- `outputs/` - Generated trading results
- `Screenshots/` - May show account balances
- `*.log` - May contain sensitive debugging info

## Before Pushing to GitHub

### 1. Scan for Credentials

Use tools to detect accidentally committed secrets:

```bash
# Install git-secrets
# https://github.com/awslabs/git-secrets

git secrets --scan
```

### 2. Manual Check

Search for common credential patterns:

```bash
# Windows PowerShell
Select-String -Path *.py,*.ipynb -Pattern "password|senha|account.*=.*[0-9]{8}"

# Linux/Mac
grep -r "password\|senha\|account.*=.*[0-9]" --include="*.py" --include="*.ipynb" .
```

### 3. Review Changes

Before committing:

```bash
git diff
git status
```

Look for:
- Hardcoded numbers (account IDs)
- Password strings
- API keys
- Server addresses (if sensitive)

## Sanitizing Existing Notebooks

If you've already hardcoded credentials, sanitize them:

### Example: Before (Bad)
```python
account = 12345678
password = "YourPassword123"
server = "YourBroker-MT5"
```

### Example: After (Good)
```python
# Load from environment variables
import os
from dotenv import load_dotenv

load_dotenv()

account = int(os.getenv('MT5_ACCOUNT'))
password = os.getenv('MT5_PASSWORD')
server = os.getenv('MT5_SERVER')

# Validate
if not all([account, password, server]):
    raise ValueError("Missing required environment variables. Check your .env file.")
```

## Removing Sensitive Data from Git History

If you accidentally committed credentials:

### Option 1: BFG Repo-Cleaner (Recommended)

```bash
# Download BFG
# https://rtyley.github.io/bfg-repo-cleaner/

# Replace passwords
java -jar bfg.jar --replace-text passwords.txt

# Clean up
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```

### Option 2: git-filter-branch

```bash
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/sensitive/file" \
  --prune-empty --tag-name-filter cat -- --all
```

### Option 3: Start Fresh

If early in development:

```bash
# Remove git history
rm -rf .git

# Sanitize files, then re-initialize
git init
git add .
git commit -m "Initial commit (sanitized)"
```

## Production Deployment

### For VPS/Cloud Deployment

**Use environment variables or secrets management**:

```bash
# Set environment variables
export MT5_ACCOUNT=50012345
export MT5_PASSWORD="YourSecurePassword"
export MT5_SERVER="YourBroker-MT5"

# Run bot
python utils/codex.py --symbol XAUUSD
```

### For Docker

**Create a docker-compose.yml**:

```yaml
version: '3.8'
services:
  trading-bot:
    build: .
    environment:
      - MT5_ACCOUNT=${MT5_ACCOUNT}
      - MT5_PASSWORD=${MT5_PASSWORD}
      - MT5_SERVER=${MT5_SERVER}
    env_file:
      - .env
```

### For Azure

Use Azure Key Vault:

```python
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

credential = DefaultAzureCredential()
client = SecretClient(vault_url="https://your-vault.vault.azure.net/", credential=credential)

account = client.get_secret("MT5-ACCOUNT").value
password = client.get_secret("MT5-PASSWORD").value
server = client.get_secret("MT5-SERVER").value
```

## Two-Factor Authentication

If your broker supports 2FA, **keep it enabled**. Some implementations:

```python
import pyotp

# TOTP 2FA
totp_secret = os.getenv('MT5_TOTP_SECRET')
totp = pyotp.TOTP(totp_secret)
two_factor_code = totp.now()

# Use in login
# (Implementation depends on broker API)
```

## Credential Rotation

Regularly update passwords:

1. Change password in MT5
2. Update `.env` file
3. Restart bot
4. Verify connection

## Security Checklist

Before making repository public:

- [ ] No hardcoded passwords in any file
- [ ] `.env` file is in `.gitignore`
- [ ] All notebooks use environment variables
- [ ] No account numbers visible in code
- [ ] No real trading data in repository
- [ ] No screenshots with sensitive info
- [ ] Scanned for secrets with automated tools
- [ ] Reviewed git history for leaks
- [ ] Updated README with security warnings

## Reporting Security Issues

If you find a security vulnerability:

1. **DO NOT** open a public issue
2. Email details to: [your-security-email]
3. Include:
   - Description of vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

## Additional Resources

- [Git Secrets Tool](https://github.com/awslabs/git-secrets)
- [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [Python Security Best Practices](https://python.readthedocs.io/en/latest/library/secrets.html)

---

**Remember**: Once credentials are pushed to a public repository, consider them compromised. Change them immediately.

