# Changelog — Lab Equipment Borrow

All notable changes per app version. The version shows next to the title in-app (e.g. `v2.5`).
Newest first.

---

## v2.5 — 2026-06-16
**Added**
- Student **"Active activity"** list under the dashboard stat boxes: the latest 3 open
  items (Pending / Reserved / Approved / Return Requested), newest first, each showing the
  status, equipment, and a date (reservations show their start date).
- A **"See history →"** shortcut as the 4th row, opening the full History tab.

_Resolved items (Returned / Denied / Cancelled) stay in History only._

---

## v2.4 — 2026-06-16
**Added — Reservations**
- Requests with a **future start date** are saved as **Reserved** bookings (held until the
  start date, then auto-converted to a normal borrow request).
- New **Reserved** tab for both students and admins. Students can **Cancel** their own;
  admins can **Revoke** any.
- Per-item **timetable** popup — tap the 📅 icon (student catalog) or the **Reserved** badge
  (admin equipment) to see all bookings for that item.
- Items that are free now but booked later still read **Available** with a reserved marker.

**Added — Overlap prevention**
- Booking dates that clash with an existing reservation/loan are **blocked**, with a popup
  listing the conflicting bookings (shared boundary day counts as a clash).

**Backend**
- Optional daily Apps Script trigger (`promoteReservations`) auto-promotes due reservations
  even when nobody opens the app — see `APPS_SCRIPT_RESERVATIONS.md`. A client-side fallback
  runs whenever an admin opens the app. No change to the existing web-app endpoints.

---

## v2.3 — 2026-06-16
**Changed**
- **Much faster login / first load** (~8s → ~3s): data sheets are now fetched in parallel
  instead of one-after-another, and the sign-in spinner stays until the dashboard is ready
  (no more lingering on the login screen).
- Added a brief "Loading…" state when reopening while signed in.

---

## v2.2 — 2026-06-16
**Added**
- Admin **"Return due"** box in the dashboard: counts items due back **today or overdue**;
  click it for a popup listing each item with the borrower's name, ID, dates, phone, and advisor.

---

## v2.1 — 2026-06-14
**Added**
- **Refresh** button on the tab bar (both student and admin) to reload the latest data
  from the Google Sheet without a full page reload.

---

## v2.0 — 2026-06-14
**Initial public release** (hosted on GitHub Pages, installable as a phone app / PWA).
- Student + admin roles; student registration and sign-in.
- Borrow flow: request → admin approve/deny → in-hand → request return → admin approve return.
- Equipment catalog with categories, tags, and status (Available / Borrowed / Maintenance / Retired).
- Admin dashboard stats; per-student "in hand" view.
- **Fixed:** phone-view — wide tables now scroll sideways instead of breaking the layout.

---

_Format: this file is updated whenever the in-app version (`APP_VERSION`) changes._
