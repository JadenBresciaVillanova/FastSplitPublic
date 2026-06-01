# Divvy — Apple Review History
**App:** Divvy (com.jadenbrescia.divvy2)
**Last updated:** May 27, 2026

This document summarizes all issues raised during App Store review and how each was resolved. It is maintained as a reference for reviewers and is attached to each submission.

---

## Resolved Issues & Fixes

### 1. Privacy Purpose Strings — Camera & Photo Library
**Problem:** Purpose strings for camera and photo library access did not sufficiently explain how the data would be used or provide specific examples.
**Fix:** Rewrote both strings with detailed explanations including real-world examples (e.g., "after dining out you can choose a photo of your restaurant receipt"), how the image is processed, and what is/isn't stored on our servers.
**Reviews:** Raised in #1 (Apr 7) · Resolved in #2 (Apr 10)

### 2. App Unresponsive on iPad After Login
**Problem:** The app became completely unresponsive to taps after login when reviewed on iPad Air. The UI was designed exclusively for iPhone and did not handle the iPad layout.
**Fix:** Initially set `supportsTablet: false`. Later re-enabled full iPad support (`supportsTablet: true` since Build 20) with responsive scaling via a `useScale` hook and ScrollView wrapping on the login screen.
**Reviews:** Raised in #1 (Apr 7) · Resolved in #2 (Apr 10) · Revisited in #6–#7 with full iPad support

### 3. Ad System — No Ads Displayed / Loading Indefinitely
**Problem:** Ad networks (Google AdMob, Unity Ads) do not serve ads in Apple's review environment. The original single-ad implementation had no fallback, making the button appear broken. Later, the 4-tier cascade took ~96 seconds to complete, appearing frozen.
**Fix:** Built a 4-tier ad cascade (rewarded video → rewarded interstitial → interstitial → free daily pass) with 6-second timeouts, a visible countdown timer, and progressive status messages. A demo account with 100 pre-loaded credits is provided in Sign-in Required fields for full testing without ads.
**Reviews:** Raised in #2 (Apr 10), #3 (Apr 21), #4 (Apr 23) · Resolved in #5 (Apr 23)

### 4. ATT Pre-Prompt — Encouraged Tracking
**Problem:** The ATT pre-prompt used persuasive language ("Allow Personalized Ads" button, "Help Keep Divvy Free" title, hint to "tap Allow") and allowed users to skip the system ATT dialog entirely.
**Fix:** Rewrote the pre-prompt with neutral language. Title changed to "About Ads in Divvy." Single "Continue" button that always proceeds to the system ATT dialog. Added disclaimer: "Tapping Continue does not enable tracking." No option to skip the system dialog.
**Reviews:** Raised in #5 (Apr 23) · Resolved in #6 (Apr 25)

### 5. Google Sign-In Opens Safari
**Problem:** The `@react-native-google-signin/google-signin` library used Google's native iOS SDK, which opened Safari for OAuth instead of staying in-app.
**Fix:** Replaced with `expo-auth-session/providers/google` + `expo-web-browser`, which uses `ASWebAuthenticationSession` — Apple's own secure in-app authentication API. The OAuth flow is presented as an in-app modal sheet; the user never leaves the app.
**Reviews:** Raised in #6 (Apr 25), re-raised in #7 (Apr 28) · Resolved in #7 with video evidence showing ASWebAuthenticationSession is in-app

### 6. iPad Layout — Sign-Up Toggle Not Visible
**Problem:** On iPad Air 11-inch, the "Don't have an account? Sign Up" toggle on the login screen was clipped off-screen due to fixed pixel dimensions and no scroll support.
**Fix:** Added ScrollView wrapping to the login screen with `flexGrow: 1` centering. On iPhone, visually identical (content fits without scrolling). On iPad, content centers when it fits and scrolls when it overflows. Sign-up toggle has `paddingBottom: 40` for breathing room.
**Reviews:** Raised in #6 (Apr 25), re-raised in #7 (Apr 28) · Resolved in #7 with video evidence on physical iPad

### 7. ATT Prompt Not Found by Reviewer
**Problem:** The ATT permission request is triggered when the user taps "Watch Ad for +1 Credit." If the reviewer does not tap this button, the ATT prompt does not appear. No tracking occurs before the prompt — the ad SDK serves only non-personalized contextual ads until ATT is authorized.
**Fix:** No code change required. The ATT implementation is correct and has been in place since Review #5. Responded with video evidence showing the full ATT flow on iPad: tap "Watch Ad" → custom pre-prompt → system ATT dialog.
**Reviews:** Raised in #4 (Apr 23), re-raised in #8 (May 25) · Responded with video evidence both times

### 8. Camera/Photo Permissions Requested Too Early
**Problem:** Camera and photo library permissions were requested on app launch, before the user interacted with any feature that needed them.
**Fix:** Moved permission requests to the point of use — camera permission is requested only when the user taps "Take Photo," and photo library only when the user taps "Upload Photo."
**Reviews:** Self-identified during #4 fixes · Included in #5 submission (Apr 23)

---

## Review Timeline

| # | Date | Build | Result | Issues |
|---|------|-------|--------|--------|
| 1 | Apr 7, 2026 | 14 | Rejected | Privacy purpose strings, app unresponsive on iPad |
| 2 | Apr 10, 2026 | 14 | Rejected | No ads displayed |
| 3 | Apr 21, 2026 | 15 | Rejected | Ad loading screen appears frozen (~96s) |
| 4 | Apr 23, 2026 | 16 | Rejected | No ads displayed (expected in review), ATT prompt not found |
| 5 | Apr 23, 2026 | 18 | Rejected | ATT pre-prompt encourages tracking |
| 6 | Apr 25, 2026 | — | Rejected | Google Auth opens Safari, iPad sign-up toggle clipped |
| 7 | Apr 28, 2026 | 20 | Rejected | Google Auth (re-raised, was ASWebAuthenticationSession), iPad toggle (re-raised) |
| 8 | May 25, 2026 | 22 | Rejected → **Approved (May 27)** | ATT prompt not found (re-raised) — responded with video evidence, no code change |
| 9 | May 27, 2026 | 22 | Pending | Metadata-only: added Marketing URL for AdMob app-ads.txt verification. No code changes, same build. |

---

## Notes for Reviewers

- **Demo account:** Credentials are in the Sign-in Required fields (divvyapplereview@gmail.com). This account has pre-loaded scan credits so you can test all features without watching ads.
- **Ad behavior during review:** Ad networks do not serve ads in the review environment. The app handles this gracefully — the ad cascade runs for ~34 seconds with a countdown timer, then grants one free daily credit.
- **ATT prompt location:** The ATT pre-prompt and system dialog appear the first time a user taps "Watch Ad for +1 Credit" on the home screen. No tracking data is collected before this point.
- **iPad support:** The app fully supports iPad with responsive layouts (`supportsTablet: true` since Build 20, Review #7).
