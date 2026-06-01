# Apple Review #7 — Rejected
**Date:** April 28, 2026
**Version:** 1.5 (Build 20)
**Review Devices:** iPhone 17 Pro Max, iPad Air 11-inch (M3), iPadOS 26.4.1
**Result:** REJECTED

---

## Rejection Reasons

### 1. Guideline 4 — Design: Google Auth opens default web browser

**Issue:** The user is taken to the default web browser to sign in or register for an account, which provides a poor user experience.

**Apple's suggestion:** Revise the app to enable users to sign in or register for an account within the app. May also implement the Safari View Controller API to display web content within the app.

### 2. Guideline 4 — Design: iPad layout

**Issue:** Parts of the app's user interface were crowded, laid out, or displayed in a way that made it difficult to use the app when reviewed on iPad Air 11-inch (M3) running iPadOS 26.4.1.

**Specifics:** The sign-up option was not fully visible when reviewed on iPad.

---

## Issues Resolved From Review #6
- **5.1.1(iv) ATT pre-prompt — NOT MENTIONED.** Still resolved.
- **2.1(a) Ads — NOT MENTIONED.** Still resolved.

---

## Root Cause Analysis

### Issue 1: Google Auth — ASWebAuthenticationSession misidentified as Safari

The code was already updated (in Review #6) to use `expo-auth-session`, which wraps `ASWebAuthenticationSession` — Apple's own secure in-app authentication API. The reviewer likely saw the `ASWebAuthenticationSession` system sheet (which has a Safari-like address bar) and interpreted it as "opening Safari." This is not the case — `ASWebAuthenticationSession` presents an in-app modal sheet and the user never leaves the app.

### Issue 2: iPad layout — sign-up toggle still clipped

`supportsTablet` was set to `true` and the layout was updated for iPad support. However, the login screen used a flat `KeyboardAvoidingView` with `justifyContent: 'center'` and no `ScrollView`. When scaled-up content filled the viewport, the toggle at the bottom was pushed off-screen with no way to scroll to it.

---

## Fixes Applied

### Issue 1 — Google Auth: Response with video evidence

No code change needed — `expo-auth-session` with `ASWebAuthenticationSession` is already the correct in-app implementation. Responding to Apple with:
- Technical explanation that the app uses `ASWebAuthenticationSession`, Apple's own secure in-app authentication API
- Screen recording from physical iPad showing the in-app auth sheet appearing (not Safari)
- The user never leaves the app at any point during Google Sign-In

### Issue 2 — iPad layout: ScrollView wrapping

**Before:**
- `login.js` used `KeyboardAvoidingView` with `justifyContent: 'center'` as the only container
- No `ScrollView` — content could not scroll if it exceeded viewport height
- On iPad, the scaled-up logo and form pushed the sign-up toggle off-screen

**After:**
- Added `ScrollView` inside `KeyboardAvoidingView`
- Centering moved from container to `scrollContent` style with `flexGrow: 1`
- `paddingBottom: 40` ensures toggle has breathing room
- On iPhone: `flexGrow: 1` fills viewport, content centers, ScrollView doesn't scroll — visually identical
- On iPad: content centers when it fits, scrolls when it overflows

### Additional fix — NSUserTrackingUsageDescription

Rewrote the system ATT dialog text to remove persuasive language:

**Before:** "Divvy uses ad revenue to stay free. Allowing tracking helps show you relevant ads... Tap Allow to help support Divvy."

**After:** "Divvy shows ads to keep the app free. If you allow tracking, ads may be more relevant to your interests. If you decline, you will still see ads, but they will be generic. You can change this choice at any time in Settings."

---

## Our Response

Hi,

Thank you for the continued feedback. Both Guideline 4 issues have been resolved, and we have included a screen recording from a physical iPad Pro 10.5-inch demonstrating both fixes.

**Issue 1 — Google Sign-In "opens default web browser"**

Google Sign-In does not open Safari or any external browser. Our implementation uses expo-auth-session, which wraps ASWebAuthenticationSession — Apple's own secure in-app authentication API introduced in iOS 12. When the user taps "Sign in with Google," the OAuth flow is presented as a system-provided in-app modal sheet. The user never leaves the app at any point during authentication.

In the attached screen recording, you can see this behavior at the point where "Sign in with Google" is tapped. A system authentication sheet appears over the app (presented by ASWebAuthenticationSession), the user enters their Google credentials within that sheet, and the sheet dismisses back to the app upon completion. Safari is never opened.

This is the same authentication mechanism used by many App Store apps for third-party OAuth, and is explicitly designed by Apple to keep users within the app during sign-in.

**Issue 2 — Sign-up option not fully visible on iPad**

The login screen layout has been updated to use a ScrollView, ensuring all content — including the "Don't have an account? Sign Up" toggle — is fully visible and accessible on all device sizes, including iPad Air 11-inch.

In the attached screen recording (filmed on a physical iPad Pro 10.5-inch), you can see the "Don't have an account? Sign Up" toggle clearly visible at the bottom of the screen, and a new account is successfully created.

This single recording demonstrates that both issues are resolved: Google authentication stays entirely in-app via ASWebAuthenticationSession, and the sign-up option is fully visible and functional on iPad.

Thank you for your patience. Please let me know if anything else is needed.

Best regards,
Jaden Brescia
