Read this in step 2 of every Design or Audit run: the order of the first session and when the paywall appears.

# First session

## Budget

First core action within 3 taps and 60 seconds of first open. No account, no permission and no paywall before it.

## Order of asks

Each ask comes after the thing that earns it:

1. First core action.
2. First reward shown.
3. Provisional notifications, silently ([notifications.md](notifications.md)).
4. Account (Sign in with Apple), once there is something to save.
5. Full notification prompt, after the user engages.
6. Paywall.

## Paywall timing

- Show the first paywall only after the first core action and a visible reward.
- Keep the daily core action free at its minimum viable size. Sell more of it, depth, or the piggy bank (history, stats, cosmetics). Gate at a moment of need.
- Trial length covers the habit-moment window: habit moment 3 actions in 3 days → a 7-day trial, so the trial ends after the habit forms, not before.
- Send a trial-ending reminder.
- Compare conversion for users who reached the habit moment vs. not (events `paywall_shown`, `trial_started` in [metrics.md](metrics.md)).
- For lapsed subscribers, use StoreKit 2 win-back offers (iOS 18+).
- Paywall screen design belongs to `ios-app-design`.

## Endowed progress

Seed the first session with visible progress ([levers.md](levers.md#pairs-with)).

Done when the plan's First session section has a value for every field.
