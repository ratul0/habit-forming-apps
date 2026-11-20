Read this when the app has friends, multiplayer, challenges, leaderboards or sharing: how each lever works through other people.

# Social loops

- **Itch.** A friend's action is the strongest external trigger ("Mia challenged you", "Your turn against Sam"). Turn and challenge pushes are functional, not marketing; they still count toward the daily cap except a direct "your turn".
- **Lazy thumb.** Accept or play from the notification in one tap (deep link straight to the match). Put a Rematch button on every result screen.
- **Paved path.** The first match starts at once: quick match, or a bot/ghost opponent when the user has no friends yet. Invite contacts after the first win, never before.
- **Slot machine.** The opponent makes the outcome variable. Show near rivals ("2 points behind Sam") instead of the global top of a leaderboard.
- **Bottomless pit.** Result leads to Rematch, next opponent, or share; a waiting async turn is always one tap away.
- **Piggy bank.** The friend list, head-to-head records, rank and rivalry history are what the user would lose.

## iOS patterns

- Game Center sign-in: `GKLocalPlayer.local.authenticateHandler`.
- Async turns: `GKTurnBasedMatch` (Game Center sends the turn notifications).
- Rankings: `GKLeaderboard`; entry point: `GKAccessPoint`.
- Invites: `ShareLink` with a universal link.

## Cold start

Never show an empty lobby; bots or ghost replays fill it.

## Related

Events are in the social events table in [metrics.md](metrics.md). Kids rules are in [compliance.md](compliance.md).
