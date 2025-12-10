# 🚨 Security Incident: Exposed API Key in Git History

## Critical Issue

**Severity**: 🔴 **HIGH**  
**Status**: **Action Required**  
**Date Discovered**: December 10, 2025

### What Was Found

A hardcoded NCBI API key was committed to your GitHub repository:

```
API_KEY = "9446fd13127051ca7343c2018204779d8007"
```

**Location**: 
- Repository: `zajacnj/Python-Rev2`
- File: `pubmed_extract-0.1/pubmed_extract-0.1/pubmed_extract/extraction.py`
- Commit: `40000a30fd315513cb8d7486b07c30aff9f520fe`
- Branch: `main` (default branch)

**Exposure Level**: 🔴 **PUBLIC** - Anyone with access to GitHub can see this key

---

## Immediate Actions (Do These NOW)

### 1. ✅ Revoke the Exposed API Key

**Option A: Online (Recommended)**
1. Go to: https://www.ncbi.nlm.nih.gov/account/settings/
2. Log in to your NCBI account
3. Find "API Key Management" or "API Settings"
4. **DELETE or DISABLE** the key: `9446fd13127051ca7343c2018204779d8007`
5. Generate a **NEW API key**

**Option B: Contact NCBI Support**
- Email: NCBI Support
- Subject: "Revoke compromised API key"
- Key to revoke: `9446fd13127051ca7343c2018204779d8007`

**⚠️ CRITICAL**: Do NOT skip this step. The key is now public and could be misused.

### 2. ✅ Create .gitignore to Prevent Future Exposures

Create or update `.gitignore` in repository root:

```bash
# In: C:\Users\VHATVHZajacN\OneDrive - Department of Veterans Affairs\Desktop\AI Projects\GRECC\Python Rev2\.gitignore

# Environment variables (contains secrets)
.env
.env.local
.env.*.local
.env.*.secret

# API Keys and credentials (should not be committed)
*.key
*.pem
*.pfx
credentials.json
config.json

# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Virtual environments
venv/
ENV/
env/
.venv

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log
logs/
```

### 3. ✅ Remove API Key from Git History

This removes the key from all past commits:

**PowerShell Command** (Run in your repo):

```powershell
cd 'C:\Users\VHATVHZajacN\OneDrive - Department of Veterans Affairs\Desktop\AI Projects\GRECC\Python Rev2'

# Remove the exposed key from git history
git filter-branch --tree-filter 'find . -name "*.py" -type f -exec sed -i "s/9446fd13127051ca7343c2018204779d8007/REDACTED/g" {} \;' HEAD

# Force push to GitHub (overwrites remote history)
git push origin main --force
```

**⚠️ WARNING**: `--force` push rewrites history. If others are using this repo, coordinate with them first.

### 4. ✅ Update Code to Use Environment Variable

**Find all Python files with hardcoded API key:**

```powershell
Get-ChildItem -Recurse -Include "*.py" | Select-String "9446fd13127051ca7343c2018204779d8007"
```

**Replace with environment variable:**

Instead of:
```python
API_KEY = "9446fd13127051ca7343c2018204779d8007"
```

Use:
```python
from dotenv import load_dotenv
import os

load_dotenv()
API_KEY = os.getenv("NCBI_API_KEY")

if not API_KEY:
    raise ValueError("NCBI_API_KEY not set in .env file")
```

---

## Step-by-Step Remediation

### Phase 1: Immediate (Within 1 hour)
- [ ] **Revoke the exposed API key** via NCBI account
- [ ] Generate a **NEW API key** from NCBI
- [ ] Verify old key is disabled

### Phase 2: Code Fix (Within 2 hours)
- [ ] Update all `extraction.py` files to use `os.getenv()`
- [ ] Create/update `.gitignore` to protect `.env`
- [ ] Create `.env.example` with template (no real keys)
- [ ] Test that code still works with new key

### Phase 3: Git History Cleanup (Within 4 hours)
- [ ] Run `git filter-branch` to remove old key from history
- [ ] Force push to GitHub
- [ ] Verify key removed from all commits

### Phase 4: Verification (Before considering done)
- [ ] Confirm old key revoked in NCBI account
- [ ] Verify `.env` in `.gitignore`
- [ ] Check git log shows no real API keys
- [ ] Confirm application works with new key
- [ ] Monitor for any unauthorized API usage

---

## Files Affected

**Files containing hardcoded key:**
1. `pubmed_extract-0.1/pubmed_extract-0.1/pubmed_extract/extraction.py`

**Files that need .gitignore protection:**
- `.env` (create this, don't commit)
- `.env.example` (commit this as template)

---

## Verification Commands

### Check if key is still in git history

```powershell
cd 'C:\Users\VHATVHZajacN\OneDrive - Department of Veterans Affairs\Desktop\AI Projects\GRECC\Python Rev2'

# Search entire git history for the exposed key
git log -p --all | Select-String "9446fd13127051ca7343c2018204779d8007"

# If nothing returns, the key is removed ✓
```

### Check .gitignore is working

```powershell
# Verify .env is in .gitignore
git check-ignore .env

# Should output: .env
```

### Verify no other keys exposed

```powershell
# Search for other potential API key patterns
git log -p --all | Select-String "api_key|API_KEY|password|secret|token" | Select-String -Pattern "=" | head -20
```

---

## New API Key Setup

### 1. Get New Key from NCBI

1. Visit: https://www.ncbi.nlm.nih.gov/account/settings/
2. Go to "API Key Management"
3. Click "Create New API Key"
4. Copy the new key (example: `abc123xyz789...`)

### 2. Add to .env File

Create file: `C:\Users\...\Python Rev2\.env`

```
NCBI_API_KEY=your_new_ncbi_key_here
NCBI_EMAIL=your_email@va.gov
```

### 3. Create .env.example Template

Create file: `C:\Users\...\Python Rev2\.env.example`

```
# NCBI Configuration
# Get API key at: https://www.ncbi.nlm.nih.gov/account/settings/
NCBI_API_KEY=your_ncbi_api_key_here
NCBI_EMAIL=your_email@example.com
```

### 4. Update Code to Use New Key

```python
from dotenv import load_dotenv
import os

load_dotenv()

# Get API key from environment
NCBI_API_KEY = os.getenv("NCBI_API_KEY")
NCBI_EMAIL = os.getenv("NCBI_EMAIL")

if not NCBI_API_KEY:
    raise ValueError("NCBI_API_KEY must be set in .env file")
```

---

## FAQ

### Q: Is my GitHub account compromised?
**A**: No, only the NCBI API key is exposed. However, the key itself must be revoked immediately.

### Q: Can I just delete the file from the latest commit?
**A**: No. Deleting from the latest commit doesn't remove it from git history. Anyone can see the old commits. You must use `git filter-branch` or `git rebase` to rewrite history.

### Q: Will force push break anything?
**A**: If you're the only developer on this repo, no. If others are working on it, notify them to re-clone after the force push.

### Q: Can someone use my exposed API key?
**A**: Yes, they could make requests to NCBI using your key. This is why you MUST revoke it immediately.

### Q: How do I monitor for misuse?
**A**: 
1. Check NCBI account for suspicious activity
2. Review request logs if available
3. Monitor your account for unusual charges/usage

---

## Prevention Going Forward

### Best Practices

✅ **DO:**
- Store all secrets in `.env` file (not committed)
- Use `.env.example` as template (safe to commit)
- Load secrets with `os.getenv()`
- Rotate API keys periodically
- Use `.gitignore` for all sensitive files
- Enable GitHub's secret scanning

❌ **DON'T:**
- Hardcode API keys in code
- Commit `.env` file
- Share API keys via email/chat
- Reuse keys across projects
- Log API keys to console/files
- Commit any credentials

### GitHub Security Settings

1. Enable **Secret Scanning** in GitHub
2. Enable **Branch Protection** on `main`
3. Require Pull Request reviews before merge
4. Set up **CODEOWNERS** for critical files

---

## Timeline for Completion

| Phase | Action | Timeline | Status |
|-------|--------|----------|--------|
| 1 | Revoke exposed API key | NOW (1 hour) | ⏳ |
| 2 | Update code & .gitignore | 2 hours | ⏳ |
| 3 | Remove from git history | 4 hours | ⏳ |
| 4 | Verify all changes | 6 hours | ⏳ |

---

## Support Resources

- **NCBI API Help**: https://www.ncbi.nlm.nih.gov/books/NBK25497/
- **GitHub Security**: https://docs.github.com/en/code-security
- **Git History Rewriting**: https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History
- **python-dotenv**: https://python-dotenv.readthedocs.io/

---

## Incident Summary

**What**: Hardcoded API key exposed in public GitHub repo  
**When**: Committed in initial commit (40000a3)  
**Where**: `pubmed_extract/extraction.py`  
**Who**: Available to anyone with GitHub access  
**Fix**: Revoke key + remove from history + use environment variables  

**Status**: 🔴 **Action Required** - Contact NCBI to revoke key NOW

---

**Document Created**: December 10, 2025  
**Next Review**: After remediation completed
