# Wedding Invitations

A single-file, mobile-first digital wedding invitation (Persian / RTL). Each couple gets its own folder and its own page, hosted for free on GitHub Pages.

**Live example:** `/invites/yasaman-moeen-4d8k/`

## What a page includes

- Veil opening: guests swipe up or tap to lift the veil, which starts the music
- Invitation card with date, time and venue
- Countdown to the ceremony and an "Add to Google Calendar" button
- Venue address with copy button and a Google Maps link
- "حضور پیدا نمی‌کنم" (not attending) button that sends a Bale message to the couple
- Works offline once loaded; fonts are embedded, so no Google Fonts are needed

## Repository layout

```
invites/
├── README.md
├── yasaman-moeen-4d8k/
│   ├── index.html   ← the invitation (all code and fonts in one file)
│   └── song.mp3     ← optional background music
└── another-couple-xyz9/
    └── index.html
```

Each folder name is also the URL, so keep a random suffix (e.g. `-4d8k`) to make links hard to guess.

## Creating a new invitation

1. Copy an existing couple folder and rename it.
2. Open `index.html` and edit the **CONFIG** block near the bottom of the `<script>` section:

| Field | Meaning | Example |
|---|---|---|
| `titleFa` | Title shown in the browser tab and calendar entry | `"جشن پیوند یاسمن و معین"` |
| `weddingISO` | Start date and time, with timezone | `"2026-11-19T19:00:00+03:30"` |
| `endISO` | End time, used for the calendar entry | `"2026-11-19T23:30:00+03:30"` |
| `dateText` | Date shown to guests (Persian) | `"پنجشنبه ۲۸ آبان ۱۴۰۵"` |
| `timeText` | Time shown to guests | `"ساعت ۱۹:۰۰"` |
| `venue` / `venueCity` | Venue name and city | `"تالار شاپرک"`, `"رشت"` |
| `mapUrl` | Google Maps link for the venue | paste from Google Maps |
| `baleToken` | Bale bot token (see below) | `"123456789:AbCd..."` |
| `notifyChatIds` | Chat IDs of the bride and/or groom | `[111111111, 222222222]` |
| `musicFile` | Music file next to `index.html` | `"song.mp3"` |
| `heroPhoto` | Optional couple photo next to `index.html` | `"photo.jpg"` |

3. Some text is written directly in the HTML, so search the file (Ctrl/Cmd+F) and replace it:
   - Couple names: `یاسمن و معین`, `Yasaman`, `Moeen`
   - Monogram: `y &amp; m`
   - Hero date: `19 · 11 · 2026`
   - Invitation text: `به جشن پیوند ما دعوتید`
   - Venue heading and address: `تالار شاپرک`, and the address paragraph under `id="addr"`

4. Test the page on a phone before sending it.

## Not-attending notifications (Bale)

The button sends a message from a Bale bot directly from the page, with no server.

1. In Bale, message `@botfather`, send `/newbot`, and copy the token. Use a new bot for each wedding.
2. The couple opens the bot and presses Start.
3. Get their chat IDs from `https://tapi.bale.ai/bot<TOKEN>/getUpdates` (look for `"chat":{"id":`).
4. Put the token and IDs into `baleToken` and `notifyChatIds`.
5. Test by pressing the button with a test name. The page can't confirm delivery, so check that the message arrives in Bale.

**Security note:** the token is visible in the page source. Anyone who finds it could send messages through the bot. Revoke it with `/revoke` in @botfather after the wedding.

## Publishing on GitHub Pages

1. Create a **public** repository named `invites`.
2. Upload the couple folders (**Add file → Upload files**).
3. Go to **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. The page is available at `https://<your-username>.github.io/invites/<couple-folder>/`.

Notes:
- The repository is public, so anyone with the exact link can open it. Get permission before publishing couples' photos.
- To update a page, upload the new `index.html` to the same folder (it overwrites the old one).

## Limitations

- No database: RSVP and guestbook forms are not included, and guest photo uploads are not supported.
- "Not attending" is one-way: the couple is notified, but the page cannot confirm delivery or track responses.
- Music plays only after the veil is lifted, because browsers block autoplay.
