Read this before shipping (SKILL.md step 5): a short checklist of what gets an app rejected or creates legal exposure.

# Compliance checklist

This is the step-5 checklist: mark each line pass, flag or n/a in the plan's Compliance section. Format: rule → what to do. Not legal advice.

## Apple App Store

- **4.5.4 marketing push** → send promotions only after explicit in-app opt-in, with an in-app opt-out and push never required ([guidelines](https://developer.apple.com/app-store/review/guidelines/)); implementation in [notifications.md](notifications.md).
- **3.1.1 loot boxes and random paid items** → show the odds of each item type before purchase.
- **1.3 Kids Category** → no links out, no purchases and no distractions outside a parental gate; no third-party analytics or ads.
- **5.1.4 apps for kids** → use no third-party analytics or advertising in apps aimed mainly at children, and follow the 1.3 terms for any limited exception.
- **5.1.1(iv)** → ask only for permissions the feature needs; never trick or force consent, and let the app work when a permission is denied.

## Google Play

- **Loot-box odds** → disclose odds in advance of purchase ([Fenwick summary](https://fenwick.com/insights/publications/google-play-now-requires-disclosure-of-loot-box-odds)).
- **Families policy** → use only self-certified ad SDKs, no personalized or remarketing ads, and no emotionally manipulative ads for child users ([Families policies](https://support.google.com/googleplay/android-developer/answer/9893335)).
- **Deceptive Behavior and Ads** → do not style notifications or ads like system alerts ([Deceptive Behavior](https://support.google.com/googleplay/android-developer/answer/17006354)).

## EU, US and UK law

- **EU DSA Art. 25 (Regulation 2022/2065)** → for online platforms, do not design interfaces that materially distort users' ability to make free, informed choices ([commentary](https://deceptive.design/articles/dark-patterns-and-the-eu-digital-services-act-mapping-autonomy-violations-and-design-factors); read the text on EUR-Lex).
- **FTC "Bringing Dark Patterns to Light" (Sept 2022)** → make subscription cancellation as easy as sign-up, and show prices and renewal terms plainly ([FTC](https://www.ftc.gov/reports/bringing-dark-patterns-light)).
- **EU Digital Fairness Act: a proposal as of Nov 2026, not law.** Press reports say it was published on 11 Nov 2026 and would add a ban on dark patterns and let users turn off streaks and autoplay; entry into force is likely 2027 or later, and the details are unconfirmed. Design streaks and autoplay with an off switch now, and check the Commission's text before relying on any provision ([EP Legislative Train](https://www.europarl.europa.eu/legislative-train/theme-protecting-our-democracy-upholding-our-values/file-digital-fairness-act), [report](https://www.ghacks.net/2026/11/11/eu-proposes-digital-fairness-act-to-ban-dark-patterns-and-let-users-turn-off-streaks-and-autoplay/)).

## Children

- **COPPA (US, under 13)** → get verifiable parental consent before collecting personal data, or collect none ([FTC COPPA](https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa)).
- **UK Children's Code (Age Appropriate Design Code)** → default to high privacy, switch off profiling and nudge techniques for children ([ICO](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/)).

**Kids or teens audience** → switch to the gentle forms:
- Streaks: no loss framing; use "days played" counts that never reset, and a freeze on by default.
- Random paid rewards: none; fixed, disclosed prices only.
- Notifications: no engagement or re-engagement pushes; send only user-set reminders, at most once a day, never at night.
- Social features: no open chat with strangers and no public leaderboards with real names.
