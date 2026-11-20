Read this for push notifications, reminders, re-engagement copy, and the permission prompt.

# Notifications

A notification is an external trigger for the Itch lever: it should name the feeling the app answers, at the moment the feeling arises.

## Copy that speaks to the feeling

| Bad | Good | Why |
|---|---|---|
| We miss you! | Your 5-minute puzzle is ready. | Names the action, not our need |
| Don't lose your streak! | One quick round keeps your 12-day streak. | Smallest action, concrete stake |
| Time to practise! | Waiting for the bus? Try today's round. | Matches the user's moment |
| You have a new notification | Mia just beat your score. | One specific fact |

Rules:
- Write the moment, the action and the payoff in under 10 words after the title.
- Use one specific fact per push (a number, a name, a time).
- Put the verb first in the body.

## Timing tied to the feeling

- Anchor to the Itch decision in `docs/habit-plan.md` (for example, the commute for boredom, evening for "falling behind").
- Learn the user's actual open time: store the hour of each session start and schedule at the median.
- Schedule with `UNCalendarNotificationTrigger` for a daily time, or `UNTimeIntervalNotificationTrigger` for relative delays (such as 22 hours after last open).
- Send the reminder only if today's core action is not done: cancel with `removePendingNotificationRequests(withIdentifiers:)` when the action completes.

## Permission: prime, then provisional

1. **Defer the ask** until after the first completed core action, so the user has felt the value.
2. **Show an in-app priming screen first** with one line on what they will get ("A nudge at 8pm if today's round isn't done") and two buttons: "Turn on reminders" and "Not now". Only "Turn on" calls the system prompt, so a "Not now" never burns the one-shot prompt.
3. **Start with provisional authorization** to deliver quietly before the user decides:

```swift
let center = UNUserNotificationCenter.current()
try await center.requestAuthorization(options: [.alert, .sound, .badge, .provisional])
```

   Provisional notifications go to Notification Center with Keep/Turn Off buttons and no prompt. Upgrade to a full prompt once the user engages with them.

## Interruption levels

Set `content.interruptionLevel`:

| Level | Use for |
|---|---|
| `.passive` | Digests, social "FYI" |
| `.active` (default) | Reminders, daily nudge |
| `.timeSensitive` | A window that really closes (a live event ends in 10 minutes); needs the Time Sensitive Notifications capability |
| `.critical` | Never, without an Apple entitlement |

Use `.timeSensitive` rarely; overuse trains users to turn all notifications off.

## Frequency caps

- Cap at 1 per day and 3 per week for non-transactional pushes; start there and adjust from the opt-out metric ([metrics.md](metrics.md)).
- Keep a small `UserDefaults`/SwiftData record of sends and drop any push that would exceed the cap.
- Give per-category toggles in Settings (`UNNotificationCategory` plus an in-app switch for each) and a link to system settings via `UIApplication.openNotificationSettingsURLString`.
- After 3 unopened pushes in a row, halve the frequency.

## Fallback to widgets

When permission is denied (check `getNotificationSettings`), move the trigger to WidgetKit: a Lock Screen widget (`accessoryRectangular`) showing "Today: not done" and a Home Screen widget with a `Button(intent:)` for the core action. Prompt the user once, in Settings or on the empty state, to add the widget. Use `WidgetCenter.shared.reloadAllTimelines()` after each session so the state is current.

## Re-engagement for lapsed users

Send one specific reason, not "we miss you":

- Name something that changed or is waiting: "3 new puzzles since Tuesday", "Your leaderboard rank dropped to 5".
- Cap the sequence at 3 messages over 14 days (day 3, day 7, day 14), then stop.
- Make each message deep-link to the core action via `userInfo` and `UNNotificationResponse`, so one tap lands on the action, not the home screen.
- If the first message is unopened, change the reason instead of repeating it.

## Apple 4.5.4: marketing needs opt-in

Apple 4.5.4 says push "should not be used for promotions or direct marketing purposes unless customers have explicitly opted in" via consent language in the app UI, with an in-app way to opt out ([guidelines](https://developer.apple.com/app-store/review/guidelines/)). So:
- Keep two classes: **functional** (reminders and results for something the user set up) and **marketing** (offers, promotions, upsells).
- Put a separate "Offers and news" toggle, off by default, in the priming screen or Settings, and send marketing only when it is on.
- Never make push required for the app to function.

**Web/Android.** Web: Web Push needs a user gesture before `Notification.requestPermission()`. Android 13+: request `POST_NOTIFICATIONS` after the first win, and use one notification channel per category.
