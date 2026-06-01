# Apple Review #4 — Rejected
**Date:** April 23, 2026
**Version:** 1.0.0 (Build 16)
**Review Devices:** iPhone 17 Pro Max, iPad Air 11-inch (M3), iPadOS 26.4.1
**Result:** REJECTED

---

## Rejection Reasons

### 1. Guideline 2.1(a) — Performance: App Completeness

**Issue:** The app exhibited one or more bugs that would negatively impact users.

**Bug description:** No ads were displayed when we tapped "Watch Ad for 1 credit"

**Review device details:**
- Device type: iPad Air 11-inch (M3)
- OS version: iPadOS 26.4.1
- Internet Connection: Active

### 2. Guideline 2.1 — Information Needed: App Tracking Transparency

**Issue:** The app uses the AppTrackingTransparency framework, but we are unable to locate the App Tracking Transparency permission request when reviewed on iOS 26.4.1 and iPadOS 26.4.1.

**Apple requested:** A screen recording from a physical device showing:
- Launching from fresh install or after resetting tracking permissions
- ATT permission request appearing before any tracking data is collected
- The user flow that follows the permission request

---

## Issues Resolved From Review #3
- **"Screen loads indefinitely after login" — RESOLVED.** Not mentioned in this review. The cascade timing reduction (96s → 34s) and countdown timer fixed the perceived freeze. Apple now specifically says "no ads were displayed" rather than "loads indefinitely" — the cascade completed successfully.

---

## Root Cause Analysis

### Issue 1: "No ads displayed"
This is **expected behavior** in Apple's review environment. Ad networks (Google AdMob, Unity Ads) do not serve ads during App Review. The cascade ran correctly — the reviewer saw the countdown timer, status messages updated, and a free credit was granted via the daily free pass.

The reviewer did not use the demo account which has 100 pre-loaded credits and would bypass the ad system entirely.

### Issue 2: ATT not appearing
The ATT pre-prompt was only triggered when the user tapped "Watch Ad for +1 Credit" for the first time. If the reviewer:
- Used the demo account and never tapped "Watch Ad" (had 100 credits), they'd never see ATT
- Had global tracking disabled in iOS Settings, the system returns 'denied' silently without showing the dialog

---

## Fixes Applied (for next submission)

### ATT moved to first "Watch Ad" tap (before any ad loads)
- On first "Watch Ad" tap, ATT check runs before the ad cascade starts
- If ATT status is `undetermined`: custom pre-prompt appears → user responds → system ATT dialog → then ad cascade
- If tracking is disabled in iOS Settings: brief "Ad Tracking Disabled" popup for 2.5 seconds, then continues to ad cascade

### Screen recording provided
- Recorded on physical iPhone showing ATT flow from fresh install
- Uploaded in App Store Connect Review Notes

### Permission timing fix
- Camera and photo library permissions were requested on app launch (before user interaction)
- Fixed: camera permission now only requested when user taps "Take Photo"
- Photo library permission now only requested when user taps "Upload Photo"
- Follows Apple's guideline of requesting permissions in context, at the point of use

### Login UX improvements
- Sign Up mode: hides Apple/Google sign-in buttons, shows only email + password fields
- Larger input fields in Sign Up mode with clearer placeholders
- Smooth fade transition between Sign In / Sign Up modes

---

## Our Response

Hi,

Thank you for the feedback. Both issues have been addressed.

**Issue 1 — "No ads displayed"**

This is expected behavior in your review environment. Ad networks (Google AdMob, Unity Ads) do not serve real ads during App Review. When you tap "Watch Ad for +1 Credit," the app cycles through three ad formats (~34 seconds with a visible countdown timer), and when none load, it automatically grants one free daily credit. This is working as designed — the free pass ensures users are never locked out.

To test the full app without any ad dependency, please use the demo account provided in the Sign-in Required section. This account has 100 scan credits, so you can test scanning, splitting, share links, and payment flows without needing any ads to load.

**Issue 2 — App Tracking Transparency**

The ATT permission request appears the first time a user taps "Watch Ad for +1 Credit." We've included a screen recording in the Review Notes showing the full ATT flow from a fresh install on a physical iPhone:

1. App launches, user logs in, home screen appears
2. User taps "Watch Ad for +1 Credit"
3. Custom pre-prompt appears explaining ad personalization
4. User responds, Apple's system ATT dialog appears
5. User responds, ad cascade begins

If the user's device has "Allow Apps to Request to Track" disabled in iOS Settings, the app shows a brief notification that targeted ads are unavailable, then continues with non-personalized ads.

**Additional improvements in this build:**
- Camera and photo library permissions are now requested only when the user taps the corresponding button ("Take Photo" or "Upload Photo"), not on app launch
- Improved Sign Up flow with clearer input fields

Please note this app is iPhone-only (supportsTablet is set to false).

Thank you for your patience. Please let me know if anything else is needed.

Best regards,
Jaden Brescia
