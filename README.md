# Navodya & Thilina — Digital Wedding Invitation 🪷

A beautiful, modern digital wedding invitation built with HTML, CSS, and vanilla JavaScript. Designed with a Kandyan gold theme for **26 August 2026** at **Avanee Hills, Mawanella**.

## 🌐 Live Demo

```
https://thilinalakshanwickramasinghe.github.io/invite/
```

> **Note:** Since the [guest eligibility check](#-guest-eligibility-check) requires a valid `?guest=Name&token=xxxxxxxx` pair, the bare link above will show "Invitation Not Found" — this is expected. Use an actual guest's link (from the Google Sheet) to view the live invitation.

---

## ✨ Features

### Invitation Cover (Envelope)
- **Animated envelope screen** — guests tap **Open Invitation** to reveal the full site
- **Content locked until opened** — invitation details stay hidden behind a solid overlay until the envelope is opened
- **Cinematic background effects:**
  - Drifting aurora gradient orbs (maroon & gold)
  - Floating lotus petals (up & down)
  - Gold sparkle particles (up & down)
  - Subtle batik tile pattern
  - Film grain texture & vignette
  - Soft pulsing glow behind the envelope
- Respects `prefers-reduced-motion` for accessibility

### Guest Experience
- **Guest personalization** — append `?guest=GuestName` or `?name=GuestName` to the URL
  - Greets the guest by name across the invitation
  - Pre-fills and hides the RSVP name field when a guest name is known
- **Guest eligibility check (name + token)** — the guest name **and** a per-guest secret token from the URL are validated against the Google Sheet (Guest Name in Column B, Token in Column H)
  - Both must match the same row, or the invitation is treated as invalid
  - If valid, the invitation opens normally
  - If not, the "Open Invitation" button is hidden and an "Invitation Not Found" message is shown instead
  - See [Guest Eligibility Check](#-guest-eligibility-check) below for why a name-only check isn't enough
- **Ambient music** — a plain mute/unmute icon button (🔊 / 🔇), no background/border/circle, just the icon with a subtle drop-shadow for visibility; music starts automatically at 30% volume when the invitation is opened, and pauses/resumes automatically when the browser tab is hidden/shown
- **Live countdown** — days, hours, minutes & seconds until the wedding
- **Floating nav pill** — quick links to Details, RSVP & Wishes (appears on scroll)
  - "Wishes" scrolls to the RSVP section, where the "message for the couple" field doubles as the wishes input
- **Scroll-reveal animations** — sections fade in as the guest scrolls
- **Parallax cover portrait** — subtle mouse-follow tilt on desktop
- **Lotus petal particles** — animated background on the main cover section

### RSVP Form
- Connects to **Google Sheets** via a Google Apps Script Web App
- **Smart validation:**
  - **Name** and **Attending?** are required (message is optional)
  - If a guest name is already known (via URL), the name field is **not** required
  - Empty required fields show a **red border** and a styled popup alert
  - RSVP cannot be submitted until required fields are filled
- Wax-seal submit button with success confirmation screen

### Design
- Fully responsive — optimized for mobile and desktop
- Kandyan gold colour palette (maroon, crimson, gold, parchment)
- Oil lamp (pahana), lotus dividers, and traditional Sri Lankan wedding styling
- Open Graph / WhatsApp preview meta tags for link sharing

---

## 📁 Project Structure

```
My Site/
├── index.html          # Full invitation (HTML + CSS + JS in one file)
├── README.md           # This file
└── assets/             # Media files (create this folder when deploying)
    ├── couple.png      # Cover couple illustration
    ├── hotel.jpg       # Venue photo
    └── music.mp3       # Background music
```

> **Note:** The `assets/` folder is referenced by `index.html` but must be added before deployment. Place your images and audio file inside it.

---

## 🔗 Guest Link Format

Every guest link **requires both a name and a token** — a name-only link is always rejected:

```
https://your-site.github.io/?guest=Kamal_Perera&token=a1b2c3d4
```

- Spaces can be written as `_` (underscores) — they are converted automatically
- `?name=GuestName` also works as an alternative to `?guest=`
- The `token` is a random per-guest code generated in the Google Sheet (Column H) — see [Guest Eligibility Check](#-guest-eligibility-check) below for how to generate and find it
- **A link missing the name, missing the token, or with a blank/wrong value for either is always invalid** — there is no "generic" or name-only link
- **Special characters in the name must be URL-encoded**, since raw characters break the query string:

  | Character | Encode as |
  |-----------|-----------|
  | `&` | `%26` |
  | `#` | `%23` |
  | space | `_` or `%20` |

  Example: `Mr_Pathum_&_Ms_Nethmi` → `Mr_Pathum_%26_Ms_Nethmi`

**Examples:**
| Link | Result |
|------|--------|
| `?guest=Sunil&token=a1b2c3d4` (correct token) | "Dear Sunil" — invitation opens |
| `?guest=Sunil&token=wrong123` (wrong token) | "Invitation Not Found" |
| `?guest=Sunil` (no token) | "Invitation Not Found" |
| (no parameters at all) | "Invitation Not Found" |

---

## 🚀 How to Host on GitHub Pages

This project is fully static — perfect for free hosting on GitHub Pages.

1. Create a new repository on GitHub.
2. Upload the contents of this folder (`index.html`, `README.md`, and the `assets/` folder).
3. Go to **Settings → Pages**.
4. Under **Build and deployment**, select the `main` branch and click **Save**.
5. Within a few minutes your site will be live at:
   ```
   https://<your-username>.github.io/<your-repo-name>/
   ```

---

## ⚙️ Google Apps Script (Backend)

RSVP responses are saved to a Google Sheet via a deployed Web App.

**Sheet columns:**

| Column | Field |
|--------|-------|
| A | Timestamp |
| B | Guest Name |
| C | Attending (Yes/No) |
| D | Message |
| E | URL / Slug |
| F | Attempts |
| G | Status (New / Updated) |
| H | Token (random per-guest secret — see [Guest Eligibility Check](#-guest-eligibility-check)) |

Update the `SCRIPT_URL` variable near the bottom of `index.html` with your own deployed Web App URL:

```javascript
var SCRIPT_URL = 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec';
```

---

## ✅ Guest Eligibility Check

A **name-only** check isn't real security — anyone could try guessing common names in the URL (`?guest=Kamal`, `?guest=Amal`, …) until one matched a row in the sheet. To close that gap, every guest link requires a **name + a random per-guest token** (Column H), and both must match the same row via a `checkGuest` action in Apps Script:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

  if (sheet.getLastRow() === 0) {
    sheet.appendRow(['Timestamp', 'Guest Name', 'Attending', 'Message', 'URL/Slug', 'Attempts', 'Status', 'Token']);
  }

  try {
    var data = JSON.parse(e.postData.contents);

    // ── Guest eligibility check (name + token must both match) ──
    if (data.action === 'checkGuest') {
      var values = sheet.getDataRange().getValues();
      var eligible = false;
      for (var i = 1; i < values.length; i++) {
        var nameMatch = String(values[i][1]).trim().toLowerCase() === String(data.guest_name).trim().toLowerCase();
        var tokenMatch = String(values[i][7]).trim() === String(data.token).trim();
        if (nameMatch && tokenMatch && data.token) {
          eligible = true;
          break;
        }
      }
      return ContentService.createTextOutput(JSON.stringify({ eligible: eligible }))
        .setMimeType(ContentService.MimeType.JSON);
    }

    // ── Fetch a guest's previous RSVP (name + token must both match) ──
    if (data.action === 'getRSVP') {
      var values2 = sheet.getDataRange().getValues();
      for (var j = 1; j < values2.length; j++) {
        var nameMatch2 = String(values2[j][1]).trim().toLowerCase() === String(data.guest_name).trim().toLowerCase();
        var tokenMatch2 = String(values2[j][7]).trim() === String(data.token).trim();
        if (nameMatch2 && tokenMatch2 && data.token) {
          var attendingVal = values2[j][2];
          var found = attendingVal !== '' && attendingVal !== undefined;
          return ContentService.createTextOutput(JSON.stringify({
            found: found,
            attending: attendingVal,
            message: values2[j][3],
            timestamp: values2[j][0] ? values2[j][0].toISOString() : null
          })).setMimeType(ContentService.MimeType.JSON);
        }
      }
      return ContentService.createTextOutput(JSON.stringify({ found: false }))
        .setMimeType(ContentService.MimeType.JSON);
    }

    // ...existing RSVP save logic (see full script for the Attempts/Status
    // fix that distinguishes a true first submission from a guest row that
    // was pre-added to the sheet for eligibility only)
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ "result": "error", "error": error }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// ── Run manually from the Apps Script editor after adding new guest names —
// fills in a random token for any row missing one in Column H ──
function generateTokens() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var lastRow = sheet.getLastRow();
  for (var row = 2; row <= lastRow; row++) {
    var nameCell = sheet.getRange(row, 2);
    var tokenCell = sheet.getRange(row, 8);
    if (nameCell.getValue() && !tokenCell.getValue()) {
      tokenCell.setValue(Utilities.getUuid().split('-')[0]);
    }
  }
}
```

**To add an eligible guest:**
1. Add a new row with the guest's name in **Column B** (other columns can stay blank).
2. In the Apps Script editor, select `generateTokens` from the function dropdown and click **▶ Run** — this fills Column H with a random token for every name that doesn't have one yet.
3. Copy the name + token from that row into the guest's link: `?guest=Name&token=xxxxxxxx`.

After editing the Apps Script code: **Deploy → Manage deployments → ✏️ Edit → Version: New version → Deploy**.

`index.html` reads the `token` URL parameter (`window.RSVP_TOKEN`) and sends it alongside `guest_name` on both the `checkGuest` and `getRSVP` requests. A guest link without a valid token is rejected exactly like an unrecognised name.

### RSVP pre-fill & update messaging

When a guest reopens their invitation link, `index.html` calls the `getRSVP` action to check for an existing response:

- If found, the **Attending choice and message field are pre-filled** with their previous answer, and `rsvpHasExistingResponse` is set to `true`.
- If the guest changes their answer and submits again, the confirmation screen shows **"updated" wording** ("Your RSVP has been updated…") instead of the first-time wording ("We've noted your RSVP…").
- A **"You last responded on [date] at [time]"** note is also shown, using the `timestamp` returned by `getRSVP`, always displayed in **Asia/Colombo (Sri Lanka) time** regardless of the guest's device timezone.

### Frontend gate (fail-closed)

The "Open Invitation" button is gated in `index.html` to close a race-condition bug where the button was briefly clickable (or stayed clickable if the eligibility check failed) before the check completed:

- When a `?guest=`/`?name=` parameter is present, the **Open Invitation button is hidden by default** and a "Verifying your invitation…" message is shown while the `checkGuest` request is in flight.
- The button is only revealed if the response confirms `eligible: true`.
- If the guest is not found, **or** the request fails for any reason (network error, script error, timeout), the button stays hidden and an "Invitation Not Found" message is shown instead — the gate fails closed, never open.
- **A missing or blank name/token is always treated as invalid** — e.g. a bare link (`/invite/`), a name-only link (`?guest=Kamal`), or an empty one (`?guest=`) all show "Invitation Not Found" just like an unrecognised name or wrong token. Every visitor must have a valid, personalised link with both a name and its matching token.
- **The couple's names and wedding date are also hidden** on an invalid link, so no wedding details are revealed to anyone outside the guest list — only the "Invitation Not Found" message is visible.

### Content obfuscation (light, not real security)

Every element that would otherwise show the couple's names, wedding date, or venue in plain text is stored as a **base64-encoded string** in a `data-b64` attribute instead — covering the envelope (`#env-names` / `#env-date`), floating nav pill, cover section, Details cards (date + venue), and footer. A single generic function, `revealAllGatedText()`, decodes and fills in every `[data-b64]` element in one pass, and only runs **after** the eligibility check (name + token) passes.

- This stops a casual "View Page Source" from showing names/date/venue in plain text anywhere on the page — the raw HTML only contains encoded strings until a valid guest link decodes them.
- **This is obfuscation, not real security.** Base64 is trivially reversible (any online decoder, or opening DevTools → Elements/Console, reveals the plain text in seconds). It only raises the bar for a casually curious visitor; it does not stop someone who deliberately wants to see the content once they're past the [token check](#-guest-eligibility-check).
- **Intentionally left in plain text** (not encoded), for reasons noted per item:
  - `<title>` and Open Graph/Twitter `<meta>` tags — needed so WhatsApp/Facebook link previews still show a proper title/description card. Since these render on link paste (before anyone clicks through), the couple's names are effectively public the moment a link is shared, regardless of the token gate.
  - `<img alt="...">` attributes — kept for screen-reader accessibility.
  - The RSVP phone number — its `sms:`/`wa.me:` link `href` attributes need the real number to function, so encoding just the visible text would be pointless.
- No meaningful performance impact — decoding is a handful of `atob()` calls that run once, after the eligibility check resolves.
- To add a newly-encoded piece of text yourself: base64-encode the string (UTF-8), give the element an empty body with `data-b64="..."` instead of the plain text, and it will be picked up automatically by `revealAllGatedText()`.

---

## 🛠️ Customisation

Key variables at the top of the `<script>` block in `index.html`:

| Variable | Purpose |
|----------|---------|
| `WEDDING_DATE` | Countdown target date & time (ISO format) |
| `SCRIPT_URL` | Google Apps Script Web App endpoint |
| `DEFAULT_GUEST` | Fallback guest name when no URL parameter is provided |

To change the couple's names, wedding date, or venue details, search `index.html` for `Navodya`, `Thilina`, `26 August`, and `Avanee Hills`.

---

## 📱 Sections

| Section | Description |
|---------|-------------|
| Envelope Cover | Animated open screen with background effects |
| Cover | Couple portrait, names, date, oil lamp |
| Details | Wedding date, time, venue with map link |
| Countdown | Live timer to the wedding day |
| RSVP | Guest response form with validation; also doubles as the "Wishes" destination (message field) |
| Footer | Couple monogram, date, contact |

> **Note:** There is no separate guestbook/wishes wall — the nav pill's "Wishes" link points to `#rsvp`, and guests leave their wishes via the optional message field on the RSVP form.

---

*Made with ♡ for our special day*
