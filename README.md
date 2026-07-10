# Navodya & Thilina — Digital Wedding Invitation 🪷

A beautiful, modern digital wedding invitation built with HTML, CSS, and vanilla JavaScript. Designed with a Kandyan gold theme for **26 August 2026** at **Avanee Hills, Mawanella**.

## 🌐 Live Demo

**[View the invitation →](https://thilinalakshanwickramasinghe.github.io/invite/?guest=Mr_%20Unknown)**

```
https://thilinalakshanwickramasinghe.github.io/invite/?guest=Mr_%20Unknown
```

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
- **Ambient music** — play/pause button with volume slider; music starts when the invitation is opened
- **Live countdown** — days, hours, minutes & seconds until the wedding
- **Floating nav pill** — quick links to Details, RSVP & Wishes (appears on scroll)
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

Send each guest a personalised link:

```
https://your-site.github.io/?guest=Kamal_Perera
```

- Spaces can be written as `_` (underscores) — they are converted automatically
- `?name=GuestName` also works as an alternative parameter
- Without a guest parameter, the RSVP form asks for the guest's name manually

**Examples:**
| Link | Result |
|------|--------|
| `?guest=Sunil` | "Dear Sunil" — name field hidden |
| `?guest=Anjali_Fernando` | "Dear Anjali Fernando" |
| (no parameter) | "Dear Valued Guest" — name field shown |

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

Update the `SCRIPT_URL` variable near the bottom of `index.html` with your own deployed Web App URL:

```javascript
var SCRIPT_URL = 'https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec';
```

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
| RSVP | Guest response form with validation |
| Footer | Couple monogram, date, contact |

---

*Made with ♡ for our special day*
