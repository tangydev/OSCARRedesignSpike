# Build: OSCAR Waitlist Redesign — Final Prototype (for a demo video)

## Context

I'm a student in a graduate HCI course. My project redesigns how students use waitlists in OSCAR, Georgia Tech's course registration system. I tested three low-fidelity designs with users, and the final design combines them. I need a **medium-fidelity, clickable prototype** that I will screen-record for a ~5 minute demo video.

This is a prototype, not a product: no backend, no real login, no real SMS or email. All data is fake and lives in memory. What matters is that every step of the flow looks believable, is readable on video, and can be triggered on cue while I record.

Do not use Georgia Tech logos or official branding. Label the app "OSCAR (prototype)" and use a neutral visual style.

## Tech

- Vite + React + TypeScript, plain CSS or Tailwind. Runs with `npm install && npm run dev`.
- No backend and no external services. Keep all state in one store (React context or Zustand) so the demo controls can drive it.
- Optimize for a 1440×900 browser window recorded at 1080p. Use body text of at least 16px, since this will be watched as video.

## Layout

One screen, split in two, so the video can show both channels at once:

- **Left (~70%): the OSCAR desktop page**, "Register for Classes, Spring 2027".
- **Right (~30%): a phone frame** showing the student's lock screen and text-message thread, plus an email inbox tab inside the phone.
- **Demo control panel:** a small floating panel, toggled with the `D` key and hidden by default, so it never appears in the recording unless I want it to.

## The scenario (seed data)

The student is "Josh". Credit limit: 21 hours.

| Course | Status | Details |
| --- | --- | --- |
| CS 6750 Human-Computer Interaction (3 cr) | Waitlisted | Position 3 of 12. Auto-enroll ON, with swap rule "If I get in, drop CS 6400." Alerts: text + email |
| CS 7641 Machine Learning (3 cr) | Waitlisted | Position 8 of 15. Auto-enroll OFF. Alerts: text |
| CS 6400 Database Systems (3 cr) | Registered | |
| VIP 6603 Vertically Integrated Project (3 cr) | Registered | |

The story: a seat opens in CS 6750 on Tuesday, Oct 27 at 9:40 pm while Josh is away from their laptop. Show a fake clock in the phone status bar and the page header, and have it advance with the demo controls.

## The final design (combined from three earlier prototypes)

1. **Auto-enroll is the core** (from Prototype C). The student can tell OSCAR to register them automatically when a seat opens, optionally dropping a backup course. A pre-flight check runs at setup.
2. **Alerts report outcomes and position** (from Prototype B). Texts and emails tell the student what happened and where they stand, rather than asking them to act. Users asked to see their position inside the alerts.
3. **The dashboard shows position** (from Prototype A). A "Your Waitlists" panel at the top of the page shows each course's position, a progress bar, recent movement, auto-enroll status, and alert channels.
4. **Every registration made from the waitlist can be undone for 24 hours**, not only swaps. Users asked for undo everywhere.
5. **The automatic drop is re-checked at the moment the seat opens.** If anything changed since setup (a new time conflict, a hold, going over the credit limit), OSCAR does NOT drop or register automatically. It holds the seat and asks the student to confirm, with a 24-hour countdown. Users worried that "if something changes after the check, I could lose a class I wanted."
6. **Fallback for courses without auto-enroll** (from Prototypes A and B): when a seat opens, the dashboard card shows a countdown and a single Claim button, and a text arrives that the student can answer with CLAIM or PASS.
7. **Delivery status:** each card shows whether the last alert was delivered ("Text delivered 9:40 pm"). If delivery fails, it says so and falls back to email. Users worried about missed or delayed notifications.

## Screens and states to build

1. **Dashboard: waiting.** The Your Waitlists panel with both waitlist cards (position "3 of 12", progress bar, "↑2 since yesterday", an "Auto-enroll: ON, drops CS 6400" badge, alert channels), then a Registered list below.
2. **Join waitlist / auto-enroll setup dialog** (opened with a "Join waitlist" button on an "Add course" search result, or with "Edit" on a card):
   - Alert channels: Text, Email, or both.
   - "Register me automatically if a seat opens" checkbox.
   - Optional swap rule: "If I get in, drop: [course dropdown]".
   - Rule expiration date.
   - An inline pre-flight check with a check or cross for each of: holds, prerequisites, time conflicts, and credit hours after the swap.
3. **Position-update text** on the phone: "OSCAR: You moved to #2 of 12 on the CS 6750 waitlist. Auto-enroll is on."
4. **Seat opens, auto-enroll succeeds:**
   - The phone shows a lock-screen notification, then this text: "OSCAR: You're registered in CS 6750 and CS 6400 was dropped. Undo within 24 h: [link]. Your other waitlist: CS 7641 #8 of 15."
   - The dashboard card flips to "Registered ✓" with a timestamp and an "Undo (23 h 59 m left)" button. The schedule list updates.
   - The email inbox gets a matching email.
5. **Seat opens, re-check fails** (triggered from the demo panel after adding a conflict):
   - The card shows "Seat held — needs your OK", explains why ("New time conflict with CS 6400's lab"), and shows the countdown with Confirm and Release buttons.
   - The phone gets a text explaining the problem and asking the student to reply CLAIM or PASS.
   - Nothing is dropped.
6. **Undo flow:** from the dashboard button or the text link. A dialog says exactly what will happen ("Drop CS 6750 · Restore CS 6400 (seat held for you)"), with "Undo swap" and "Keep CS 6750" buttons. After undoing, show a confirmation and restore the schedule.
7. **Fallback claim for CS 7641** (auto-enroll off): a seat opens, the card shows a countdown and a single Claim button, and a text arrives asking for CLAIM or PASS. Replying CLAIM in the phone thread (with a tappable quick-reply chip) registers the course, and the dashboard updates live.
8. **Delivery failure state:** a card shows "Text not delivered — sent by email instead".

## Demo control panel (press D)

Each control is a button plus a keyboard shortcut, so I can trigger events on cue while recording:

- `1` Move CS 6750 up one position (sends a position text).
- `2` Open a seat in CS 6750 (auto-enroll succeeds).
- `3` Add a time conflict, then open a seat in CS 6750 (the re-check-fails path).
- `4` Open a seat in CS 7641 (fallback claim path).
- `5` Make the next text fail to deliver.
- `6` Advance the clock by 1 hour, 6 hours, or 23 hours (countdowns update).
- `R` Reset everything to the seed data.

Animate state changes (about 300 ms for a card changing state or a new message) so they are visible on video. Phone notifications should slide in.

## Visual style (medium fidelity)

- Clean and modern, but plainly a prototype: neutral grays, one accent color for primary actions, and one clear status color for "Registered". Status must never rely on color alone; always pair it with an icon and text.
- Real buttons, inputs, and labels; visible keyboard focus; text contrast of at least 4.5:1.
- No lorem ipsum. Use the scenario data everywhere.

## Out of scope

Real authentication, Duo, a backend, persistence across reloads, mobile responsiveness of the left pane, and any course other than the four above.

## How to work

1. Restate the flows and propose a component and state plan before writing code, then build.
2. Run the dev server and click through every path in the browser: waiting → position update → auto-enroll success → undo; re-check fails → confirm; fallback claim via CLAIM; delivery failure; reset. Fix anything that doesn't work.
3. Finish with a short README that lists the demo shortcuts and a suggested recording order.

## Suggested recording order (for the README)

1. Dashboard, waiting: point out position and movement.
2. Open the setup dialog: auto-enroll, swap rule, pre-flight check.
3. Press `1`: position text arrives on the phone.
4. Press `2`: the seat opens at 9:40 pm and Josh is registered automatically; show the text and the dashboard.
5. Undo the swap, then reset and replay to show the outcome kept.
6. Press `3`: the re-check-fails path; nothing is dropped and the student confirms.
7. Press `4`: the fallback claim by text for a course without auto-enroll.
