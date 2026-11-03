# OSCAR waitlist redesign: demo prototype

A clickable prototype for the demo video. All data is fake, and reloading the page resets it.

Open `index.html` in a browser. It needs no install, server or network. It's laid out for a 1440×900 window.

## Demo shortcuts

Shortcuts work while the demo panel is hidden.

| Key | Action |
| --- | --- |
| `D` | Show or hide the demo panel |
| `1` | Move CS 6750 up one position (sends a position text) |
| `2` | Open a seat in CS 6750 (auto-enroll succeeds) |
| `3` | Add a time conflict, then open a seat in CS 6750 (re-check fails) |
| `4` | Open a seat in CS 7641 (fallback claim) |
| `5` | Make the next text fail to deliver (press again to cancel) |
| `6` | Advance the clock 1 hour |
| `7` | Advance the clock 6 hours |
| `8` | Advance the clock 23 hours |
| `R` | Reset everything to the seed data |

Keys are ignored while the cursor is in the search box, a dropdown or the date field. Click an empty part of the page before pressing one.

## Suggested recording order

1. Dashboard, waiting: point out position and movement.
2. Click **Edit** on CS 6750 to show auto-enroll, the swap rule and the pre-flight check. Cancel.
3. Press `1`: the position text arrives on the phone.
4. Press `2`: the seat opens at 9:40 pm and Josh is registered automatically. Show the text and the dashboard.
5. Click **Undo**, then **Undo swap**. Press `R`, then `2` again to show the outcome kept.
6. Press `R`, then `3`: the re-check fails and nothing is dropped. Click **Confirm**.
7. Press `R`, then `4`: tap the **CLAIM** chip in the phone thread and the dashboard updates live.

Optional extras:

- **Delivery failure:** press `R`, `5`, `1`. The card shows "Text not delivered — sent by email instead" and the email opens on the phone.
- **Countdowns:** after `2`, `3` or `4`, press `6`, `7` or `8`. If a countdown runs out, the undo window closes or the held seat is released.

## Good to know

- **Clock:** it starts at Tue, Oct 27, 8:30 pm and only moves on demo keys. `1` adds 10 minutes. A seat opening jumps to 9:40 pm, or adds 2 minutes if the clock is already past that.
- **Phone:** a new alert shows on the lock screen for about 3 seconds, then the app opens. Tap the notification to open it sooner.
- **Keys mid-story:** `2`, `3` and `4` always fire. If the course isn't on the waitlist, they put it back first. Press `R` between paths for a clean take, because the thread and inbox keep their history otherwise.
- **Filming "Join waitlist":** both full courses start on Josh's waitlist, so search shows **Edit** for them. Click **Edit**, then **Leave waitlist**, and search then shows **Join waitlist**.
- **Pre-flight cross:** click **Edit** on CS 7641 and tick auto-enroll. It shows a time conflict with VIP 6603, which clears if you choose to drop VIP 6603.
