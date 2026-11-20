---
name: habit-forming-apps
description: Design or audit the daily-habit loop of an app before and alongside its UI — triggers and notifications, a one-tap core action, defaults, variable rewards, no dead ends, and stored value that makes leaving costly. Use when starting a new app, adding a habit loop to an existing app, planning a first session, auditing retention or day-1 drop-off, writing push notifications, adding streaks, rewards or feeds, or defining a habit moment and D1/D7/D30 metrics. iOS-first; applies to web and Android.
license: MIT
metadata:
  author: ratul0
  version: "1.0.0"
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

The **habit moment** ties them together: the countable threshold of core actions after which users stay (TikTok's internal research, as alleged in Kentucky's 2024 lawsuit, put it at 260 videos). Every plan names one.

[`references/levers.md`](references/levers.md) holds each lever's mechanism, evidence, the decisions it forces, and iOS patterns. Load it for every Design or Audit run.

## Pick the mode

| The request | Mode | Run |
|---|---|---|
| New app, or a new feature that should become a habit | **Design** | Steps 1 → 5 |
| Existing app: "why don't people come back", retention review | **Audit** | Step 1, then [`references/audit.md`](references/audit.md), then steps 3 → 5 |
| Push notifications, reminders, re-engagement copy | **Notify** | [`references/notifications.md`](references/notifications.md) |
| Habit moment, retention metrics, analytics events | **Measure** | [`references/metrics.md`](references/metrics.md) |

## 1. Read the app

Read what already exists: the codebase, `README`, `CLAUDE.md`/`AGENTS.md`, `docs/`, any store listing, and `docs/habit-plan.md` if present (an earlier plan is the starting point, not a blank page).

Then pin down three facts in one line each:

- **The user** — who, and the moment in their day they reach for their phone.
- **The feeling** — the internal trigger the app answers (boredom, curiosity, guilt about falling behind, loneliness, stress, wanting to feel smart). One feeling, named in the user's words.
- **The core action** — the one thing a user does that delivers the value (solve a puzzle, log a meal, swipe a profile).

Infer from the code and docs first. Ask the user only for what stays ambiguous, as multiple choice with your recommendation first. Done when all three lines are concrete enough that a stranger could act on them.

## 2. Write the habit plan

Create or update `docs/habit-plan.md` in the project from [`references/plan-template.md`](references/plan-template.md). Work through the six levers in order using `references/levers.md`, then the habit moment and metrics using `references/metrics.md`.

Every lever gets a **decision**, not a principle: a named screen, control, default, reward, or notification, plus the metric that will show whether it works. "Make onboarding easy" is a principle; "app opens straight into today's puzzle, Start button pinned bottom, 72pt" is a decision.

Done when every lever row and the habit moment have a decision and a metric, and the plan's **Board brief** section lists the screen-level requirements step 3 needs.

## 3. Lay it out with /design

The plan becomes a design board through the `/design` skill. Pick the branch:

- **New app, or existing app with no design board** → run `/design` to create a new board that lays out the whole UI/UX from the plan: the first session, the core loop, every end/empty/error state, and the notification surfaces.
- **Existing app with a design board** → run `/design` on that board to add a new flow named `Habit loop v<N>`. Redesign existing screens only where the plan changes them, and keep them in the board's existing design system.

To find an existing board, check `docs/habit-plan.md`, `CLAUDE.md`, `docs/`, and the Design artifacts in the user's gallery. If more than one could be it, ask.

Pass `/design` the plan's Board brief verbatim. For screen-level craft (layout, polish, paywalls, reward screens), the `ios-app-design` skill applies when it is installed. Record the board link at the top of `docs/habit-plan.md`.

## 4. Review the board against the levers

Score the board with the rubric in [`references/audit.md`](references/audit.md). Every lever scoring below 2 goes back to `/design` with the specific fix. Then walk every screen on the board once more and confirm each one ends in a next action.

Done when every lever scores 2 or higher and the dead-end list is empty. Append the scores and fix list to the plan in the format `references/audit.md` gives.

## 5. Store and legal check

Run the short checklist in [`references/compliance.md`](references/compliance.md) against the plan. It covers only what gets an app rejected or creates legal exposure: marketing push without opt-in, random paid rewards without odds, Kids Category rules, and EU/FTC dark-pattern rules. Record each flag and its fix in the plan's Compliance section.

## Output

End every run with: the path to `docs/habit-plan.md`, the board link (Design and Audit modes), the lever scores, and the three changes most likely to move day-1 retention, highest impact first.
