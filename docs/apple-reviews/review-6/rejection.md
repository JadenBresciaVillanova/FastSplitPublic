# Apple Review #6 — Rejected
**Date:** April 25, 2026
**Version:** 1.5
**Review Devices:** iPhone 17 Pro Max, iPad Air 11-inch (M3)
**Result:** REJECTED

---

## Rejection Reasons

### Issue 1: Guideline 4 — Design (Google Auth opens Safari)

**Apple's complaint:** When a user signs in with Google, the app opens Safari browser for authentication instead of handling it within the app.

**Specifics:** The `@react-native-google-signin/google-signin` library uses the Google Sign-In iOS SDK, which internally launches an ASWebAuthenticationSession or redirects to Safari for the OAuth flow. Once authentication completes, the user is redirected back to the app via deep link.

**Apple's expectation:** Authentication should happen within the app (e.g., using an in-app web view or native authentication session) without visibly leaving the app to open Safari.

### Issue 2: Guideline 4 — Design (iPad layout)

**Apple's complaint:** On iPad Air 11-inch (M3), the sign-up option is not fully visible. The layout does not properly accommodate the iPad screen.

**Specifics:** The app has `"supportsTablet": false` in app.json, meaning it's declared as iPhone-only. However, Apple still tests iPhone-only apps running in iPad compatibility mode and flags layout issues.

---

## Issues Resolved From Review #5
- **5.1.1(iv) ATT pre-prompt — NOT MENTIONED.** Apple did not raise this issue again. The revised neutral ATT pre-prompt with "Continue" button appears to have satisfied this concern.

---

## Root Cause Analysis

### Google Auth / Safari
The `@react-native-google-signin/google-signin` package (v16.1.2) uses Google's native iOS Sign-In SDK (v9.0). On iOS, this SDK calls `GIDSignIn.sharedInstance.signIn(withPresenting:)`, which **opens the full Safari browser app** for the OAuth flow rather than presenting an in-app authentication sheet. After authentication, the user is deep-linked back to the app via the configured URL scheme. Apple correctly flagged this as a UX issue.

### iPad Layout
Even with `supportsTablet: false`, iPhone apps can run on iPad in a scaled/compatibility window. The login screen used fixed sizing (300px logo, `justifyContent: 'center'`) that works well on iPhone but caused the bottom toggle text ("Don't have an account? Sign Up") to be clipped off-screen in iPad compatibility mode.

---

## Fixes Applied

### Issue 1 — Google Auth: Replaced native SDK with expo-auth-session

**Before (opened Safari):**
- Used `@react-native-google-signin/google-signin` (v16.1.2) with Google's native iOS SDK
- Called `GoogleSignin.signIn()` which invoked `GIDSignIn.sharedInstance.signIn(withPresenting:)`
- This opened the full Safari browser app for OAuth, then deep-linked back to Divvy
- User visibly left the app during authentication

**After (stays in-app):**
- Replaced with `expo-auth-session/providers/google` + `expo-web-browser`
- `expo-auth-session` uses `ASWebAuthenticationSession` (Apple's own secure in-app authentication API)
- OAuth is presented as an in-app modal sheet — the user never leaves the app
- `WebBrowser.maybeCompleteAuthSession()` handles the redirect callback within the app
- The `id_token` is passed to Firebase `GoogleAuthProvider.credential()` → `signInWithCredential()`

### Issue 2 — iPad Layout: Responsive scaling via useScale hook

**Before:**
- Login screen used fixed pixel values: 300x300 logo, -30px margin, 54px back button offset
- On iPad compatibility mode, this caused the "Sign Up" toggle at the bottom to be clipped offscreen

**After:**
- Created `useScale.js` hook — returns `vScale` (ratio of current screen height to iPhone X baseline of 812pt)
- Logo size: `300 * vScale` (shrinks proportionally on tighter screens)
- All fixed vertical spacing scaled by `vScale`
- On standard iPhone (812pt), `vScale ≈ 1.0` so nothing changes visually
- On iPad compatibility mode or smaller devices, elements scale down to fit

---

## Our Response

### Issue 1 — Google Auth

Hi,

Thank you for the feedback regarding Google Sign-In.

We identified and resolved the issue. Our previous implementation used the @react-native-google-signin/google-signin library (v16.1.2) with Google's native iOS Sign-In SDK, which opened the Safari browser for OAuth authentication.

We have replaced this with expo-auth-session, which uses ASWebAuthenticationSession — Apple's recommended secure in-app authentication API. The Google Sign-In flow now presents an in-app modal sheet. The user never leaves the app at any point during authentication.

Technical details of the change:
- Removed: @react-native-google-signin/google-signin (native SDK that opened Safari)
- Added: expo-auth-session/providers/google + expo-web-browser (in-app ASWebAuthenticationSession)
- The OAuth token is still passed to Firebase Authentication via GoogleAuthProvider.credential(), so the sign-in result is identical — only the presentation layer changed.

We have included a screen recording showing the updated Google Sign-In flow to demonstrate that it now stays entirely within the app.

### Issue 2 — iPad Layout

We identified the layout issue on iPad Air 11-inch in compatibility mode. The login screen used fixed pixel dimensions (300px logo, fixed margins) that caused the "Sign Up" toggle to be clipped offscreen.

We implemented a responsive scaling system that adjusts vertical spacing and element sizes based on screen height. The login screen now fits correctly across all device sizes, including iPad compatibility mode. The "Sign Up" option is fully visible and accessible.

Best regards,
Jaden Brescia
