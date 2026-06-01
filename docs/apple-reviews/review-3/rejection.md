# Apple Review #3 — Rejected
**Date:** April 21, 2026
**Version:** 1.0.0 (Build 15)
**Review Device:** iPad Air (5th generation), iPadOS 26.4.1
**Result:** REJECTED

---

## Rejection Reason

### Guideline 2.1(a) — Performance: App Completeness

**Issue:** The app exhibited one or more bugs that would negatively impact users.

**Bug description:** Screen loads indefinitely after login

**Review device details:**
- Device type: iPad Air (5th generation)
- OS version: iPadOS 26.4.1
- Internet Connection: Active

**Note from Apple:** The issues we previously identified still need your attention.

---

## Issues Resolved From Review #2
- **2.1(a) "No ads displayed" — PARTIALLY RESOLVED.** The 4-tier ad cascade and free pass system were in place and functioning. However, the per-ad-load timeout (15 seconds) combined with retries meant the cascade took ~96 seconds in Apple's sandbox where no ads load. The reviewer saw the "Loading Ad" overlay for over a minute, which appeared as "loads indefinitely."

---

## Root Cause Analysis

The rejection message says "screen loads indefinitely after login," but the attached screenshot shows the ad loading overlay. The reviewer tapped "Watch Ad for +1 Credit" after logging in, and the ad cascade appeared stuck.

**The actual problem:** Each of the 3 ad tiers had a 15-second load timeout with 1 retry per tier. In Apple's review sandbox, no ads ever load, so every timeout was hit:
- Tier 1 (rewarded video): 2 attempts × 15s + 2s delay = 32s
- Tier 2 (rewarded interstitial): 2 attempts × 15s + 2s delay = 32s
- Tier 3 (interstitial): 2 attempts × 15s + 2s delay = 32s
- **Total: ~96 seconds** before the free pass tier

With no visual countdown or progress indicator beyond status text changes, 96 seconds of waiting looked like the app was frozen.

---

## Fixes Applied (for next submission)

### Ad cascade timing — dramatically reduced
1. **Load timeout reduced** from 15s to 6s per attempt (`AD_LOAD_TIMEOUT_MS`)
2. **Retries reduced** — tiers 1 & 2 (rewarded ads) get 1 retry each, tier 3 gets 0 retries
3. **New worst case: ~34 seconds** instead of ~96 seconds
   - Tier 1: 6s + 2s delay + 6s retry = 14s
   - Tier 2: 6s + 2s delay + 6s retry = 14s
   - Tier 3: 6s (no retry) = 6s

### Visual countdown timer
- Ad loading overlay now shows a **countdown timer** (6s → 5s → 4s → 3s → 2s → 1s) for each attempt
- Countdown resets when the status changes to the next tier or retry
- Reviewer sees clear progress: the number ticks down, status text updates, cycle repeats

### Demo account provided
- Created a pre-loaded test account with 100 scan credits and pre-scanned receipts
- Credentials provided in App Store Connect "Sign-in Required" fields
- Reviewer can test all features without needing any ads to load

---

## Our Response

Hi,

Thank you for the feedback. The loading issue has been resolved — the ad loading overlay was taking too long in your review environment. Here's what changed:

**Root cause:** Our ad cascade had a 15-second timeout per ad format with retries, totaling ~96 seconds when no ads load (as in your review environment). This made the screen appear frozen.

**What we fixed:**

1. **Dramatically reduced cascade timing.** Cut the per-tier timeout from 15 seconds to 6 seconds and reduced retries. The full cascade now completes in ~34 seconds (down from ~96 seconds). Each tier shows a visible countdown timer (6s → 5s → 4s → 3s → 2s → 1s) so it's clear the app is actively working, not stuck.

2. **Provided a demo account.** We've included a pre-loaded test account in the Sign-in Required fields with 100 scan credits and pre-scanned receipts. You can use this account to test every feature — scanning, splitting, share links, payment links, and history — without needing any ads to load.

**To test with the demo account (recommended):**
1. Sign in using the credentials provided in the Sign-in Required section
2. The account has 100 credits and existing receipts in History
3. You can scan new receipts, split items, create share links, and test payments
4. No ads are needed — all features work directly with the pre-loaded credits

**To test the ad cascade (optional):**
1. Tap "Watch Ad for +1 Credit" on the home screen
2. The cascade will cycle through ad formats (~34 seconds with a countdown timer)
3. Since no ads load in the review environment, a free daily credit is granted automatically

Please note this app is iPhone-only (supportsTablet is set to false).

Thank you for your patience. Please let me know if anything else is needed.

Best regards,
Jaden Brescia
