# Apple Review #2 — Rejected
**Date:** April 10, 2026 (response received April 11)
**Version:** 1.0 (Build 14, resubmission)
**Review Device:** iPad Air 11-inch (M3), iPadOS 26.4.1
**Result:** REJECTED

---

## Rejection Reason

### Guideline 2.1(a) — Performance: App Completeness

**Issue:** The app still exhibited one or more bugs that would negatively impact users.

**Bug description:** No ads were displayed in the app after we tapped on the "Watch Ad for +1 Credit" button.

**Review device details:**
- Device type: iPad Air 11-inch (M3)
- OS version: iPadOS 26.4.1
- Internet Connection: Active

**Note from Apple:** The issue we previously identified still needs your attention.

---

## Issues Resolved From Review #1
- **5.1.1(ii) Privacy Purpose Strings — PASSED.** Not mentioned in this review, confirming the rewritten camera and photo library purpose strings were accepted.
- **2.1(a) iPad Unresponsive — PASSED.** The original "app became completely unresponsive to taps after login" bug was not mentioned. However, a new 2.1(a) issue was raised (ads not loading).

---

## Root Cause Analysis

Ad networks (Google AdMob, Unity Ads) do not serve ads in Apple's review sandbox environment. The previous build had no fallback mechanism — when the single rewarded video ad failed to load, the button did nothing.

Additionally, Apple continued testing on iPad despite `supportsTablet: false` being set. iPhone apps can still run in compatibility mode on iPad.

---

## Fixes Applied (for next submission)

### Complete ad system rebuild:
1. **4-tier ad cascade** — Rewarded Video -> Rewarded Interstitial -> Regular Interstitial -> Free Pass. Each tier retries before falling through.
2. **Free pass system** — If all ad tiers fail (as in Apple's review sandbox), the user gets 1 free scan credit per day.
3. **Progressive status messages** — Loading overlay shows changing text as tiers cascade: "Loading ad..." -> "Trying another ad format..." -> "Almost there, checking last option..." -> "No ads available, granting free credit..."
4. **Server-side verification (SSV)** — Signed callbacks from Google, with fallback if SSV times out.
5. **Rate limiting** — Max 3 unverified credits per day.
6. **AdMob mediation** — Unity Ads linked as mediation partner for broader ad fill.
7. **SKAdNetwork IDs** — 40 network IDs added for ad attribution.

### Additional improvements:
- App Tracking Transparency (ATT) pre-prompt
- Contacts pre-prompt
- Login loading modal (prevents unresponsive appearance)
- Tutorial navigation fix
- Splash animation
- Updated privacy policy with IDFA/tracking disclosures
- Updated App Store Connect privacy declarations

### Review notes added:
Explaining that ad networks don't serve in review environment, expected cascade timing, and free credit is granted automatically.

---

## Our Response

Hi,

Thank you for the continued feedback. The ad loading issue has been fully resolved in this new build.

Root cause: Ad networks (Google AdMob, Unity Ads) do not serve ads in Apple's review environment. Our previous build had a single ad format with no fallback, so when the ad failed to load, the button appeared to do nothing.

What we changed:

1. 4-tier ad cascade with automatic fallback. When the user taps "Watch Ad for +1 Credit," the app now tries four ad formats in sequence — rewarded video, rewarded interstitial, regular interstitial, and finally a free daily pass. Each tier retries before falling through. The user sees progressive status messages throughout ("Loading ad...", "Trying another ad format...", "Almost there...", "No ads available, granting free credit...") so it's clear the app is actively working.

2. Free pass system. If no ads load (as will happen in your review environment), the app automatically grants 1 free scan credit per day. The user sees a clear confirmation: "Ad unavailable. Free pass granted (1 of 1 today)." This ensures the core functionality — scanning and splitting receipts — always works regardless of ad availability.

3. Additional improvements since last submission: App Tracking Transparency prompt, server-side ad verification, AdMob mediation with Unity Ads, connectivity checks, loading overlay with status updates, rate limiting, and updated privacy disclosures.

Expected behavior during review: When you tap "Watch Ad for +1 Credit," the app will cycle through ad tiers (each timing out quickly since no ads will fill in the review environment), then grant a free pass. The total wait is approximately 20-25 seconds with status messages updating throughout. The app will then have 1 scan credit available for testing the full receipt scanning flow.

To test the full app flow:
1. Tap "Watch Ad for +1 Credit" — wait for the cascade to complete — a free credit will be granted
2. Tap "Upload or Take Photo" to scan a receipt
3. The AI will extract all items, prices, tax, and totals
4. Add friends and assign items to split the bill

Please note this app is iPhone-only (supportsTablet is set to false).

Thank you for your time. Please let me know if anything else is needed.

Best regards,
Jaden Brescia
