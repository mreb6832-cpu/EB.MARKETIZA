# E&B MARKETIZA — PWA + Android app source

This package keeps the existing marketplace pages and integrations, and adds:
- Installable PWA manifest and app icons.
- Service worker with conservative caching (payment, account, cart, login and API requests are not cached).
- PWA metadata and service-worker registration on the HTML pages.
- Capacitor configuration for generating an Android native project.

## 1) Deploy the PWA
1. Upload/replace the repository contents with this folder's contents.
2. Deploy on HTTPS (the existing Vercel domain is suitable).
3. Open the deployed site in Chrome on Android and use **Install app** / **Add to Home screen**.
4. On iPhone, open the site in Safari → Share → **Add to Home Screen**.

PWA installation on iPhone is through Safari; it does not create an App Store listing.

## 2) Generate and build the Android APK
Requirements: Node.js LTS, Android Studio, Android SDK and a JDK supported by your installed Android Gradle Plugin.

From this folder:
```bash
npm install
npx cap add android
npx cap sync android
npx cap open android
```
In Android Studio, wait for Gradle sync, then choose **Build → Build Bundle(s) / APK(s) → Build APK(s)**.
The debug APK is produced under `android/app/build/outputs/apk/debug/`.
For public distribution, create a signed release APK or AAB using **Build → Generate Signed Bundle / APK**.

## 3) iPhone native App Store build (optional)
PWA already works on iPhone via Safari. To create a native iOS project/App Store build, use macOS with Xcode:
```bash
npm install
npx cap add ios
npx cap sync ios
npx cap open ios
```
Then configure signing, bundle ID and App Store Connect in Xcode.

## Important checks before production
- Confirm the live website domain is the intended canonical domain.
- Test Supabase authentication, storage and Row Level Security.
- Test Stripe checkout and webhook in Stripe test mode, then live mode.
- Confirm all required Vercel environment variables are set.
- Test install, update, navigation and payment on real Android and iPhone devices.

## Offline behavior
The service worker caches the app shell and selected static assets. It does not make checkout, authentication, cart/account data, or Supabase/Stripe services work offline. A real purchase still requires network access.
