Read this to find the habit moment, define retention metrics, or specify analytics events.
If `docs/habit-plan.md` does not exist, create it from [plan-template.md](plan-template.md) before writing the Metrics and events section.

# Metrics

Tool-agnostic: the events and queries below work in PostHog, Firebase/GA4, TelemetryDeck or a SQL warehouse.

## Find the habit moment

The habit moment is the number of core actions within the first N days after which users mostly stay.

**Before you have data (starting guess).** Guess 3 core actions within 3 days, then replace it. For daily-use apps use 3 in 3 days; for weekly-use apps, 2 in 7 days. Write it into the plan as "provisional".

**Once you have data (about 500+ new users, or 4 weeks):**
1. Pick N (try 3, 7 and 14 days).
2. For each user, count core actions in their first N days.
3. Label each user retained (active on day 30, or in days 28-32) or churned.
4. For thresholds k = 1, 2, 3 and so on, compute: retention of users with at least k actions, and retention of users with fewer.
5. Choose the k with the largest gap, and where the gap stops growing (the "knee").
6. Report it as "users with at least k actions in N days retain at X% vs Y%". This is a correlation, not proof of cause; confirm by testing a change that raises the share reaching k.

TikTok's 260 videos, an internal figure alleged in Kentucky's 2024 complaint ([NPR](https://www.npr.org/2024/10/11/g-s1-27676/tiktok-redacted-documents-in-teen-safety-lawsuit-revealed)), is not a benchmark for your app.

## Retention measures

| Metric | Definition | Use |
|---|---|---|
| D1 / D7 / D30 | Share of an install cohort active on day 1, 7 or 30 after install | Onboarding and Itch; compare cohorts by release |
| Bracket retention | Active in day 7-13, 30-37 | Less noisy for weekly-use apps |
| L7 / L28 | Distribution of days active in the last 7 or 28 | Habit depth; track share with L7 ≥ 4 |
| DAU/MAU | Daily active divided by monthly active | Stickiness; above 0.2 is solid, 0.5 is exceptional (rules of thumb, not sourced) |
| Resurrection rate | Lapsed users who return in a week | Re-engagement |

Track cohorts by install week and compare against the previous release.

## One event per lever

| Lever | Event | Properties |
|---|---|---|
| Itch | `session_start` | `source` (notification, widget, live_activity, shortcut, organic), `hour`, `notif_summary` (Scheduled Summary on or off, from `scheduledDeliverySetting`) |
| Lazy thumb | `core_action_done` | `taps_from_open`, `seconds_from_open`, `entry_point` |
| Paved path | `default_kept` / `default_changed` | `setting`, `value` |
| Slot machine | `reward_shown` | `reward_type`, `size`, `session_index` |
| Bottomless pit | `next_action_taken` | `from_state` (end, empty, error, complete), `action` |
| Piggy bank | `investment_made` | `kind`, `total_count` |

Add `notification_sent`, `notification_opened` and `permission_result` (granted, provisional, denied) for [notifications.md](notifications.md).

### Monetization events

| Event | Properties |
|---|---|
| `paywall_shown` | `placement`, `core_actions_before`, `habit_moment_reached` |
| `trial_started` | `plan`, `placement` |

### Social events (when social.md applies)

| Event | Properties |
|---|---|
| `challenge_sent` | `channel`, `opponent_type` (friend, stranger, bot) |
| `challenge_accepted` | `seconds_to_accept`, `from_notification` |
| `invite_sent` | `channel`, `after_first_win` |
| `invite_converted` | `channel`, `days_to_install` |
| `turn_taken` | `match_type` (sync, async), `seconds_since_prompt` |

## Guardrail metrics

Pair every growth metric with a guardrail so that a win in one doesn't hide a loss in another.

| Guardrail | How to measure | Act when |
|---|---|---|
| Notification opt-out | Share who disable notifications in 7 days (`await UNUserNotificationCenter.current().notificationSettings()` at launch, log changes) | It rises after a push change: cut frequency |
| Uninstall | Deletions ÷ Active Devices in App Store Connect App Analytics (counts only users who share analytics), or APNs `410 Unregistered` responses on your push sends | It rises after a release |
| Satisfaction | In-app one-tap thumbs or 1-5 prompt after a completed session, at most once a week | Score falls, even as sessions rise |
| Crash-free sessions | Crash tool | It falls below 99.5% |

Ask for an App Store review with `@Environment(\.requestReview)` only after a success moment; the system shows it at most 3 times a year.
