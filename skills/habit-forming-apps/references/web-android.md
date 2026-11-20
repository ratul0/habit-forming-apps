Read this when the target app is web or Android: the equivalents of the iOS patterns in the other references.

# Web and Android equivalents

## Levers

- **Itch.** Web: PWA push plus an installable home-screen icon. Android: app widgets (Glance) and notification channels.
- **Lazy thumb.** Web: sticky bottom action bar, `100dvh` layout. Android: Material FAB or bottom-anchored button.
- **Paved path.** Web: `<input>` defaults and `autocomplete`. Android: `DataStore` defaults and pre-selected `MaterialTimePicker`.
- **Slot machine.** Web: Web Animations API and `canvas-confetti`. Android: Compose `AnimatedVisibility` and `HapticFeedback`.
- **Bottomless pit.** Web: `IntersectionObserver` for infinite scroll, with a "Load more" button as the no-JS fallback. Android: Paging 3 with `LazyColumn`.
- **Piggy bank.** Web: IndexedDB plus account sync. Android: Room plus Backup/Auto Backup.

## Notifications

Web: Web Push needs a user gesture before `Notification.requestPermission()`. Android 13+: request `POST_NOTIFICATIONS` after the first win, and use one notification channel per category.

## Metrics

Same events; web uses cookieless analytics where possible, Android uses Firebase Analytics events with the same names.
