# Apple Review #1 — Rejected
**Date:** April 7, 2026
**Version:** 1.0 (Build 14)
**Review Device:** iPad Air 11-inch (M3), iPadOS 26.3
**Result:** REJECTED

---

## Rejection Reasons

### 1. Guideline 5.1.1(ii) — Legal: Privacy - Data Collection and Storage

**Issue:** One or more purpose strings in the app do not sufficiently explain the use of protected resources. Purpose strings must clearly and completely describe the app's use of data and, in most cases, provide an example of how the data will be used.

**Next Steps from Apple:** Update the camera and photo library purpose string to explain how the app will use the requested information and provide a specific example of how the data will be used.

### 2. Guideline 2.1(a) — Performance: App Completeness

**Issue:** The app exhibited one or more bugs that would negatively impact users.

**Bug description:** The app became completely unresponsive to taps after login.

**Review device details:**
- Device type: iPad Air 11-inch (M3)
- OS version: iPadOS 26.3
- Internet Connection: Active

---

## Our Response (April 7, 2026)

Hi,

Thank you for the detailed feedback. Both issues have been addressed in the new build.

**Guideline 5.1.1(ii) — Privacy Purpose Strings**

I've rewritten both the camera and photo library purpose strings to clearly explain what the permission is used for, provide a specific real-world example, describe how the data is processed, and clarify what is and isn't stored. For example, the photo library string now reads:

"Divvy uses your photo library to let you select a photo of a receipt for bill splitting. For example, after dining out you can choose a photo of your restaurant receipt from your library. The selected image is compressed and securely sent to our servers where AI extracts the item names, prices, discounts, tax, and total. Only the extracted text data is saved to your account so you can assign items to friends and split the bill. The original photo is processed in memory and is never stored on our servers."

The camera purpose string follows the same structure with its own specific example.

**Guideline 2.1(a) — App Unresponsive on iPad**

The unresponsive behavior on the iPad Air was caused by our UI being designed exclusively for iPhone and not handling the iPad layout properly. We have set supportsTablet to false in our build configuration, restricting the app to iPhone only. iPad is no longer a supported device for this release.

Thank you for your time reviewing my app. Please let me know if anything else is needed.

Best regards,
Jaden Brescia

---

## Fixes Applied
- Rewrote NSCameraUsageDescription and NSPhotoLibraryUsageDescription with detailed examples
- Set `supportsTablet: false` to disable iPad

## Outcome in Next Review (Review #2, April 10)
- **5.1.1(ii) Privacy Purpose Strings — PASSED.** Not mentioned in Review #2, confirming the rewritten purpose strings were accepted.
- **2.1(a) App Completeness — PARTIALLY RESOLVED.** iPad unresponsive bug was no longer flagged. However, a new 2.1(a) issue was raised: "no ads displayed" after tapping Watch Ad button. Apple still tested on iPad Air despite `supportsTablet: false`.
