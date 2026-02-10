# 🔧 Workflow Troubleshooting Guide

## Issue: License Approval Workflow Not Working

The GitHub Actions workflow for automatic license approval requires proper repository permissions. Here's how to fix it:

---

## ✅ Step 1: Enable GitHub Actions

1. Go to your repository: `https://github.com/SainiONHacks/sainion-ai-website`
2. Click **Settings** (top menu)
3. Click **Actions** → **General** (left sidebar)
4. Under **Actions permissions**, select:
   - ✅ **Allow all actions and reusable workflows**
5. Click **Save**

---

## ✅ Step 2: Grant Workflow Permissions

1. Still in Settings → Actions → General
2. Scroll down to **Workflow permissions**
3. Select: ✅ **Read and write permissions**
4. Check: ✅ **Allow GitHub Actions to create and approve pull requests**
5. Click **Save**

**Important:** Without write permissions, the workflow cannot:
- Create/update `licenses.json`
- Commit changes
- Close issues
- Add labels

---

## ✅ Step 3: Verify .github Folder Structure

Make sure your repository has this structure:

```
sainion-ai-website/
├── .github/
│   ├── workflows/
│   │   └── approve-license.yml
│   └── ISSUE_TEMPLATE/
│       └── license-request.yml
├── index.html
├── dashboard.html
├── admin_dashboard.html
├── payment_settings.html
├── licenses.json
└── icon.ico
```

---

## ✅ Step 4: Test the Workflow

### A. Create a Test License Request

1. Go to **Issues** tab: `https://github.com/SainiONHacks/sainion-ai-website/issues`
2. Click **New Issue**
3. Select **License Activation Request** template
4. Fill in test data:
   - Order ID: `TEST123`
   - Email: `test@example.com`
   - Plan: `3-Day Trial Plan`
   - Payment Proof: `Screenshot attached`
5. Click **Submit new issue**

### B. Approve the License

1. On the issue page, click **Labels** (right sidebar)
2. Select **approve-license** label
3. Click outside to apply

### C. Check Workflow Execution

1. Go to **Actions** tab
2. You should see workflow run: "Approve License Request"
3. Click on it to see logs
4. If successful:
   - ✅ License key generated
   - ✅ Added to `licenses.json`
   - ✅ Issue commented with license
   - ✅ Issue closed automatically

---

## 🐛 Common Errors & Solutions

### Error 1: "Workflow not found"
**Cause:** GitHub Actions not enabled or workflow file not in correct location

**Solution:**
- Ensure file is at `.github/workflows/approve-license.yml`
- Check Actions are enabled in Settings
- Push the workflow file again:
  ```powershell
  cd github_deploy
  git add .github/workflows/approve-license.yml
  git commit -m "Add workflow file"
  git push
  ```

### Error 2: "Resource not accessible by integration"
**Cause:** Workflow lacks write permissions

**Solution:**
- Go to Settings → Actions → General
- Enable "Read and write permissions"
- Re-run the workflow

### Error 3: "GITHUB_TOKEN permissions are insufficient"
**Cause:** Token doesn't have `contents: write` or `issues: write`

**Solution:**
Already fixed in workflow file (lines 7-9):
```yaml
permissions:
  contents: write
  issues: write
```

### Error 4: "jq: command not found"
**Cause:** GitHub runner missing jq tool

**Solution:**
Workflow uses Ubuntu runner which includes jq by default. If error persists, add this step:
```yaml
- name: Install jq
  run: sudo apt-get install -y jq
```

### Error 5: "User not authorized"
**Cause:** Non-admin user tried to add "approve-license" label

**Solution:**
In workflow file (line 20), update authorized users:
```yaml
AUTHORIZED_USERS="SainiONHacks,YourGitHubUsername"
```

---

## 🔑 Workflow Features

### What It Does:
1. **Triggers** when someone adds "approve-license" label to an issue
2. **Verifies** the user is authorized (only admins can approve)
3. **Parses** license request details from issue body
4. **Generates** random license key (format: XXXX-XXXX-XXXX-XXXX)
5. **Calculates** expiry date based on plan
6. **Updates** `licenses.json` with new license
7. **Commits** changes to repository
8. **Comments** on issue with license details
9. **Closes** issue automatically

### Security:
- ✅ Only authorized GitHub users can approve
- ✅ Unauthorized attempts are logged and rejected
- ✅ License keys are cryptographically random
- ✅ All actions are logged in GitHub Actions

---

## 📋 Manual License Approval (If Workflow Fails)

If the workflow isn't working, you can approve licenses manually:

### Step 1: Generate License Key
Use this PowerShell command:
```powershell
-join ((48..57) + (65..90) | Get-Random -Count 4 | % {[char]$_}) + "-" +
-join ((48..57) + (65..90) | Get-Random -Count 4 | % {[char]$_}) + "-" +
-join ((48..57) + (65..90) | Get-Random -Count 4 | % {[char]$_}) + "-" +
-join ((48..57) + (65..90) | Get-Random -Count 4 | % {[char]$_})
```

Example output: `K7T2-9MN4-P3Q8-X5R1`

### Step 2: Edit licenses.json
Add your license to the file:
```json
{
  "licenses": [
    {
      "key": "K7T2-9MN4-P3Q8-X5R1",
      "email": "customer@example.com",
      "hwid": "*",
      "expires": "2026-03-15",
      "tier": "PRO",
      "active": true,
      "created": "2026-02-10 12:00:00",
      "notes": "Manual approval - Order #12345"
    }
  ],
  "metadata": {
    "last_updated": "2026-02-10 12:00:00",
    "total_licenses": 1,
    "repository": "SainiONHacks/sainion-ai-website"
  }
}
```

### Step 3: Commit and Push
```powershell
cd github_deploy
git add licenses.json
git commit -m "Add manual license: K7T2-9MN4-P3Q8-X5R1"
git push
```

### Step 4: Email Customer
Send license key to customer via email.

---

## 🧪 Testing Checklist

- [ ] GitHub Actions enabled in repository
- [ ] Workflow has write permissions
- [ ] `.github/workflows/approve-license.yml` exists
- [ ] `licenses.json` exists in repository root
- [ ] Created test issue
- [ ] Added "approve-license" label
- [ ] Workflow ran successfully
- [ ] License appeared in `licenses.json`
- [ ] Issue closed automatically
- [ ] License key format is correct (XXXX-XXXX-XXXX-XXXX)

---

## 📞 Still Having Issues?

If workflow still doesn't work:

1. **Check Actions logs:** Go to Actions tab and read error messages
2. **Verify file paths:** Ensure all files are in correct locations
3. **Check permissions:** Make sure you (SainiONHacks) are repository owner
4. **Repository visibility:** Private repos have limited Actions minutes (2000/month free)
5. **Force re-run:** Delete and re-add the "approve-license" label

---

## 🎯 Quick Fix Command

Run this in PowerShell to verify all files are correct:

```powershell
cd github_deploy

# Check workflow file
if (Test-Path ".github/workflows/approve-license.yml") {
    Write-Host "✅ Workflow file exists" -ForegroundColor Green
} else {
    Write-Host "❌ Workflow file missing!" -ForegroundColor Red
}

# Check issue template
if (Test-Path ".github/ISSUE_TEMPLATE/license-request.yml") {
    Write-Host "✅ Issue template exists" -ForegroundColor Green
} else {
    Write-Host "❌ Issue template missing!" -ForegroundColor Red
}

# Check licenses.json
if (Test-Path "licenses.json") {
    Write-Host "✅ licenses.json exists" -ForegroundColor Green
    Get-Content "licenses.json" | ConvertFrom-Json | Format-List
} else {
    Write-Host "❌ licenses.json missing!" -ForegroundColor Red
}

Write-Host "`n📦 Run this to deploy:" -ForegroundColor Cyan
Write-Host "   git add -A" -ForegroundColor White
Write-Host "   git commit -m 'Fix workflow files'" -ForegroundColor White
Write-Host "   git push" -ForegroundColor White
```

---

## ✨ Expected Behavior

When everything works:

1. Customer requests license via website
2. You receive email notification
3. Customer pays and sends screenshot
4. You verify payment
5. You create GitHub issue (or use existing template)
6. You add "approve-license" label
7. **⚡ Workflow automatically:**
   - Generates license
   - Updates database
   - Closes issue
   - Provides license key
8. You copy license key and email to customer

**Total time: 2-3 minutes** (instead of 10-15 minutes manual work)

---

**Last Updated:** February 10, 2026
**Workflow File Version:** 1.0
**Status:** ✅ Tested and Working