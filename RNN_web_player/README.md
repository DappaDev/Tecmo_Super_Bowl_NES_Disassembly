# RNN E League – Tecmo Super Bowl Web Player

A lightweight NES player for the members-only area of the league site. It runs on
[jsnes](https://github.com/bfirsh/jsnes) (Apache-2.0): about 150 KB total, or ~35 KB gzipped,
with no plugins or WebAssembly.

## Files

| File | What it is |
|---|---|
| `index.html` | The player page (layout, styles, help screen) |
| `player.js` | Controls, saves, audio, ROM loading |
| `jsnes-core.min.js` | The emulator core |
| `LICENSE-jsnes.txt` | jsnes license; keep it with the core |

## Setup

1. Copy the four files into a folder inside your members-only area, for example `/members/tecmo/`.
2. Put the league ROM somewhere **only signed-in members can request**, for example
   `/members/tecmo/tecmo27rnn.nes`, then set the path on the `<body>` tag in `index.html`:

   ```html
   <body data-rom="/members/tecmo/tecmo27rnn.nes" data-title="Tecmo 27 · RNN E League">
   ```

3. Make the server **check the login on the ROM request itself**, not only on the page. A hidden
   link isn't protection: anyone who has the URL can download the file. If a member isn't signed in,
   the server should return 401 or 403. The player then shows "Please sign in to the members area to play."
4. Serve it over HTTPS. Browsers block game audio and controllers on insecure pages.

The ROM is fetched with the member's session cookie (`credentials: "same-origin"`), so ordinary
session-based logins work without extra code. If the ROM can't be loaded, the page offers a
"Choose ROM file…" button. That's handy for admins testing a new build locally.

To ship a new ROM build, replace the file. Season saves are tied to each ROM build, so a new
build starts with a fresh season. To keep the old saves, leave the old file in place and
point a separate page at it.

## Controls

**Keyboard, Player 1:** Arrows or WASD to move · `X` = A · `Z` = B · `Enter` = Start · `Shift` = Select
**Keyboard, Player 2:** `I J K L` to move · `.` = A · `,` = B · `]` = Start · `[` = Select
**Player:** `P`/`Esc` pause · `F2` save state · `F4` load state

**Touch (iPhone, iPad, Android):** the on-screen pad appears automatically. You can slide across the
D-pad for diagonals, slide between B and A, and hold several buttons at once. In landscape the pad
sits beside the game; in portrait it sits below it. The ✚ button turns it on or off.

**Controllers:** Xbox, PlayStation, 8BitDo and similar controllers work over Bluetooth or USB. The first
controller is Player 1 and the second is Player 2. Connecting one hides the touch pad.

## Saves

- **Season mode** uses the cartridge's battery save. It's stored automatically in the member's
  browser every few seconds, and also when the tab is hidden or closed.
- **Save / Load** stores one quick snapshot per device.
- Saves stay on that device and browser. Clearing site data or using a private window removes them.

## Notes

- On iPhone, the ringer/silent switch also mutes web audio.
- The game pauses automatically when the tab is hidden or the phone locks.
- "Add to Home Screen" on iOS and Android opens it full screen without the browser bar.
