# 📱 OPTIX PRO - Android APK Generation Guide

This guide explains how to create a private Android APK from the OPTIX PRO web application.

---

## 🚀 Quick Start Options

### Option 1: PWA Install (Easiest - No Build Required)

1. **Host the app** on any web server (GitHub Pages, Netlify, Vercel, or local server)
2. **Open in Chrome** on your Android device
3. **Tap the install prompt** that appears, or go to Menu → "Add to Home Screen"
4. The app will be installed and work like a native app!

---

### Option 2: PWABuilder (Recommended for APK)

**PWABuilder** generates a signed APK automatically from your hosted PWA.

1. **Deploy your app** to a public URL (e.g., GitHub Pages)
   ```bash
   # Using GitHub Pages
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/YOUR_USERNAME/optix-pro.git
   git push -u origin main
   # Enable GitHub Pages in repo settings
   ```

2. **Go to [PWABuilder.com](https://www.pwabuilder.com/)**

3. **Enter your app URL** and click "Start"

4. **Click "Build My PWA"** → Select "Android"

5. **Download the APK package** (includes signed APK)

6. **Install on your device**:
   - Enable "Install from Unknown Sources" in Android Settings
   - Transfer APK to phone and install

---

### Option 3: Bubblewrap CLI (Advanced)

**Bubblewrap** is Google's official tool for creating TWA (Trusted Web Activity) apps.

#### Prerequisites
- Node.js 14+
- Java JDK 8+
- Android SDK

#### Steps

1. **Install Bubblewrap**
   ```bash
   npm install -g @anthropic/anthropic@anthropic-ai/bubblewrap
   ```

2. **Initialize project**
   ```bash
   mkdir optix-pro-android
   cd optix-pro-android
   bubblewrap init --manifest https://YOUR_DOMAIN/manifest.json
   ```

3. **Answer the prompts**:
   - Package ID: `com.yourname.optixpro`
   - App name: `OPTIX PRO`
   - Signing key: Create new or use existing

4. **Build the APK**
   ```bash
   bubblewrap build
   ```

5. **Output**: `app-release-signed.apk` in your directory

---

### Option 4: Android Studio WebView Wrapper

For maximum control, create a native Android wrapper:

1. **Create new Android Studio project**
   - Select "Empty Activity"
   - Package name: `com.yourname.optixpro`
   - Minimum SDK: API 21

2. **Add Internet permission** in `AndroidManifest.xml`:
   ```xml
   <uses-permission android:name="android.permission.INTERNET" />
   ```

3. **Replace MainActivity.java**:
   ```java
   package com.yourname.optixpro;
   
   import android.os.Bundle;
   import android.webkit.WebSettings;
   import android.webkit.WebView;
   import android.webkit.WebViewClient;
   import androidx.appcompat.app.AppCompatActivity;
   
   public class MainActivity extends AppCompatActivity {
       private WebView webView;
       
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);
           
           webView = findViewById(R.id.webview);
           WebSettings settings = webView.getSettings();
           settings.setJavaScriptEnabled(true);
           settings.setDomStorageEnabled(true);
           settings.setCacheMode(WebSettings.LOAD_DEFAULT);
           
           webView.setWebViewClient(new WebViewClient());
           webView.loadUrl("https://YOUR_DOMAIN/index.html");
       }
       
       @Override
       public void onBackPressed() {
           if (webView.canGoBack()) {
               webView.goBack();
           } else {
               super.onBackPressed();
           }
       }
   }
   ```

4. **Update activity_main.xml**:
   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <WebView xmlns:android="http://schemas.android.com/apk/res/android"
       android:id="@+id/webview"
       android:layout_width="match_parent"
       android:layout_height="match_parent" />
   ```

5. **Build** → Generate Signed APK

---

## 📁 Required Files for PWA

Ensure these files are in your project:

```
optix-pro/
├── index.html           # Main app
├── manifest.json        # PWA manifest
├── service-worker.js    # Offline support
├── generate-icons.html  # Icon generator tool
└── icons/               # App icons (create this folder)
    ├── icon-72x72.png
    ├── icon-96x96.png
    ├── icon-128x128.png
    ├── icon-144x144.png
    ├── icon-152x152.png
    ├── icon-192x192.png
    ├── icon-384x384.png
    └── icon-512x512.png
```

### Generate Icons

1. Open `generate-icons.html` in a browser
2. Click "Download All Icons"
3. Create `icons/` folder and place all icons there

---

## 🔐 Signing Your APK (For Private Distribution)

For private distribution, you need to sign your APK:

### Using keytool (Command Line)
```bash
# Generate keystore
keytool -genkey -v -keystore optix-pro.keystore -alias optixpro -keyalg RSA -keysize 2048 -validity 10000

# Sign APK
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore optix-pro.keystore app-release-unsigned.apk optixpro

# Verify signature
jarsigner -verify -verbose -certs app-release-signed.apk
```

### Using Android Studio
1. Build → Generate Signed Bundle/APK
2. Select APK
3. Create new keystore or use existing
4. Build Release APK

---

## 📲 Installing on Android Device

1. **Enable Unknown Sources**:
   - Settings → Security → Unknown Sources → Enable
   - Or Settings → Apps → Special Access → Install Unknown Apps

2. **Transfer APK**:
   - USB cable
   - Cloud storage (Drive, Dropbox)
   - Direct download from your server

3. **Install**:
   - Open file manager
   - Navigate to APK
   - Tap to install

---

## 🔄 Updating the App

### For PWA-based APKs:
- Simply update the web app files on your server
- The service worker will detect and apply updates

### For Native Wrapper:
- Rebuild and redistribute the APK
- Users must manually update

---

## 🛠 Troubleshooting

### "App not installed" error
- Ensure the APK is properly signed
- Check for conflicting package names
- Uninstall previous version first

### WebView shows blank screen
- Check internet permission in manifest
- Verify JavaScript is enabled
- Test URL in browser first

### Install prompt doesn't appear
- Ensure HTTPS is used
- Verify manifest.json is valid
- Check service worker is registered

---

## 📋 Checklist Before Distribution

- [ ] Icons generated and placed in `icons/` folder
- [ ] manifest.json has correct start_url and scope
- [ ] service-worker.js is working
- [ ] App works offline
- [ ] APK is signed
- [ ] Tested on multiple Android versions
- [ ] Privacy policy URL (if publishing to store)

---

## 🔗 Useful Resources

- [PWABuilder](https://www.pwabuilder.com/)
- [Bubblewrap GitHub](https://github.com/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder/AAAbuilder)
- [Android WebView Docs](https://developer.android.com/reference/android/webkit/WebView)
- [PWA Documentation](https://web.dev/progressive-web-apps/)

---

**Need help?** The easiest path is:
1. Deploy to GitHub Pages (free)
2. Use PWABuilder to generate APK
3. Distribute via direct download or file sharing
