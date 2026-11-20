Read this when auditing an existing app, or when scoring a design board against the levers (SKILL.md step 4).

# Audit: scoring, dead ends, and output

## 1. Score each lever 0-3

Score only what you can observe in the code, the board, or a run of the app.

| Lever | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| Itch | No trigger at all | Only generic reminders ("come back!") | One trigger at a named moment with feeling-based copy | Trigger plus widget or Live Activity, and sessions that start unprompted are measured |
| Lazy thumb | Core action 4+ taps or behind a menu | 2-3 taps | 1 tap, primary button is the largest control, bottom zone | 1 tap, also available from a widget or App Intent, with a reduced "done" bar |
| Paved path | Every setting is empty, user must choose | A few defaults, onboarding still blocks on choices | Every decision has a default; app opens to the core action | Defaults pre-filled from known data, and keep-rate per default is measured |
| Slot machine | Identical result every session | Cosmetic variety only | A reward that varies in size or content | Varied reward with a written distribution and a fresh element on day 30 |
| Bottomless pit | Dead ends on completion, empty or error | Some states lead on, some don't | Every state has a next action | Every state leads to a specific next action, tracked in events |
| Piggy bank | Nothing accumulates | History exists, not shown | History visible and used to improve the product | Users see what they've built, it is exportable, and invested users' D30 is measured |

A lever below 2 goes back to `/design` for a fix (SKILL.md step 4).

## 2. Dead-end inventory

List every state that ends a flow and where it leads. A dead end is any state whose only exit is the system back button.

1. Enumerate screens. For SwiftUI, list every `View` reachable from the root `NavigationStack`, `TabView`, sheet or `fullScreenCover`.
2. For each, record four states: **end** (list exhausted), **empty** (no data), **error** (failure), **completion** (task done).
3. Fill a table: `State | Screen | What the user sees | Where it leads`.
4. Mark rows whose "leads" cell is empty, "back" or "dismiss". Each needs a specific next action (replay, next item, share, set tomorrow's reminder).
5. Done when no row is marked.

## 3. Grep hints for SwiftUI codebases

Run from the repo root.

```sh
# Empty and error states
rg "ContentUnavailableView|\.alert\(|\.confirmationDialog\(|ProgressView\(\)" --type swift
# Sheets and covers; check each for a follow-up screen
rg "\.sheet\(|\.fullScreenCover\(|\.popover\(" --type swift
# Completion and results screens
rg -i "complete|finished|result|summary|success" --type swift -l
# Notifications and permission timing
rg "UNUserNotificationCenter|requestAuthorization|UNNotificationRequest|UNCalendarNotificationTrigger" --type swift
# Widgets, Live Activities, intents
rg "WidgetKit|ActivityKit|AppIntent|AppShortcutsProvider" --type swift
# Onboarding choices without defaults
rg '@State.*(selected|choice)\w*\s*(:[^=]*)?=\s*(nil|"")' --type swift .
rg "OnboardingView|Onboarding" --type swift -l
# Streak or reward logic
rg -i "streak|reward|xp|level" --type swift
```

What to check in the hits:
- Every `.alert` has a button that retries or goes forward, not only "OK".
- Every `ContentUnavailableView` has an `actions:` closure with a real action.
- `requestAuthorization` is called after a first win, not at launch.
- Selections that start as `nil` or `""` in onboarding need a default.
- Sheets that dismiss to the same screen need a follow-up (a next item, a result).

## 4. Output to append to `docs/habit-plan.md`

If `docs/habit-plan.md` does not exist, create it from [`plan-template.md`](plan-template.md) first. Then add a row to the **Audit scores** table:

```markdown
| 2026-11-20 | 1 | 2 | 1 | 0 | 1 | 2 | Notification permission asked at launch; 6 dead ends |
```

Then add a prioritised fix list under it, highest impact on day-1 retention first:

```markdown
### Fixes (priority order)

1. **<Lever>** (<score> → <target>): <the specific change, with file or screen>. Metric: <event or rate>.
2. …
```

Order by lever score (lowest first), then by how early in the first session the problem occurs. Name the file or screen for every fix.
