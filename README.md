# TubeClose Booking — free private booking system

Per-video booking links (`/book/video-name`), Google Meet auto-created, leads stored
inside the app itself (Netlify Blobs — no Google Sheets, no external DB), private
dashboard showing per-video lead counts and details. 100% free: Netlify + Google
Calendar API.

## 1. Create the new Google account
Use the fresh Gmail you're making for this. Create a Google Calendar under it
(the default calendar is fine).

## 2. Create a Google Cloud service account (free, no billing needed)
1. Go to console.cloud.google.com -> create a new project (e.g. "tubeclose-booking").
2. APIs & Services -> Enable APIs -> enable **Google Calendar API**.
3. APIs & Services -> Credentials -> Create Credentials -> Service Account.
4. Open the service account -> Keys -> Add Key -> JSON. This downloads a `.json` file — keep it safe, never commit it.
5. Copy the service account's email (looks like `xxxx@xxxx.iam.gserviceaccount.com`).

## 3. Share your calendar with the service account
- Google Calendar (your new account) -> Settings -> your calendar -> "Share with specific people"
  -> add the service account email -> permission: **Make changes to events**.
- Go to Google Calendar Settings -> copy the **Calendar ID** (usually your Gmail address).

## 4. Push this project to GitHub
Create a repo and push this folder (or drag-and-drop deploy the folder directly to Netlify).

## 5. Deploy to Netlify
1. New site from Git -> pick the repo.
2. Netlify auto-detects Next.js (the `netlify.toml` here handles the plugin).
3. Site settings -> Environment variables -> add:
   - `GOOGLE_SERVICE_ACCOUNT_JSON` — open the downloaded JSON key file, paste its full contents as **one line**.
   - `GOOGLE_CALENDAR_ID` — from step 3.
   - `NEXT_PUBLIC_DASHBOARD_PASSWORD` — any password you want for `/dashboard`.
4. Deploy.

Lead storage (Netlify Blobs) needs **no setup or keys** — it works automatically
once the site is deployed on Netlify.

## 6. Use it
- Put a link like `https://yoursite.netlify.app/book/how-to-get-youtube-leads` in that
  video's description — the last part of the URL is the video's name, tracked automatically.
- Every video gets its own link this way — no manual dropdown, no spreadsheet.
- Go to `/dashboard`, enter your password:
  - top of the page shows a card per video with its lead count ("this video: 12 leads,
    that video: 5 leads")
  - below that, full lead details (name, email, call time, Meet link) grouped by video

## Notes / limits
- The dashboard password check is simple (client-side) — fine for keeping casual visitors
  out, not bank-grade security. Don't share the link publicly.
- Business hours/slot length/timezone are set at the top of `pages/api/availability.js` —
  edit those constants to match your schedule.
- All lead data lives in Netlify Blobs tied to this site — no external accounts, no cost.
