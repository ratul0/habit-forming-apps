Read this for every Design or Audit run: each lever's mechanism, evidence, decisions to record, and iOS patterns. Web or Android target: see [web-android.md](web-android.md).

# The six levers

Contents: [Itch](#1-itch) · [Lazy thumb](#2-lazy-thumb) · [Paved path](#3-paved-path) · [Slot machine](#4-slot-machine) · [Bottomless pit](#5-bottomless-pit) · [Piggy bank](#6-piggy-bank) · [Pairs with](#pairs-with)

## 1. Itch

**Mechanism.** Users return when an internal trigger (a feeling: boredom, curiosity, falling behind) points at the app. External triggers (notification, widget) exist to wire that feeling to the app until it fires unprompted. Source: Nir Eyal, *Hooked* (2014), [nirandfar.com/hooked](https://www.nirandfar.com/hooked).

**Evidence.** Hooked's loop is Trigger, Action, Variable Reward, Investment. This is a practitioner framework, not an experimental result.

**Decide.**
- The one feeling, in the user's words, and the time of day it arises.
- Which external trigger fires at that moment (notification, widget, Live Activity, Shortcut), and what copy names the feeling.
- The target share of sessions that start with no notification by week 2.

**iOS patterns.**
- WidgetKit Home Screen and Lock Screen widgets show today's state (`accessoryCircular`, `accessoryRectangular`) so the glance itself is the trigger.
- ActivityKit Live Activities for a time-boxed event (a daily round, a timer).
- App Intents plus `AppShortcutsProvider` expose the core action to Siri, Spotlight and the Action button.
- Schedule notifications with `UNCalendarNotificationTrigger` at the moment the feeling arises. Copy rules are in [notifications.md](notifications.md).

## 2. Lazy thumb

**Mechanism.** Behavior happens when motivation, ability and a prompt coincide (B=MAP). Users on a low-motivation day still act if ability is high, so make the core action one tap and lower the bar for "done". Source: BJ Fogg, [behaviormodel.org](https://behaviormodel.org/).

**Evidence.** Duolingo let one lesson, instead of the full daily goal, extend a streak. It reported a 3.3% relative lift in day-14 retention, +10.5% of daily learners on a streak within 20 days, and over 40% more learners on a 7+ day streak (a result reported a year later) ([Duolingo blog](https://blog.duolingo.com/improving-the-streak)). "Relative" means 3.3% of the baseline rate, not 3.3 points. Photo logging versus typed food search applies the same idea: one action replaces a multi-step form.

**Decide.**
- Taps from app open to the core action (target 1).
- Where the primary button sits, its size, and what counts as "done" for today's streak.
- The minimum viable action for a low-motivation day.

**iOS patterns.**
- Pin the primary button with `.safeAreaInset(edge: .bottom)` and style it `.buttonStyle(.borderedProminent)` with `.controlSize(.large)`, at least 44pt tall (aim for 56pt+).
- Open straight into the core action (no home menu); restore state with `@SceneStorage`.
- Confirm with `.sensoryFeedback(.success, trigger:)`.
- Offer the same action from a widget button (`Button(intent:)`) so it takes no app launch.

## 3. Paved path

**Mechanism.** Most users keep whatever is preselected, so each default is a decision you make for them. Choose the one that gets them to the habit.

**Evidence.** Johnson & Goldstein, *Science* 302:1338 (2003), [paper](https://www.science.org/doi/10.1126/science.1091721): effective organ-donation consent was about 42% under opt-in versus 82% under opt-out in their online experiment ([Goldstein 2004 summary](https://www.dangoldstein.com/papers/JohnsonGoldstein_Defaults_Transplantation2004.pdf)). National consent rates show the same gap, roughly 60 points between opt-in and opt-out countries; note that "effective consent" is not actual donation.

**Decide.**
- What the app opens to with no user input.
- Every decision point (reminder time, goal size, notification category, difficulty) with its default and why.
- Defaults pre-filled from what you already know (locale, calendar, Health data).

**iOS patterns.**
- Pre-select a sensible reminder time with `DatePicker` and `@AppStorage`.
- `@AppStorage`-backed settings that start in the "on" state for the habit-forming option.
- Skip account creation until after the first completed action (Sign in with Apple later).
- Ask for notification permission after the first win, with `.provisional` authorization (see [notifications.md](notifications.md)).

## 4. Slot machine

**Mechanism.** Rewards that vary in size or timing hold attention better than fixed ones. This traces to Skinner's variable-ratio schedules (Ferster & Skinner, *Schedules of Reinforcement*, 1957).

**Evidence.** Variable-ratio schedules produce the highest, most extinction-resistant response rates in Skinner's work: animals rewarded unpredictably keep responding long after rewards stop. Swipe feeds apply this per swipe.

**Decide.**
- What varies (reward size, content, surprise bonus) and what stays fixed.
- The odds or distribution, written down.
- What still surprises a user on day 30.

**iOS patterns.**
- Use `.sensoryFeedback(.impact, trigger: rewardID)` and `.phaseAnimator` for reveal moments, with `Canvas`/`TimelineView` confetti.
- Draw rewards with a seeded generator so each draw is reproducible and loggable: a five-line SplitMix64 `RandomNumberGenerator`, seeded per draw with the seed stored in the event (`SystemRandomNumberGenerator` cannot be seeded). Tune the distribution there.
- Rotate content with a `WidgetKit` timeline so the widget differs by day.
- If any variable reward is purchasable, check [compliance.md](compliance.md) first.

## 5. Bottomless pit

**Mechanism.** Behavior continues until something signals "stop". Removing stopping cues, such as endless feeds and autoplay, extends sessions. Every screen should lead somewhere next.

**Evidence.** Aza Raskin, widely credited with inventing infinite scroll (the credit is disputed; see NPR's editor's note), has said it wastes enormous amounts of time ([NPR, 2026](https://www.npr.org/2026/04/07/nx-s1-5775917/why-infinite-scrolls-inventor-wants-to-kill-his-creation)).

**Decide.**
- For each end, empty, error and completion state: what the user sees next (the dead-end inventory is in [audit.md](audit.md)).
- Whether lists auto-load the next page, and whether the next item queues itself.
- What follows a completed session.

**iOS patterns.**
- Replace blank screens with `ContentUnavailableView` carrying an action button.
- Load the next page from `.onAppear` on the last row of a `LazyVStack`.
- Autoplay the next item with `NavigationStack` path push or an `AVQueuePlayer`.
- Completion screens end in a "Next" button and a `ShareLink`.

## 6. Piggy bank

**Mechanism.** Each input the user makes (data, preferences, creations, history) improves the product and raises the cost of leaving. Source: Hooked's Investment phase.

**Evidence.**
- IKEA effect: people overvalue what they build, but not if they fail or destroy it. Norton, Mochon & Ariely, *J. Consumer Psychology* 22(3):453-460 (2012), [Harvard DASH](https://dash.harvard.edu/handle/1/12136084).
- Netflix's own estimate is that personalization and recommendation are worth over $1B a year, mainly through lower churn. It is a self-reported estimate. Gomez-Uribe & Hunt, *ACM TMIS* 6(4), 2015, [PDF](https://gwern.net/doc/reinforcement-learning/exploration/2015-gomezuribe.pdf).

**Decide.**
- The asset the user accumulates (history, library, level, preferences) and the first moment it becomes visible.
- What the app does better because of it.
- How the user sees what they have built.

**iOS patterns.**
- Store history in SwiftData or Core Data, synced with `ModelConfiguration(cloudKitDatabase:)`.
- Show progress on a profile screen using `Charts`.
- Surface it in a widget ("Day 12, 34 puzzles solved").
- Offer an export (`ShareLink`, `Transferable`).

## Pairs with

- **Endowed progress.** Start a progress bar partly full: Nunes & Drèze, *J. Consumer Research* 32(4):504-512 (2006), [abstract](https://ideas.repec.org/a/oup/jconrs/v32y2006i4p504-512.html). On iOS, seed onboarding with "2 of 7 steps done".
- **Goal-gradient.** Effort rises as the goal nears: Kivetz, Urminsky & Zheng, *J. Marketing Research* (2006). Show a visible distance to the next milestone.
- **Loss aversion (streaks).** Kahneman & Tversky, *Econometrica* (1979). Losing a streak hurts more than extending one pleases. Pair every streak with a streak freeze and a repair option, which also reduce churn at the break point.
- **Zeigarnik (low weight).** Unfinished tasks are remembered better. The memory effect replicates weakly, so rely on goal-gradient and endowed progress and use Zeigarnik only as a light "pick up where you left off" prompt.
