# Apple Review #8 — Rejected → Approved
**Date:** May 25, 2026
**Version:** 1.6 (Build 22)
**Review Device:** iPad Air 11-inch (M3), iPadOS 26.5
**Result:** REJECTED → **APPROVED (May 27, 2026)**

---

## Rejection Reason

### 1. Guideline 2.1 — Performance: App Completeness (ATT prompt not appearing)

**Issue:** The app uses the AppTrackingTransparency framework, but the ATT permission request is not appearing when reviewed on iPadOS 26.5.

**Apple's suggestion:** If ATT is implemented but the permission request is not appearing on the latest OS, review documentation and confirm correct implementation. Reply with a screen recording from a physical device showing the ATT flow.

---

## Issues Resolved From Review #7
- **Guideline 4 (Google Auth opens browser) — NOT MENTIONED.** Still resolved.
- **Guideline 4 (iPad sign-up toggle clipped) — NOT MENTIONED.** Still resolved.

---

## Root Cause Analysis

### Issue 1: ATT prompt exists but reviewer did not locate it

**This is not a bug — the ATT implementation is correct and working.** The reviewer did not tap the "Watch Ad for +1 Credit" button, which is where the ATT flow is triggered. The prompt has been present and functioning since Review #5.

The ATT flow works as follows:
1. User taps "Watch Ad for +1 Credit" on the home screen
2. On first ad attempt, the custom pre-prompt modal ("About Ads in Divvy") appears
3. User taps "Continue" → Apple's native ATT system dialog appears
4. User makes their choice → ad loads accordingly (personalized or generic)

**No tracking occurs before the ATT prompt.** The Google Mobile Ads SDK checks ATT authorization status automatically. When ATT is `undetermined` or `denied`, the SDK serves only non-personalized (contextual) ads. Tracking is only activated if the user taps "Allow" on the system ATT dialog.

---

## Fixes Applied

### No code changes required

The ATT implementation is correct and has been reviewed and accepted in Reviews #5, #6, and #7. Responded to Apple with video evidence showing the full ATT flow on iPad.

---

## Our Response

Hi,

Thank you for flagging this. The App Tracking Transparency permission request is implemented and functioning correctly — I believe it was not encountered during review because it appears at a specific point in the user flow.

The ATT prompt is triggered when the user taps the "Watch Ad for +1 Credit" button on the home screen. This is the only point where tracking becomes relevant, as it is the entry point to the ad system. On the first tap, the app presents a custom pre-prompt explaining ad tracking, followed by Apple's native ATT system dialog. No tracking data is collected at any other point in the app — the ad SDK loads only non-personalized, contextual ads until the user explicitly grants tracking permission through the ATT dialog.

This ATT implementation was originally addressed in Review #4, where the pre-prompt and system dialog were added. The pre-prompt language was then revised to use neutral wording in Review #5 (title: "About Ads in Divvy," single "Continue" button, disclaimer text). This updated implementation was reviewed and accepted without issue in Reviews #6 and #7 — no ATT concerns were raised in either of those submissions.

I've attached two screen recordings demonstrating the full ATT flow on physical devices and the general app flow on iPad.

Please let me know if there is anything else I can clarify or demonstrate.

Best regards,
Jaden Brescia

---

## Outcome
**APPROVED — May 27, 2026. App is live on the App Store.**
