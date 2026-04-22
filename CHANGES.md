# Changelog

## v1.0.0 — Resubmission (April 2026)

### Ad System
- Added 4-tier ad cascade with automatic fallback (rewarded video, rewarded interstitial, interstitial, free pass)
- Added free pass system — 1 free scan credit per day when no ads are available
- Added server-side ad verification (SSV) for rewarded ads
- Added AdMob mediation with Unity Ads for broader ad fill
- Added progressive status messages during ad loading
- Added rate limiting on unverified credit grants

### Onboarding & UX
- Added animated splash screen with logo bounce and progress bar
- Added sign-in loading modal ("Setting Up Your Account")
- Added App Tracking Transparency (ATT) with custom pre-prompt
- Added contacts permission pre-prompt explaining local-only access
- Added tutorial tips about photo cropping and payment setup
- Fixed tutorial navigation bug that could trap users after sign-in

### Web Share Link
- Fixed who-paid confirm button that was stuck disabled
- Added detailed Venmo payment notes with item breakdown

### Privacy
- Added IDFA / Advertising Data disclosure for ATT + ad mediation
- Added contacts access disclosure (local-only, never uploaded)
- Updated privacy policy with tracking and ad attribution details
- Added 40 SKAdNetwork IDs for ad attribution

### Platform
- Disabled iPad support — iPhone only for v1

## v1.0.0 — Initial Submission (April 2026)

- AI-powered receipt scanning
- Multi-provider authentication (Google, Apple, Email)
- Real-time item assignment with proportional tax/tip splitting
- Shareable web links — recipients don't need the app
- Payment deep links for Venmo, PayPal, and Cash App
- Receipt history with per-person paid tracking
- 5-card tutorial carousel
- Floating help button with page-specific tips
