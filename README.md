# Divvy - The Easy Bill Splitter

<p align="center">
  <img src="assets/logov2.png" alt="Divvy Logo" width="400">
</p>

Snap a photo of any receipt. AI reads every item, price, and total. Assign items to friends, generate a shareable web link, and get paid back through Venmo, PayPal, or Cash App. No app needed on their end.

### App Screenshots

<p align="center">
  <img src="assets/tutorial_1.png" alt="Scan" width="150">
  &nbsp;&nbsp;
  <img src="assets/tutorial_2.png" alt="Split" width="150">
  &nbsp;&nbsp;
  <img src="assets/tutorial_3.png" alt="Share" width="150">
  &nbsp;&nbsp;
  <img src="assets/tutorial_4.png" alt="Assign" width="150">
  &nbsp;&nbsp;
  <img src="assets/tutorial_5.png" alt="Pay" width="150">
</p>

### Accepted Payment Methods

<p align="center">
  <img src="assets/Venmo_Logo.png" alt="Venmo" height="40">
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="assets/PayPal.png" alt="PayPal" height="40">
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="assets/Cash-App-Logo.png" alt="Cash App" height="40">
</p>

---

## How It Works

1. **Scan** — Take a photo of any receipt and AI extracts every item, price, tax, and total instantly
2. **Split** — Add friends and tap items to assign who ordered what. Tax and tip are split proportionally
3. **Share** — Generate a web link your friends can open in any browser — no app install needed
4. **Pay** — One-tap payment links via Venmo, PayPal, or Cash App with itemized breakdowns

---

## Features

- **AI Receipt Scanning** — Handles handwritten receipts, itemized bills, tax, discounts, and tips
- **Smart Splitting** — Assign items to multiple people. Tax and tip split proportionally based on what each person ordered
- **Shareable Web Links** — Recipients open a link in their browser to see their share and pay. No app download required
- **Payment Deep Links** — Venmo, PayPal, and Cash App links pre-filled with the exact amount and itemized notes
- **Receipt History** — Track past splits, see who's paid, and revisit any receipt
- **Free to Use** — Watch a short ad to earn a scan credit. No subscriptions, no paywalls. First scan is free
- **Upload Idempotency** — SHA-256 dedup prevents duplicate AI calls on network retries
- **Undo Delete** — Accidentally remove a friend from the split? Stack-based undo with a 12-second countdown
- **Crashlytics** — Firebase Crashlytics integrated for production crash monitoring

---

## App Flow

```
Start Screen --> Tutorial --> Login --> Home
                                         |
                             +-----------+-----------+
                             |           |           |
                        Watch Ad    Upload/Camera   History
                             |           |           |
                       Ad Cascade    AI Scanning    Past Receipts
                       (4 tiers)   (uses 1 credit)  (paid tracking)
                             |           |
                        Earn Credit   Split Items
                                     - Add friends
                                     - Assign items
                                     - Create share link
                                          |
                                      Breakdown
                                      - Payment summary
                                      - Select who paid
                                      - Send payment links
```

---

## Apple Review Journey

Divvy has been through **6 Apple review rejections** (and counting). Each rejection surfaced a new issue — some obvious in hindsight, others surprising. Every one made the app better. The review process is ongoing.

| # | Date | Guideline | Rejection Reason | Fix |
|---|------|-----------|------------------|-----|
| 1 | Apr 7 | 5.1.1(ii), 2.1(a) | Privacy purpose strings too vague; app froze on iPad after login | Rewrote camera/photo permission strings with specific examples; set `supportsTablet: false` |
| 2 | Apr 10 | 2.1(a) | "No ads displayed" after tapping Watch Ad | Ad networks don't serve in Apple's sandbox — built entire 4-tier ad cascade with free pass fallback |
| 3 | Apr 21 | 2.1(a) | "Screen loads indefinitely" (ad cascade took ~96 seconds) | Cut cascade from 96s to 34s, added visible countdown timer, created demo account with 100 credits |
| 4 | Apr 23 | 2.1(a), 2.1 | Still flagging "no ads"; ATT prompt not found by reviewer | Explained expected sandbox behavior; moved ATT to first Watch Ad tap; provided screen recording from physical device |
| 5 | Apr 23 | 5.1.1(iv) | ATT pre-prompt wording "encouraged tracking" ("Allow Personalized Ads" button) | Complete ATT rewrite — neutral "About Ads in Divvy" title, single "Continue" button, no persuasive language |
| 6 | Apr 25 | 4 (Design) | Google Sign-In opened Safari; sign-up text clipped on iPad | Replaced `@react-native-google-signin` with `expo-auth-session` (in-app `ASWebAuthenticationSession`); built responsive scaling hook |

### Lessons Learned

**Apple tests on iPad even if you declare iPhone-only.** Setting `supportsTablet: false` removes your app from iPad search results, but Apple reviewers still test iPhone apps in iPad compatibility mode. Layout bugs there will get you rejected.

**Ad networks don't serve ads during App Review.** This caused three consecutive rejections (Reviews 2-4). Google AdMob and Unity Ads return nothing in Apple's sandbox. The solution was a 4-tier ad cascade with a backend free pass system, plus a demo account with pre-loaded credits so reviewers could test without ads.

**ATT pre-prompt wording is heavily scrutinized.** Buttons labeled "Allow Personalized Ads" were flagged as directing users toward accepting tracking. Even a "No Thanks" option that skipped ATT was a violation. Apple requires strictly neutral language — a single "Continue" button that always leads to the system dialog.

**Provide video evidence.** Starting with Review 4, screen recordings from a physical device proved that features (ATT, Google Auth, permissions) were working correctly. Apple can only test what they can see in their environment.

**Every rejection made the app better.** The ad system went from a single ad format with no fallback to a production-grade cascade with SSV verification, rate limiting, mediation, and a free pass safety net. The login flow went from opening Safari to a seamless in-app sheet. None of these improvements would have happened without the review process.

See [docs/apple-reviews/](docs/apple-reviews/) for the full review history with detailed responses.

---

## Revenue Model

Users watch a rewarded ad to earn 1 scan credit. Each receipt scan consumes 1 credit. New users get 1 free credit on signup. A 4-tier ad cascade (rewarded video, rewarded interstitial, interstitial, free pass) ensures users are never stuck — if no ads are available, a daily free pass grants a credit automatically.

---

## Status

**Version:** 1.5 | **Platform:** iOS (iPhone) | **Status:** In active App Store review (7th submission pending)

> Divvy is fully built and functional but **not yet available on the App Store**. The app has been through 6 Apple review cycles, each rejected for a different reason. Every rejection has been addressed and the app has improved substantially as a result. The review process is ongoing.

---

## Links

- **Website:** [divvy-app-491221.web.app](https://divvy-app-491221.web.app)
- **Privacy Policy:** [divvy-app-491221.web.app/privacy-policy.html](https://divvy-app-491221.web.app/privacy-policy.html)

---

## Repository Structure

```
Divvy/
├── README.md                  # This file
├── CHANGES.md                 # High-level changelog
├── LICENSE                    # All rights reserved
├── docs/
│   └── apple-reviews/         # Apple review history with responses
├── website/
│   ├── index.html             # Landing page
│   ├── privacy-policy.html    # Privacy policy
│   └── logov2.png             # Logo
└── assets/
    ├── applogo.png            # App icon
    ├── logov2.png             # In-app logo
    ├── tutorial_1-5.png       # App tutorial screenshots
    ├── Venmo_Logo.png         # Venmo logo
    ├── PayPal.png             # PayPal logo
    └── Cash-App-Logo.png      # Cash App logo
```

> **Note:** Source code is maintained in a private repository. This public repo contains documentation, assets, and the Apple review journey for portfolio purposes. Code is available for live demo upon request.

---

## Contact

**Jaden Brescia** — jadenbresciawebsite@gmail.com

---

Copyright (c) 2026 Jaden Brescia. All rights reserved.
