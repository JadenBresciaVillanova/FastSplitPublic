# Apple Review #9 — Metadata-Only Submission
**Date:** May 27, 2026
**Version:** 1.7 (Build 22 — same binary as v1.6)
**Result:** Pending
**Changes:** Marketing URL added; no code changes

---

## Purpose

Version 1.7 is being submitted solely to add the **Marketing URL** field (`https://divvy-app-491221.web.app`) to the App Store listing. This is required for Google AdMob app-ads.txt verification, which is currently blocking ad delivery in the production app.

### Why This Is Needed

- AdMob requires a **Developer Website** link on the App Store listing to crawl and verify the `app-ads.txt` file
- The Marketing URL field was not set in v1.6, so AdMob cannot verify the app
- Without verification, the app's AdMob approval status remains "Requires review" and ads are not being served to users
- The `app-ads.txt` file has been published at `https://divvy-app-491221.web.app/app-ads.txt` via Firebase Hosting

### What Changed

| Change | Details |
|--------|---------|
| Marketing URL | Added `https://divvy-app-491221.web.app` |
| Code changes | None |
| New build | No — same Build 22 (1.0.0) from v1.6 |
| New features | None |
| Bug fixes | None |

---

## Our Response

Hi,

This submission updates the app from version 1.6 to 1.7. The sole purpose of this update is to add a Marketing URL (https://divvy-app-491221.web.app) to the App Store listing. This is required by Google AdMob for app-ads.txt verification, which enables ad serving in our app.

To confirm — nothing has changed from the approved v1.6:

- No changes to App Tracking Transparency (ATT) implementation or pre-prompt
- No changes to authentication (Apple Sign-In, Google Sign-In via ASWebAuthenticationSession, email/password)
- No changes to camera, photo library, or contacts permission handling
- No changes to iPad layout or responsive scaling
- No changes to the ad cascade system or free daily pass fallback
- No changes to UI, features, or app behavior

Thank you for your time.

Best regards,
Jaden Brescia
