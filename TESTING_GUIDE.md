# 🧪 Security, Compliance & RBAC Testing Guide

## 🎯 Test Objectives

1. ✅ Verify only authorized users can approve licenses
2. ✅ Ensure no direct repository access is required
3. ✅ Validate all operations work through GitHub website only
4. ✅ Confirm RBAC (Role-Based Access Control) is enforced
5. ✅ Test audit logging and compliance tracking

---

## 🔐 Test 1: Role-Based Access Control (RBAC)

### Setup Test Users

Create 3 test GitHub accounts:
- `admin-test` - Admin role (Write access)
- `support-test` - Support role (Read access)  
- `unauthorized-test` - No access

### Test 1A: Admin Can Approve
**Expected:** ✅ SUCCESS

1. Login as `admin-test`
2. Go to Issues tab
3. Click "New Issue" → Select "License Request"
4. Fill form:
   - Order ID: `TEST-001`
   - Email: `admin@test.com`
   - Plan: `1 Month Pro`
   - Check all verification boxes
5. Submit issue
6. Add label: `approve-license`
7. Go to Actions tab
8. Wait for workflow to complete (30-60 seconds)
9. Check issue comments

**✅ Expected Result:**
- Workflow runs successfully
- License key posted in comment
- Issue closed automatically
- License added to `licenses.json`

### Test 1B: Support Cannot Approve
**Expected:** ❌ BLOCKED

1. Login as `support-test` (Read-only)
2. Go to Issues tab
3. Open existing license request
4. Try to add label: `approve-license`

**✅ Expected Result:**
- Cannot add labels (permission denied)
- Error message: "Only users with write access can add labels"

### Test 1C: Unauthorized User Blocked
**Expected:** ❌ REJECTED

1. Add `unauthorized-test` as collaborator with Write access
2. But DON'T add username to workflow's `AUTHORIZED_USERS` list
3. Login as `unauthorized-test`
4. Create license request issue
5. Add label: `approve-license`

**✅ Expected Result:**
- Workflow starts but fails auth check
- Comment added: "❌ UNAUTHORIZED: Only admin users can approve"
- Label `unauthorized` added
- `approve-license` label removed
- No license generated

**Test Status:** [ ] Pass [ ] Fail
**Tester:** ________________
**Date:** ________________

---

## 🚫 Test 2: No Direct Repository Access

### Test 2A: Cannot Clone Private Repo
**Expected:** ❌ ACCESS DENIED

1. Logout of GitHub
2. Try to clone repository:
```bash
git clone https://github.com/SainiONHacks/phantom-ai-licenses.git
```

**✅ Expected Result:**
- Error: "Repository not found" or "Permission denied"
- Cannot access repository without authentication

### Test 2B: Cannot Access Raw Files
**Expected:** ❌ ACCESS DENIED

1. Open browser (incognito mode)
2. Try to access:
```
https://raw.githubusercontent.com/SainiONHacks/phantom-ai-licenses/main/licenses.json
```

**✅ Expected Result:**
- 404 Not Found (repository is private)
- No customer data exposed

### Test 2C: GitHub Pages is Public but Safe
**Expected:** ✅ PUBLIC BUT NO SENSITIVE DATA

1. Open browser (not logged in)
2. Access GitHub Pages URL:
```
https://sainionhacks.github.io/phantom-ai-licenses/
```

**✅ Expected Result:**
- Website loads successfully
- Admin dashboard accessible
- But NO data displayed (must login)
- No tokens in JavaScript console
- No API calls expose sensitive data

**Test Status:** [ ] Pass [ ] Fail
**Tester:** ________________
**Date:** ________________

---

## 🌐 Test 3: Everything Through GitHub Website Only

### Test 3A: Create License Request (Web UI)
**Expected:** ✅ SUCCESS

1. Login to GitHub.com as admin
2. Navigate to repository
3. Click **Issues** tab
4. Click **New issue**
5. Select **License Request** template
6. Fill form in browser
7. Click **Submit new issue**

**✅ Expected Result:**
- Issue created successfully
- No command line needed
- No API tokens required
- All done in browser

### Test 3B: Approve License (Web UI)
**Expected:** ✅ SUCCESS

1. Open the license request issue
2. On right sidebar, click **Labels**
3. Select `approve-license`
4. Click away to apply label
5. Refresh page after 30 seconds

**✅ Expected Result:**
- GitHub Actions workflow triggered automatically
- Progress visible in Actions tab
- No manual commands needed
- License key appears in issue comments
- Issue closes automatically

### Test 3C: View License (Web UI)
**Expected:** ✅ SUCCESS

1. Click **Code** tab
2. Click on `licenses.json` file
3. Click **History** to see commits

**✅ Expected Result:**
- Can view file contents in browser
- Can see commit history
- Can see who approved each license
- No need to clone repository

**Test Status:** [ ] Pass [ ] Fail
**Tester:** ________________
**Date:** ________________

---

## 📋 Test 4: Audit Logging & Compliance

### Test 4A: View Audit Trail
**Expected:** ✅ COMPLETE HISTORY

1. Go to Settings → Audit log
2. Filter events:
   - `issue.labeled` - License approval
   - `workflow_run.completed` - Automation
   - `issue.closed` - Request fulfilled

**✅ Expected Result:**
- All license approvals logged
- Timestamp and user recorded
- Complete audit trail available
- Can export logs for compliance

### Test 4B: Issue-Based Audit
**Expected:** ✅ DETAILED TRACKING

1. Open any approved license issue
2. Scroll through timeline

**✅ Expected Result:**
- Shows who created request
- Shows who approved (added label)
- Shows workflow execution
- Shows license key (in comment)
- Complete chain of custody

### Test 4C: GDPR Data Request Simulation
**Expected:** ✅ DATA ACCESSIBLE

Scenario: Customer requests all their data

1. Go to repository
2. Press `/` (search)
3. Search: `customer@example.com`
4. Find in `licenses.json` and issue comments

**✅ Expected Result:**
- Can locate all customer data
- Email, license key, expiry date visible
- Issue history provides context
- Can export data for customer

**Test Status:** [ ] Pass [ ] Fail
**Tester:** ________________
**Date:** ________________

---

## 🔒 Test 5: Security Boundaries

### Test 5A: Token Not in Client Code
**Expected:** ✅ NO TOKENS EXPOSED

1. Visit GitHub Pages site
2. Open DevTools (F12)
3. Go to Sources tab
4. Search all JavaScript files for:
   - `ghp_`
   - `token`
   - `secret`

**✅ Expected Result:**
- No GitHub tokens found
- No API keys exposed
- All sensitive operations server-side (GitHub Actions)

### Test 5B: Cannot Modify licenses.json Directly
**Expected:** ❌ PROTECTED

1. Login as non-admin collaborator
2. Try to edit `licenses.json` via Web UI
3. Make a change
4. Try to commit

**✅ Expected Result:**
- Branch protection prevents direct commit
- Must create pull request
- Admin must review and approve
- Cannot bypass approval process

### Test 5C: Failed Auth Logged
**Expected:** ✅ LOGGED

1. Login as unauthorized user
2. Create issue and try to approve
3. Check Actions tab → Failed workflow
4. Check workflow logs

**✅ Expected Result:**
- Workflow shows "UNAUTHORIZED" error
- User who attempted approval is logged
- Issue labeled with `unauthorized`
- Security event recorded

**Test Status:** [ ] Pass [ ] Fail
**Tester:** ________________
**Date:** ________________

---

## ⏱️ Test 6: Performance & Reliability

### Test 6A: Concurrent Requests
**Expected:** ✅ HANDLES MULTIPLE

1. Create 3 license request issues
2. Add `approve-license` label to all 3 quickly
3. Watch Actions tab

**✅ Expected Result:**
- All 3 workflows run in parallel
- Each generates unique license key
- No conflicts in `licenses.json`
- All complete successfully

### Test 6B: Workflow Timeout
**Expected:** ✅ COMPLETES IN TIME

1. Create license request
2. Approve (add label)
3. Start timer

**✅ Expected Result:**
- Workflow completes in < 60 seconds
- License key posted in < 2 minutes
- No timeout errors

**Test Status:** [ ] Pass [ ] Fail
**Tester:** ________________
**Date:** ________________

---

## 📊 Compliance Tests

### Test 7A: GDPR Right to Access
**Scenario:** Customer requests their data

**Procedure:**
1. Search repository for customer email
2. Collect all matching records:
   - Entry in `licenses.json`
   - License request issue
   - Approval comments

**✅ Expected Result:**
- All customer data found in < 5 minutes
- Data is accurate and complete
- Can export to send to customer

**Compliance:** [ ] Pass [ ] Fail

### Test 7B: GDPR Right to Deletion
**Scenario:** Customer requests data deletion

**Procedure:**
1. Create test license for `delete-test@example.com`
2. Simulate deletion:
   - Edit `licenses.json` - remove entry
   - Close related issues with note
3. Commit changes: "Delete data for GDPR request"
4. Verify deletion

**✅ Expected Result:**
- Data removed from `licenses.json`
- Issues marked "data deleted"
- Operation logged in audit trail
- Process took < 10 minutes

**Compliance:** [ ] Pass [ ] Fail

### Test 7C: GDPR Right to Rectification
**Scenario:** Customer email changed

**Procedure:**
1. Find license in `licenses.json`
2. Create pull request to update email
3. Admin reviews and approves
4. Merge PR

**✅ Expected Result:**
- Data updated accurately
- Change logged in commit history
- Old/new values visible in diff
- Customer notified via issue comment

**Compliance:** [ ] Pass [ ] Fail

---

## 🎯 Test Results Summary

### RBAC Tests
- [ ] Admin can approve ✅
- [ ] Support cannot approve ❌
- [ ] Unauthorized blocked ❌
- [ ] Result: **PASS** / **FAIL**

### Access Control Tests
- [ ] Cannot clone private repo ❌
- [ ] Cannot access raw files ❌
- [ ] Web UI works ✅
- [ ] Result: **PASS** / **FAIL**

### Web-Only Operation Tests
- [ ] Create request via web ✅
- [ ] Approve via web ✅
- [ ] View via web ✅
- [ ] Result: **PASS** / **FAIL**

### Audit & Compliance Tests
- [ ] Audit trail complete ✅
- [ ] GDPR access ✅
- [ ] GDPR deletion ✅
- [ ] Result: **PASS** / **FAIL**

### Security Tests
- [ ] No token exposure ✅
- [ ] File protection ✅
- [ ] Failed auth logged ✅
- [ ] Result: **PASS** / **FAIL**

---

## ✅ Final Certification

**Test Date:** ________________

**Tested By:** ________________

**Overall Result:** [ ] ✅ ALL TESTS PASSED [ ] ❌ SOME TESTS FAILED

**Security Score:** ___ / 100

**Ready for Production:** [ ] YES [ ] NO

### Sign-off

**Admin:** ________________ Date: ______

**Security:** ________________ Date: ______

**Compliance:** ________________ Date: ______

---

## 🐛 Issues Found

| Test # | Issue | Severity | Status | Fix Date |
|--------|-------|----------|--------|----------|
| | | | | |
| | | | | |

---

## 📝 Recommendations

After testing, document any recommended improvements:

1. ____________________________________
2. ____________________________________
3. ____________________________________

---

**Next Test Date:** ________________ (Quarterly recommended)
