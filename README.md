# Habit-forming apps

[![skills.sh](https://skills.sh/b/ratul0/habit-forming-apps)](https://skills.sh/ratul0/habit-forming-apps)

One agent skill for designing and auditing the **daily-habit loop** of an app before and alongside its UI: why the user opens the app, what they do in one tap, and why they come back tomorrow. It turns six retention levers into a written habit plan, then hands that plan to the `/design` skill so the screens follow from the habit instead of the other way round. iOS-first, and it applies to web and Android.

Install on Claude Code, Cursor, Codex, and other agents that read `SKILL.md`:

```bash
npx skills add ratul0/habit-forming-apps
```

Install globally, for every project:

```bash
npx skills add ratul0/habit-forming-apps -g
```

Or clone it into a project:

```bash
git clone https://github.com/ratul0/habit-forming-apps.git
```

The `npx skills` CLI discovers the skill at `skills/habit-forming-apps/SKILL.md`. The skill auto-applies from its `description`; invoke it by name when you want it specifically.

## Example prompts

- "Users stop opening my app after day 1. Audit the habit loop."
- "Design the habit loop for my new flashcard app before we build the UI."
- "Write the push notifications and permission flow for my habit tracker."
- "What should our habit moment be, and which retention events should we track?"
- "Add friend challenges to my quiz game so people come back daily."

## The six levers

Six levers, in the order a user meets them. The first three get the user in; the last three keep them coming back.

| # | Lever | The question it answers |
|---|---|---|
| 1 | **Itch** | What feeling makes the user open the app with no notification at all? |
| 2 | **Lazy thumb** | Can the core action be done in one tap, by someone with low motivation, one-handed? |
| 3 | **Paved path** | What happens if the user decides nothing? |
| 4 | **Slot machine** | Is the reward still a surprise on day 30? |
| 5 | **Bottomless pit** | Where does every screen, including "done", "empty" and "error", lead next? |
| 6 | **Piggy bank** | What would the user lose by deleting the app today? |

The **habit moment** ties them together: the countable threshold of core actions after which users stay. Every plan names one.

## How it runs

Pick the mode from the request:

| The request | Mode |
|---|---|
| New app, or a new feature that should become a habit | **Design** (steps 1 to 5) |
| Existing app: "why don't people come back", retention review | **Audit** (step 1, the audit reference, then steps 2 to 5) |
| Push notifications, reminders, re-engagement copy | **Notify** (step 1, then the notifications reference) |
| Habit moment, retention metrics, analytics events | **Measure** (step 1, then the metrics reference) |

The steps:

1. **Read the app.** Codebase, docs, store listing, and any earlier plan. Pin down the user, the feeling (the internal trigger), and the core action, one line each.
2. **Write the habit plan** to `docs/habit-plan.md`. Every lever gets a decision (a named screen, control, default, reward or notification) plus the metric that will show whether it works, and the first-session order and paywall timing are filled in.
3. **Lay it out with `/design`.** For a new app, or an app with no design board, the plan's Board brief creates a new board covering the first session, the core loop, every end/empty/error state, and the notification surfaces. For an app that already has a board, `/design` adds a `Habit loop v<N>` flow to it and redesigns existing screens only where the plan changes them.
4. **Review the board against the levers.** Score each lever; anything below 2 goes back to `/design` with a specific fix, for at most 2 rounds, and every screen must end in a next action.
5. **Store and legal check.** A short checklist of what gets an app rejected or creates legal exposure.

Creating or extending the design board uses Claude's `/design` (Design artifacts). In other agents the plan gets a Screens section instead, and step 4 scores that list.

Every run ends with its mode's output: the path to `docs/habit-plan.md` plus, for Design and Audit, the board link (or Screens section), the lever scores and the three changes most likely to move day-1 retention; for Notify, the Notifications table; for Measure, the habit moment and the event list.

## Sample output

An excerpt of the Levers table in a generated `docs/habit-plan.md` (fictional daily puzzle app):

| Lever | Decision | Screen / surface | Metric |
|---|---|---|---|
| Lazy thumb | App opens straight into today's puzzle; Start button pinned bottom, 56pt; a puzzle counts as done at the first correct row | Today | Seconds from open to first move |
| Paved path | Difficulty defaults to Medium, reminder time defaults to the user's first-solve hour, no account until the first streak day | Onboarding, Settings | % who keep each default |
| Bottomless pit | Puzzle-complete screen leads to "Next puzzle" and "Set tomorrow's reminder"; empty archive offers yesterday's puzzle | Result, Archive | Puzzles per session |

`SKILL.md` holds the workflow and the routing. Everything topic-specific loads on demand from `references/`:

| File | Owns |
|---|---|
| `references/levers.md` | Each lever's mechanism, evidence, the decisions it forces, and iOS patterns. Loaded for every Design or Audit run |
| `references/first-session.md` | The order of the first session (asks, account, notifications) and paywall timing against the habit moment |
| `references/audit.md` | The scoring rubric for the six levers, and the audit of an existing app's retention and dead ends |
| `references/notifications.md` | Push notifications, reminders and re-engagement copy (Notify mode) |
| `references/metrics.md` | The habit moment, D1/D7/D30 retention, and analytics events (Measure mode) |
| `references/social.md` | Social and multiplayer loops: friend challenges, async turns, leaderboards, Game Center |
| `references/web-android.md` | Web and Android equivalents of the iOS patterns |
| `references/compliance.md` | The store and legal checklist: marketing push opt-in, random paid rewards, Kids Category, EU/FTC dark-pattern rules |
| `references/plan-template.md` | The template for `docs/habit-plan.md`, including the Board brief |

## Extending this skill

This repo is built to grow by accretion. A new learning should land as exactly one of two moves.

**Move 1: a new rule in an existing file.** Add one bullet or one table row to the single file that owns that topic. Nothing else changes.

**Move 2: a new topic.** Add `references/<topic>.md` and one new row to the routing in `SKILL.md`. The new file opens with a one-line statement of when to read it.

Rules that keep the two moves cheap:

- **One topic per file, single source of truth.** If a rule could plausibly live in two files, pick one owner and never state it in the other. `audit.md` is the deliberate exception: it *checks* rules that other files own, so it names them without redefining them.
- **State positives.** Write the move to make, not the mistake to avoid. Every rule must change what an agent does versus its default.
- **Make decisions, not principles.** "Make onboarding easy" is a principle; "app opens straight into today's puzzle, Start button pinned bottom, 56pt" is a decision.
- **Keep `SKILL.md` short.** It is the router, not the manual. Anything lever-specific or topic-specific belongs in `references/`.
- **Watch the density budget.** When a reference file grows past roughly 1,200 words, split its largest section into its own `references/<topic>.md` and add a routing row. That is Move 2.

## Credits & sources

Inspired by a YouTube app-design breakdown of a six-step retention framework. The framework was rewritten from scratch for agent use and grounded in published work:

- Nir Eyal, *Hooked* (the trigger, action, variable reward, investment loop)
- BJ Fogg's Behavior Model (behavior as motivation, ability and a prompt)
- Johnson & Goldstein (2003), on the power of defaults
- B. F. Skinner's variable-ratio reinforcement schedules
- Aza Raskin on stopping cues and infinite scroll
- The IKEA effect (Norton, Mochon & Ariely, 2012), on valuing what we helped build

No source text is reproduced. This is original agent instructions, not a copy of any video, book, or course.

## What this is not

- **Not affiliated with Apple, TikTok, Duolingo or Tinder.** They are named only as familiar examples of patterns.
- **Not legal advice.** The compliance checklist flags common store and regulatory risks; check the App Store Review Guidelines and the rules that apply to you, and ask a lawyer for anything that matters.
- **Not a dark-pattern kit.** It pulls the retention levers at full strength, and keeps them inside App Store, Google Play and consumer-protection rules (see `references/compliance.md`).

## License

MIT, see [LICENSE](LICENSE).
