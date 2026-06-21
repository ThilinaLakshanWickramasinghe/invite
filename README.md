# Navodya & Thilina - Digital Wedding Invitation 🪷

A beautiful, modern digital wedding invitation created with HTML, CSS, and vanilla JavaScript. 

## ✨ Features
* **Guest Personalization:** Seamlessly customize the invitation for each guest by appending `?guest=GuestName` to the URL. The invitation dynamically greets them by name and hides the manual name input in the RSVP form.
* **Integrated RSVP System:** A built-in RSVP form that connects directly to a Google Sheet via a Google Apps Script Web App. Captures the guest's response, message, and automatically logs the number of update attempts.
* **Ambient Music:** Includes a delicate background music player with play/pause controls and volume adjustment for an immersive experience.
* **Modern Design:** Features smooth scroll-reveal animations, parallax effects on the cover portrait, and a dynamic lotus petal particle background.
* **Responsive Layout:** fully optimized for both mobile devices and desktops.

## 🚀 How to Host on GitHub Pages
This project is completely static, which makes it perfect for hosting on GitHub Pages for free!

1. Create a new repository on GitHub.
2. Upload the contents of this folder (`index.html`, `README.md`, and any media folders) to your new repository.
3. Go to the repository **Settings** > **Pages**.
4. Under "Build and deployment", select the `main` branch and click **Save**.
5. Within a few minutes, your site will be live at `https://<your-username>.github.io/<your-repo-name>/`!

## ⚙️ Google Apps Script (Backend)
To collect RSVPs, this project uses a Google Apps Script connected to a Google Sheet.

**Sheet Columns:**
* A: Timestamp
* B: Guest Name
* C: Attending (Yes/No)
* D: Message
* E: URL/Slug
* F: Attempts
* G: Status (New/Updated)

The `SCRIPT_URL` variable in `index.html` is linked to the deployed Web App URL, allowing seamless and secure data submission without reloading the page.

---
*Made with ♡ for our special day*
