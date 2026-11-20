---
name: habit-forming-apps
description: "Design or audit an app's habit loop, the retention engine behind why users open it, act in one tap and come back tomorrow: triggers and push notifications, defaults, variable rewards, no dead ends, stored value, and the habit moment. Use when starting a new app or habit feature, when users churn or don't return after day 1, when asked to make an app sticky, addictive, more engaging or gamified, when writing push or re-engagement notifications, when designing streak, reward, feed or social-challenge mechanics, when timing a paywall or trial against the habit moment, or when defining D1/D7/D30 retention metrics and analytics events. iOS-first; applies to web and Android."
license: MIT
metadata:
  author: ratul0
  version: "1.2.0"
---

# Habit-forming apps

A beautiful app that nobody opens on day two is wasted work. Design the **habit** first — why the user opens the app, what they do, why they come back tomorrow — then let the screens follow from it.

## The six levers

Six levers, in the order a user meets them. The first three get the user in; the last three keep them coming back. Use these names as working vocabulary in the plan, on the board, and in reviews.

| # | Lever | The question it answers |
|---|---|---|
| 1 | **Itch** | What feeling makes the user open the app with no notification at all? |
| 2 | **Lazy thumb** | Can the core action be done in one tap, by someone with low motivation, one-handed? |
| 3 | **Paved path** | What happens if the user decides nothing? |
| 4 | **Slot machine** | Is the reward still a surprise on day 30? |
| 5 | **Bottomless pit** | Where does every screen — including "done", "empty" and "error" — lead next? |
| 6 | **Piggy bank** | What would the user lose by deleting the app today? |

The **habit moment** ties them together: the countable threshold of core actions after which users stay. Every plan names one.

[`references/levers.md`](references/levers.md) holds each lever's mechanism, evidence, the decisions it forces, and iOS patterns. Load it for every Design or Audit run.

## Pick the mode

| The request | Mode | Run |
|---|---|---|
| New app, or a new feature that should become a habit | **Design** | Steps 1 → 5 |
| Existing app: "why don't people come back", a retention review, or making an existing app stickier | **Audit** | Step 1, then [`references/audit.md`](references/audit.md), then steps 2 → 5 (step 2 turns the audit's fix list into lever decisions and the Board brief) |
| Push notifications, reminders, re-engagement copy | **Notify** | Step 1, then [`references/notifications.md`](references/notifications.md); writes the plan's Notifications section |
| Habit moment, retention metrics, analytics events | **Measure** | Step 1, then [`references/metrics.md`](references/metrics.md); writes the habit moment and the Metrics and events section |

Also load:

| When | Load |
|---|---|
| Step 2 of every Design or Audit run | [`references/first-session.md`](references/first-session.md) |
| The app has friends, multiplayer, challenges, leaderboards or sharing | [`references/social.md`](references/social.md) |
| The target is web or Android (`web-android.md` replaces the iOS patterns in `levers.md`) | [`references/web-android.md`](references/web-android.md) |

## 1. Read the app

Read what already exists: the codebase, `README`, `CLAUDE.md`/`AGENTS.md`, `docs/`, any store listing, and `docs/habit-plan.md` if present (an earlier plan is the starting point, not a blank page).

Then pin down three facts in one line each:

- **The user** — who, and the moment in their day they reach for their phone.
- **The feeling** — the internal trigger the app answers (boredom, curiosity, guilt about falling behind, loneliness, stress, wanting to feel smart). One feeling, named in the user's words.
- **The core action** — the one thing a user does that delivers the value (solve a puzzle, log a meal, swipe a profile).

Infer from the code and docs first. Ask the user only for what stays ambiguous, as multiple choice with your recommendation first. Done when all three lines are concrete enough that a stranger could act on them.

## 2. Write the habit plan

Create or update `docs/habit-plan.md` in the project from [`references/plan-template.md`](references/plan-template.md). Work through the six levers in order using `references/levers.md`, then first-session order and paywall timing using `references/first-session.md`, then the habit moment and metrics using `references/metrics.md`.

Every lever gets a **decision**, not a principle: a named screen, control, default, reward, or notification, plus the metric that will show whether it works. "Make onboarding easy" is a principle; "app opens straight into today's puzzle, Start button pinned bottom, 56pt" is a decision.

Done when every lever row and the habit moment have a decision and a metric, the plan's **First session** section is filled, and the plan's **Board brief** section lists the screen-level requirements step 3 needs.

## 3. Lay it out with /design

The plan becomes a design board through Claude's `/design` skill.

**Find the board**, in this order:

1. The board link at the top of `docs/habit-plan.md`.
2. Board links in `CLAUDE.md`, `AGENTS.md` or `docs/`.
3. In Claude, the user's Design artifacts (Artifact tool, action `list`, type `Design`), matched by app name.

Exactly one candidate → use it. Several → ask the user, multiple choice, recommended first. None → the new-board branch.

**How to run `/design`:** a skill named `design` in the skills list → invoke it. Otherwise, in Claude, use the **Design artifact type**: create the board by publishing with that type's `type_url` (Artifact tool, `list` with scope `types` to find it), or extend an existing board by publishing to its `url` — in both cases following the type's own instructions. Neither a `design` skill nor an Artifact tool → the fallback below.

**Pick the branch:**

- **New board** → have `/design` lay out the whole UI/UX from the plan: the first session, the core loop, every end/empty/error state, and the notification surfaces.
- **Existing board** → read the board and list its `Habit loop v<k>` flows; add a flow named `Habit loop v<N>`, where N is one more than the highest existing k, or 1 if none. Redesign existing screens only where the plan changes them, and keep them in the board's existing design system.

Pass `/design` the plan's Board brief verbatim. Boundary: screen layout, visual polish, paywall screens and reward-screen craft belong to the `ios-app-design` skill when installed; this skill decides what the loop is and when each piece appears.

**Fallback** (no `/design` route above): write a `## Screens` section in the plan instead, one entry per screen: its job, its primary action, and the next action from its end, empty, error and completion states. Step 4 then scores that list instead of a board.

Done when the board link (or "see Screens") is recorded at the top of `docs/habit-plan.md` and every Board brief line names the board frame (or Screens entry) that satisfies it.

## 4. Review the board against the levers

Score the board (or the Screens list) with the rubric in [`references/audit.md`](references/audit.md). Every lever scoring below 2 goes back to `/design` (or into the Screens section) with the specific fix, for at most 2 fix rounds. Then walk every screen once more and confirm each one ends in a next action.

Done when every lever scores 2 or higher and the dead-end list is empty, or after round 2, when a lever still below 2 is recorded in the plan's fix list as open, with the reason. Append the scores and fix list to the plan in the format `references/audit.md` gives.

## 5. Store and legal check

Run the checklist in [`references/compliance.md`](references/compliance.md) against the plan. It covers only what gets an app rejected or creates legal exposure: marketing push without opt-in, random paid rewards without odds, Kids Category rules, and EU/FTC dark-pattern rules.

Done when every line of the checklist is marked pass, flag or n/a in the plan's Compliance section, and every flag has a fix.

## Output

End every run with the items for its mode:

- **Design / Audit:** the path to `docs/habit-plan.md`, the board link or Screens section, the lever scores, and the three changes most likely to move day-1 retention, highest impact first.
- **Notify:** the plan path and the Notifications table.
- **Measure:** the plan path, the habit moment statement and the event list.
