# Apple Review #5 — Rejected
**Date:** April 23, 2026
**Version:** 1.4 (Build 18)
**Review Devices:** iPhone 17 Pro Max, iPad Air 11-inch (M3)
**Result:** REJECTED

---

## Rejection Reason

### Guideline 5.1.1(iv) — Legal: Privacy — Data Collection and Storage

**Issue:** The app includes App Tracking Transparency permission requests, but it encourages or directs users to accept tracking.

**Specific complaints:**
1. A message appears before the permission request, and to proceed users press an "Allow Personalized Ads" button. Apple says: use words like "Continue" or "Next" instead.
2. A message appears before the permission request, and the user can close the message and delay the permission request with the "No Thanks, Continue" button. Apple says: the user should always proceed to the permission request after the message.

**Apple's required next steps:**
- Revise the permission request process to not display messages with inappropriate words on buttons
- Include an exit button on the message before the tracking request
- If necessary, provide information about why tracking is requested before the prompt appears

---

## Issues Resolved From Review #4
- **2.1(a) "No ads displayed" — NOT MENTIONED.** Apple did not raise this issue again. The ad cascade explanation and demo account appear to have satisfied this concern.
- **2.1 "ATT not appearing" — NOT MENTIONED.** The screen recording and explanation of when ATT fires resolved this. However, the ATT *content* triggered a new guideline violation (5.1.1(iv)).

---

## Root Cause Analysis

The ATT pre-prompt modal had two Apple-guideline violations:

1. **"Allow Personalized Ads" button** — This wording directed users toward accepting tracking. Apple's guideline requires neutral language like "Continue" or "Next" on buttons that precede the system ATT dialog.

2. **"No Thanks, Continue" button** — This allowed users to skip the system ATT dialog entirely and proceed directly to ads (with non-personalized ads). Apple requires that once a pre-prompt is shown, the user must proceed to the system ATT dialog. The only alternative should be to exit the flow entirely.

Additionally, the pre-prompt contained:
- Title "Help Keep Divvy Free" — could be seen as guilt-tripping
- Hint text saying "just tap 'Allow' there to finish" — directly instructed users to accept tracking
- Comparison to Instagram/YouTube/Spotify — social pressure to accept

---

## Fixes Applied (for next submission)

### ATT pre-prompt completely rewritten

**Before:**
- Title: "Help Keep Divvy Free"
- Body: guilt-driven copy, Instagram/YouTube comparison, hint to "tap Allow"
- Two buttons: "Allow Personalized Ads" (proceeds to ATT) / "No Thanks, Continue" (skips ATT, loads ads anyway)
- No exit button

**After:**
- Title: "About Ads in Divvy" — neutral, informational
- Body: factual explanation that Divvy uses ads, user can choose personalized or generic on the next screen, app works the same either way
- Single "Continue" button — always proceeds to system ATT dialog
- Disclaimer below button: "Tapping Continue does not enable tracking. You will be taken to a system prompt where you make your final decision."
- X button (top-right corner) — dismisses the modal, returns to home screen, no ads load

### Flow logic updated
- `handleATTContinue`: dismiss → requestATTPermission → markATTPrompted → load ads
- `onDismiss`: dismiss modal only (no markATTPrompted, no ads)

### Compliance checklist (Guideline 5.1.1(iv))
- [x] No button text that encourages or directs users to accept tracking
- [x] "Continue" button always leads to system ATT dialog
- [x] Exit button (X) included to dismiss without proceeding
- [x] X button does NOT skip ATT and continue to ads — it exits the entire flow
- [x] Pre-prompt text is informational only, no guilt/pressure language
- [x] No instructions telling users what to select on the system ATT dialog
- [x] Disclaimer clarifies that "Continue" does not enable tracking

---

## Our Response

Hi,

Thank you for the feedback. The ATT pre-prompt has been completely revised to comply with Guideline 5.1.1(iv).

**Changes made:**

1. **Removed "Allow Personalized Ads" button.** Replaced with a single "Continue" button that always proceeds to the system ATT dialog, using neutral language as requested.

2. **Removed "No Thanks, Continue" button.** Users can no longer skip the system ATT dialog. The "Continue" button always leads to Apple's system tracking prompt where the user makes their final decision.

3. **Revised all pre-prompt text.** The title is now "About Ads in Divvy" (informational, not persuasive). The body text neutrally explains that the app uses ads and the user can choose personalized or generic on the next screen. A disclaimer below the Continue button states: "Tapping Continue does not enable tracking. You will be taken to a system prompt where you make your final decision."

**Regarding the exit button:** Apple's Human Interface Guidelines for Privacy state: "Don't include additional actions in your custom screen or window. For example, don't provide a way for people to leave the screen or window without viewing the system alert." To comply with this, the pre-prompt contains only the "Continue" button, which always proceeds to the system ATT dialog. There is no way to dismiss the pre-prompt without viewing the system alert.

**If the user terminates the app** before responding to the ATT dialog, no tracking preference is recorded. The next time they tap "Watch Ad," the full flow will appear again. The user cannot watch any ads until they have responded to the system ATT dialog.

Please note this app is iPhone-only (supportsTablet is set to false).

Thank you for your patience. Please let me know if anything else is needed.

Best regards,
Jaden Brescia
