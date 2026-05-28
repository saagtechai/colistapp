# Colist PWA — Deployment & App Store Guide

## Project Structure
```
colist-pwa/
├── index.html       ← Main app (all-in-one)
├── manifest.json    ← PWA manifest
├── sw.js            ← Service worker (offline support)
└── icons/
    ├── icon-192.png
    └── icon-512.png
```

---

## Step 1 — Get Your Anthropic API Key
1. Visit https://console.anthropic.com
2. Go to **API Keys** → **Create Key**
3. Copy your key (starts with `sk-ant-`)
4. Enter it in the app under **Settings → API Key**

---

## Step 2 — Deploy the Web App

### Option A: Netlify (Recommended — free, instant)
1. Go to https://netlify.com → sign up free
2. Drag and drop the entire `colist-pwa/` folder onto the Netlify dashboard
3. Your app is live at `https://your-app-name.netlify.app`
4. Optional: connect a custom domain (e.g. `colist.yourdomain.com`)

### Option B: Vercel
1. Install Vercel CLI: `npm i -g vercel`
2. Run in the project folder: `vercel deploy`
3. Follow prompts — app is live in ~30 seconds

### Option C: GitHub Pages
1. Push the `colist-pwa/` folder to a GitHub repo
2. Go to repo **Settings → Pages → Deploy from branch → main**
3. Live at `https://yourusername.github.io/colist-pwa`

### Option D: Self-hosted
Serve the folder from any static file server.
**Important:** HTTPS is required for PWA install prompts and service workers.

---

## Step 3 — Install on iOS (Add to Home Screen)
1. Open your deployed URL in **Safari** on iPhone/iPad
2. Tap the **Share** button (box with arrow)
3. Scroll down → tap **Add to Home Screen**
4. Tap **Add** — app appears on home screen like a native app

> Apple does not allow PWAs in the App Store directly, but users install
> via Safari with full offline support, push notifications, and home screen icon.

---

## Step 4 — Submit to Google Play Store (TWA)

PWAs can be published to the Google Play Store as a **Trusted Web Activity (TWA)**.

### Requirements
- Node.js 16+ installed
- Android Studio installed (for signing)
- Google Play Developer account ($25 one-time fee)
  at https://play.google.com/console

### Build with Bubblewrap (Google's official tool)
```bash
# Install Bubblewrap
npm install -g @bubblewrap/cli

# Initialize TWA project (run from colist-pwa folder)
bubblewrap init --manifest https://your-deployed-url.com/manifest.json

# Follow prompts — enter your app details:
#   Package name: com.yourname.colist
#   App name: Colist
#   Start URL: https://your-deployed-url.com

# Build the APK/AAB
bubblewrap build
```

### Sign and Upload
1. Bubblewrap generates a signing key during init — save it securely
2. Upload the generated `.aab` file to Google Play Console
3. Fill in store listing: description, screenshots, category (Business)
4. Submit for review (~3-7 days)

### Alternative: PWABuilder (no-code)
1. Go to https://www.pwabuilder.com
2. Enter your deployed URL
3. Click **Package for stores** → select **Google Play**
4. Download the `.aab` and upload to Play Console

---

## Step 5 — Microsoft Store (Optional)
PWABuilder also supports packaging for the **Microsoft Store**:
1. https://www.pwabuilder.com → enter URL → **Windows**
2. Download the MSIX package and submit to Partner Center

---

## Checklist Before Submission

- [ ] App deployed on HTTPS
- [ ] `manifest.json` has all required fields
- [ ] Icons: 192×192 and 512×512 PNG
- [ ] Service worker registered and working
- [ ] App works offline (at least shows cached UI)
- [ ] Tested on real iOS and Android devices
- [ ] API key instructions visible in Settings screen
- [ ] Privacy policy URL (required for Play Store)

---

## Notes

- **API Key security:** In production, consider proxying Anthropic API calls
  through your own backend so the API key is never exposed client-side.
  A simple Netlify/Vercel serverless function handles this easily.

- **iOS App Store:** A fully native iOS app requires wrapping the PWA in a
  WKWebView with Swift. Tools like Median.co or Capacitor can do this
  without rewriting code.
