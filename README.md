# 🧠 PHANTOM AI - Public Website Repository

## 📄 Repository Contents

This is the **PUBLIC** repository for PHANTOM AI's marketing website and documentation.

### 🌐 Live Website
**URL:** https://SainiONHacks.github.io/phantom-ai-website/

---

## 📁 Files in This Repository

### 1. **index.html** - Main Landing Page
- Product features showcase
- Pricing plans (₹100, ₹500, ₹2000)
- Video demo section (placeholder)
- Download links
- System requirements
- Full sales funnel

**Sections:**
- ✨ Features Grid (8 key features)
- 🎬 Video Demo (ready for YouTube embed)
- 💰 Pricing Cards (3 plans with detailed breakdown)
- ⬇️ Download Section (EXE coming soon)
- 📋 System Requirements
- 🚀 Call to Action
- 📞 Contact & Support

### 2. **dashboard.html** - User Dashboard
- Login/Sign-up interface with tabs
- User statistics (sessions, queries, licenses)
- License tracking
- Latest updates feed
- Quick actions (purchase, support, docs)
- Full account management

**Features:**
- 🔑 Login/Signup tabs
- 📊 Usage statistics
- 🔑 License management
- 📢 Update notifications
- ⚡ Quick action buttons

### 3. **GITHUB_PAGES_SETUP.md** - Documentation
- Complete GitHub Pages setup guide
- How to add video demo
- How to add EXE download
- Customization instructions
- SEO optimization tips
- Custom domain setup

---

## 🔐 What's NOT in This Repository

This repository does **NOT** contain:
- ❌ Customer data
- ❌ License keys
- ❌ User passwords
- ❌ Email addresses
- ❌ Purchase history
- ❌ GitHub tokens

All sensitive data is stored in a **PRIVATE** repository (`phantom-ai-licenses`) that only the owner can access.

---

## 📝 How to Update Website

### Method 1: Using Upload Script (Recommended)
```bash
# Edit files locally
# Then run:
python upload_website.py
```

### Method 2: Direct GitHub Upload
1. Go to: https://github.com/SainiONHacks/phantom-ai-website
2. Click on file (e.g., index.html)
3. Click "Edit" (pencil icon)
4. Make changes
5. Commit changes

### Method 3: Git Commands
```bash
git add index.html dashboard.html
git commit -m "Update website content"
git push origin main
```

Changes appear live in 1-2 minutes!

---

## 🎬 Adding Video Demo

When your demo video is ready:

1. Upload to YouTube
2. Get embed code
3. Open `index.html`
4. Find line ~398 (video placeholder)
5. Replace with:
```html
<iframe src="https://www.youtube.com/embed/YOUR_VIDEO_ID" 
        allowfullscreen></iframe>
```
6. Run: `python upload_website.py`

---

## ⬇️ Adding EXE Download

When EXE is ready:

1. Create GitHub Release:
   - Go to: https://github.com/SainiONHacks/phantom-ai-website/releases
   - Click "Create a new release"
   - Tag: `v2.1`
   - Upload `Phantom-AI.exe`
   - Publish release

2. Update `index.html` download link (line ~574):
```html
<a href="https://github.com/SainiONHacks/phantom-ai-website/releases/download/v2.1/Phantom-AI.exe" 
   class="download-button">
  <span>🪟</span>
  <span>Download for Windows</span>
</a>
```

3. Remove `disabled` class from button

4. Run: `python upload_website.py`

---

## 🎨 Customization

### Change Pricing
Edit `index.html` pricing cards section:
```html
<div class="price">₹100</div>  <!-- Change amount -->
<div class="period">1 Day Access</div>  <!-- Change duration -->
```

### Update Contact Email
Replace all instances of `nitinsaini077@gmail.com` with your email.

### Modify Colors
Update CSS gradient colors:
```css
background: linear-gradient(135deg, #00ff88, #00d4ff);
```

### Add Features
Copy existing feature card and modify:
```html
<div class="feature-card">
    <div class="feature-icon">🎯</div>
    <h3>Your Feature</h3>
    <p>Description...</p>
</div>
```

---

## 📊 SEO & Analytics

### Add Google Analytics
Add before `</head>` in index.html:
```html
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### Optimize Meta Tags
Already included:
- Title, description, keywords
- Open Graph tags
- Mobile viewport
- Responsive design

---

## 🔗 Important Links

- **Live Website:** https://SainiONHacks.github.io/phantom-ai-website/
- **Dashboard:** https://SainiONHacks.github.io/phantom-ai-website/dashboard.html
- **Main Page:** https://SainiONHacks.github.io/phantom-ai-website/#features
- **Pricing:** https://SainiONHacks.github.io/phantom-ai-website/#pricing
- **Download:** https://SainiONHacks.github.io/phantom-ai-website/#download

---

## 📱 Mobile Responsive

Website is fully responsive and tested on:
- ✅ Desktop (1920x1080+)
- ✅ Laptop (1366x768)
- ✅ Tablet (768x1024)
- ✅ Mobile (375x667)

---

## 🛡️ Security Note

This is a **PUBLIC** repository - safe to share!

**Contains:**
- ✅ Marketing content
- ✅ Product information
- ✅ Documentation
- ✅ Public-facing pages

**Does NOT contain:**
- ❌ Customer database
- ❌ License keys
- ❌ API tokens
- ❌ Sensitive data

For security documentation, see `SECURITY.md` in desktop app files.

---

## 📞 Support & Contributions

**Issues/Suggestions:** 
- Email: nitinsaini077@gmail.com
- GitHub Issues: https://github.com/SainiONHacks/phantom-ai-website/issues

**Documentation:**
- Setup Guide: GITHUB_PAGES_SETUP.md (in this repo)
- Security: SECURITY.md (in desktop app)

---

## 📅 Update History

**February 5, 2026:**
- ✅ Added sign-up functionality to dashboard
- ✅ Fixed documentation links (now point to website repo)
- ✅ Added login/signup tabs
- ✅ Improved dashboard UI
- ✅ Updated Quick Actions links

**February 4, 2026:**
- ✅ Initial website launch
- ✅ Dashboard page created
- ✅ Pricing updated to Indian Rupees
- ✅ Important notes and disclaimers added

---

## 🚀 Deployment

Website automatically deploys via GitHub Pages:
- Push to `main` branch → Auto-deploy in 1-2 minutes
- No build process needed (static HTML)
- SSL certificate included (HTTPS)
- CDN delivery (fast worldwide)

---

**Made with 💚 by Nitin Saini**

*Last Updated: February 5, 2026*
