# 🚀 GitHub Pages Deployment - Secure Setup Guide
## Zero-Token, Web-Only, RBAC-Enabled License Management

---

## 🎯 Overview

This deployment uses **GitHub Actions + Issues** for secure license management:

✅ **No tokens in browser** - All operations server-side via GitHub Actions  
✅ **Web-only interface** - Everything done through GitHub.com  
✅ **RBAC enforced** - Only authorized admins can approve licenses  
✅ **Complete audit trail** - Every action logged in issues & workflows  
✅ **GDPR compliant** - Right to access, delete, rectify customer data  

---

## 📋 Prerequisites

- GitHub account
- Repository (can be private or public)
- Admin access to repository
- GitHub Actions enabled (free for public repos, included in private)

---

## 🛠️ Step-by-Step Setup

### Step 1: Create GitHub Repository

1. Go to https://github.com/new
2. Repository name: `phantom-ai-licenses` (or your choice)
3. Description: "License management for SainiON AI Tool"
4. **Visibility:** PRIVATE (recommended)
5. Initialize with README: ✅
6. Add .gitignore: None
7. License: None
8. Click **Create repository**

**✅ Checkpoint:** Repository created

---

### Step 2: Upload Files

#### Option A: Via Web UI (Easiest)
1. Click **Add file** → **Upload files**
2. Upload from `BACKUP_DEPLOYMENTS\1_GITHUB_PAGES_DEPLOY\`:
   - All files in `website/` folder
   - `.github/` folder (workflows + issue templates)
   - `SECURITY.md`
   - `TESTING_GUIDE.md`
3. Commit message: "Initial secure setup"
4. Click **Commit changes**

#### Option B: Via Git Command Line
```bash
cd BACKUP_DEPLOYMENTS\1_GITHUB_PAGES_DEPLOY

git init
git add .
git commit -m "Initial secure setup"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/phantom-ai-licenses.git
git push -u origin main
```

**✅ Checkpoint:** Files uploaded to repository

---

### Step 3: Enable GitHub Pages

1. Go to repository **Settings**
2. Scroll to **Pages** (left sidebar)
3. Source: **Deploy from a branch**
4. Branch: `main`
5. Folder: `/website` (or `/ (root)` if files are in root)
6. Click **Save**
7. Wait 1-2 minutes for deployment
8. Visit: `https://YOUR-USERNAME.github.io/phantom-ai-licenses/`

**✅ Checkpoint:** Website is live

---

### Step 4: Configure RBAC (Role-Based Access Control)

#### 4A: Set Repository to Private
1. Settings → General → Danger Zone
2. Click **Change visibility**
3. Select **Private**
4. Confirm with repository name

**⚠️ CRITICAL:** Private repo protects customer data

#### 4B: Add Admin Users
1. Settings → Collaborators
2. Click **Add people**
3. Enter GitHub username (e.g., `AdminUser2`)
4. Select role: **Write** or **Admin**
5. Send invitation
6. Admin accepts invitation

Repeat for each admin (max 2-3 recommended)

#### 4C: Configure Authorized Approvers
1. Open `.github/workflows/approve-license.yml`
2. Edit line ~14:
```yaml
AUTHORIZED_USERS="SainiONHacks,YourUsername,AdminUser2"
```
3. Replace with actual GitHub usernames (comma-separated)
4. Commit changes:
   - Message: "Configure authorized admins"
   - **Commit directly to main**

**✅ Checkpoint:** Only listed users can approve licenses

---

### Step 5: Configure Branch Protection

1. Settings → Branches
2. Click **Add rule**
3. Branch name pattern: `main`
4. Enable:
   - ✅ Require pull request reviews (1 approval)
   - ✅ Require status checks to pass
   - ✅ Require conversation resolution
   - ✅ Include administrators
   - ✅ Restrict who can push (select admins only)
5. Disable:
   - ❌ Allow force pushes
   - ❌ Allow deletions
6. Click **Create** or **Save changes**

**✅ Checkpoint:** Main branch is protected

---

### Step 6: Test GitHub Actions

1. Go to **Actions** tab
2. If prompted, click **I understand, enable Actions**
3. You should see workflow: "🔑 Approve License Request"
4. Workflow status: ✅ Ready (waiting for events)

**✅ Checkpoint:** GitHub Actions is enabled

---

### Step 7: Create Initial licenses.json

1. Click **Add file** → **Create new file**
2. Name: `licenses.json`
3. Content:
```json
{
  "licenses": [],
  "metadata": {
    "last_updated": "2026-02-10",
    "total_licenses": 0
  }
}
```
4. Commit message: "Initialize licenses database"
5. Click **Commit**

**✅ Checkpoint:** Database file created

---

## 🎮 How to Use - Admin Workflow

### Create License Request

1. Go to **Issues** tab
2. Click **New issue**
3. Select template: **🔒 License Request**
4. Fill the form:
   - **Order ID:** Unique identifier (e.g., `ORD-2024-001`)
   - **Customer Email:** `customer@example.com`
   - **Plan:** Select from dropdown
   - **Payment Reference:** Transaction ID
   - **Notes:** Any special instructions
   - **Verification:** Check all boxes
5. Click **Submit new issue**

**Issue Created!** Status: `pending`

### Approve License Request

1. Open the license request issue
2. Review details carefully
3. **Add label:** `approve-license`
   - Method 1: Right sidebar → Labels → select `approve-license`
   - Method 2: Type `/label approve-license` in comment
4. GitHub Actions starts automatically
5. Wait 30-60 seconds
6. Check issue comments for license key

**Expected Result:**
```
✅ License Approved!

🔑 License Details
- License Key: ABCD-1234-EFGH-5678
- Tier: PRO
- Expires: 2026-03-12
- Duration: 30 days
```

### Send License to Customer

1. Copy license key from issue comment
2. Email to customer:
```
Subject: Your SainiON AI License Key

Dear Customer,

Your license has been approved!

License Key: ABCD-1234-EFGH-5678
Tier: PRO
Expires: March 12, 2026

To activate:
1. Open SainiON AI Tool
2. Click "Activate License"
3. Enter the license key above
4. Click "Validate"

Thank you for your purchase!
```

---

## 🔍 Verification & Testing

### Test 1: Admin Can Approve
1. Create test issue (your own email)
2. Add `approve-license` label
3. Check Actions tab → Workflow running
4. Verify license in issue comment
5. Check `licenses.json` → License added

**Expected:** ✅ SUCCESS

### Test 2: Non-Admin Cannot Approve
1. Invite test user with Read-only access
2. Login as test user
3. Try to add `approve-license` label to issue

**Expected:** ❌ BLOCKED (no permission)

### Test 3: Unauthorized User Rejected
1. Add user with Write access
2. But DON'T add to `AUTHORIZED_USERS` in workflow
3. User adds `approve-license` label
4. Workflow runs but fails auth check

**Expected:** ❌ REJECTED with "UNAUTHORIZED" comment

### Test 4: No Tokens Exposed
1. Visit GitHub Pages site
2. Open DevTools (F12)
3. Check Sources/Network tabs
4. Search for: `ghp_`, `token`, `secret`

**Expected:** ❌ NO TOKENS FOUND

---

## 🔐 Security Checklist

Before going live, verify:

- [ ] ✅ Repository is PRIVATE
- [ ] ✅ Only admins have Write access
- [ ] ✅ `AUTHORIZED_USERS` list is correct
- [ ] ✅ Branch protection enabled on `main`
- [ ] ✅ GitHub Actions enabled
- [ ] ✅ `licenses.json` initialized
- [ ] ✅ No tokens in client code
- [ ] ✅ HTTPS enforced on GitHub Pages
- [ ] ✅ Tested license approval flow
- [ ] ✅ Tested unauthorized rejection

**Security Score:** ___ / 10

---

## 📊 Monitoring & Maintenance

### Daily: Check Pending Requests
1. Go to **Issues** tab
2. Filter: `is:open label:license-request label:pending`
3. Review and approve within 24 hours

### Weekly: Review Audit Log
1. Settings → Audit log
2. Filter events:
   - `issue.labeled` (approvals)
   - `workflow_run.completed` (automation)
3. Verify only authorized users approved licenses

### Monthly: Security Review
1. Review collaborators list
2. Update `AUTHORIZED_USERS` if staff changes
3. Check for security alerts (Dependabot)
4. Test approval workflow

### Quarterly: Access Audit
1. Review who has access
2. Remove inactive collaborators
3. Rotate credentials if needed
4. Update documentation

---

## 🐛 Troubleshooting

### Issue: Workflow Not Triggering
**Symptoms:** Adding `approve-license` label does nothing

**Solutions:**
1. Check Actions tab → Verify Actions is enabled
2. Check `.github/workflows/approve-license.yml` exists
3. Verify YAML syntax (use yamllint.com)
4. Check workflow permissions

### Issue: "UNAUTHORIZED" Error
**Symptoms:** Workflow runs but rejects approval

**Solutions:**
1. Verify user is in `AUTHORIZED_USERS` list
2. Check exact spelling (case-sensitive)
3. Ensure user has Write access to repo
4. Re-commit workflow file after editing

### Issue: License Not Added to licenses.json
**Symptoms:** Workflow succeeds but file not updated

**Solutions:**
1. Check `jq` is available (should be in Ubuntu runner)
2. Verify `licenses.json` exists and is valid JSON
3. Check workflow logs for errors
4. Ensure repository write permissions granted

### Issue: Cannot Access GitHub Pages
**Symptoms:** 404 Not Found on Pages URL

**Solutions:**
1. Settings → Pages → Verify enabled
2. Wait 5 minutes after enabling
3. Check source branch is `main`
4. Verify `index.html` or `admin_dashboard.html` exists in folder

---

## 🚀 Going Live Checklist

Ready to launch? Complete this checklist:

### Pre-Launch
- [ ] All setup steps completed
- [ ] Security checklist: 10/10
- [ ] Tested with 3 test license requests
- [ ] Admin team trained on approval process
- [ ] Customer email template prepared
- [ ] Backup procedures documented
- [ ] Incident response plan ready

### Launch Day
- [ ] Repository visibility set correctly (Private)
- [ ] GitHub Pages URL confirmed working
- [ ] Test approval end-to-end
- [ ] Monitor first 5 real approvals
- [ ] Document any issues

### Post-Launch
- [ ] Daily monitoring for first week
- [ ] Customer feedback collected
- [ ] Performance metrics tracked
- [ ] Security audit completed
- [ ] Documentation updated

---

## 📞 Support & Resources

**Documentation:**
- Full Security Guide: `SECURITY.md`
- Testing Procedures: `TESTING_GUIDE.md`
- GitHub Actions Docs: https://docs.github.com/actions

**Common Tasks:**
- Add admin: Settings → Collaborators
- View audit log: Settings → Audit log
- Check workflows: Actions tab
- Monitor issues: Issues tab → Filters

**Emergency Contacts:**
- Repository Admin: @SainiONHacks
- Security: security@sainion.ai
- Support: support@sainion.ai

---

## ✅ Setup Complete!

Your secure, web-only, RBAC-enabled license management system is ready.

**Next Steps:**
1. Train admin team on approval process
2. Set up customer email template
3. Monitor first few approvals
4. Run quarterly security audit

**Estimated Setup Time:** 30 minutes  
**Monthly Maintenance:** < 2 hours  
**Cost:** FREE (GitHub Free includes Actions + Pages)

---

*Last Updated: 2026-02-10*
*Version: 2.0 - Secure GitHub Actions Implementation*
