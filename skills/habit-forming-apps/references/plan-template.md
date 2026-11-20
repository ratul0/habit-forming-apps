Read this in step 2 to create or update docs/habit-plan.md.

# Habit plan template

Copy into the project as `docs/habit-plan.md`. Replace every `<…>`. Keep it to decisions; reasoning goes in one short clause after a decision, if at all.

```markdown
# Habit plan — <App name>

Design board: <link, or "not yet created">
Last updated: <YYYY-MM-DD> · Mode: <Design | Audit | Notify | Measure>

## Foundations

- **The user:** <who, and the moment in their day they reach for their phone>
- **The feeling:** <the one internal trigger, in the user's words>
- **The core action:** <the one action that delivers the value>
- **Habit moment:** <N core actions within D days> — <how this number was chosen or will be validated>

## First session

- **Taps and seconds from first open to first core action:** <N taps, N s> (target ≤3 taps, ≤60 s)
- **Order of asks:** <e.g. core action → first reward → provisional notifications → account → full notification prompt → paywall>
- **Paywall:** <placement, and trial length relative to the habit moment>

## Levers

| Lever | Decision | Screen / surface | Metric |
|---|---|---|---|
| Itch | <when the feeling arises → which external trigger fires then: notification, widget, Live Activity> | <surfaces> | <e.g. % of sessions not started from a notification by week 2> |
| Lazy thumb | <core action in N taps; where the button sits; what counts as "done"> | <screen> | <time from open to core action> |
| Paved path | <what the app opens to; every decision point and its default> | <screens> | <% who keep each default> |
| Slot machine | <the reward; how its timing or size varies> | <screen> | <sessions per active day> |
| Bottomless pit | <what follows "done", empty, error, end of list> | <screens> | <actions per session> |
| Piggy bank | <what the user builds up and would lose> | <surface> | <D30 retention of users who invested vs. not> |

## Social loops

Optional: if the app has friends or multiplayer. <trigger, reward, stored value; see references/social.md>

## Board brief

Screen-level requirements for /design, one per line, each naming the board frame or Screens entry that satisfies it:

- <e.g. App opens directly into today's session; no menu or home screen in between.> → <board frame or Screens entry>
- <e.g. Primary action is the largest element, pinned in the bottom thumb zone.> → <board frame or Screens entry>
- <requirement> → <board frame or Screens entry>

## Screens

Optional: used only when /design is unavailable. One entry per screen:

- **<Screen>:** job <…>; primary action <…>; next action from its end / empty / error / completion states <…>

## Notifications

| Trigger moment | Copy (speaks to the feeling) | Type | Cap |
|---|---|---|---|
| <when> | <text> | <transactional / reminder / marketing (opt-in)> | <max per day/week> |

## Metrics and events

<Events to track, the retention cohorts, and guardrail metrics, from references/metrics.md.>

## Audit scores

| Date | Itch | Lazy thumb | Paved path | Slot machine | Bottomless pit | Piggy bank | Notes |
|---|---|---|---|---|---|---|---|

## Compliance

- <flag> → <fix>
```
