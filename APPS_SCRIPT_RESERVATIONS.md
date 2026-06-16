# Reservations — Apps Script auto-promote (one-time setup)

The web app handles reservations entirely through the existing `log` sheet — **no change
to `doGet`/`doPost` or the web-app deployment is required.** The only optional backend
piece is a daily trigger that turns a reservation into a borrow request on its start date,
even when nobody opens the app that day.

> The app already does this client-side whenever an **admin opens it**, so this trigger is
> a safety net for days the admin doesn't log in. Recommended, not strictly required.

## 1. Add the function

Open the Sheet → **Extensions → Apps Script**, and paste this function at the bottom of the
existing script (it reuses the `getSheet` / `headersOf` helpers already in the file):

```js
function promoteReservations() {
  var sh = getSheet("log"), headers = headersOf(sh);
  var idx = {}; headers.forEach(function (h, i) { idx[h] = i; });
  var tz = SpreadsheetApp.getActiveSpreadsheet().getSpreadsheetTimeZone();
  var todayStr = Utilities.formatDate(new Date(), tz, "yyyy-MM-dd");
  var lastRow = sh.getLastRow();
  if (lastRow < 2) return;
  var rng = sh.getRange(2, 1, lastRow - 1, headers.length);
  var vals = rng.getValues(), changed = false;
  for (var r = 0; r < vals.length; r++) {
    var bdRaw = vals[r][idx["borrow_date"]];
    var bd = (bdRaw instanceof Date)
      ? Utilities.formatDate(bdRaw, tz, "yyyy-MM-dd")
      : String(bdRaw);
    if (String(vals[r][idx["status"]]) === "Reserved" && bd && bd <= todayStr) {
      vals[r][idx["status"]] = "Pending";
      vals[r][idx["request_date"]] = todayStr;
      changed = true;
    }
  }
  if (changed) rng.setValues(vals);
}
```

**Save** (Ctrl/Cmd+S). You can test it now: pick `promoteReservations` in the function
dropdown → **Run**. (Approve the permission prompt the first time.) Any reservation whose
start date is today or earlier flips to `Pending`.

## 2. Install the daily trigger

In the Apps Script editor, left sidebar → **Triggers** (the alarm-clock icon) →
**+ Add Trigger**:

- Function to run: **promoteReservations**
- Deployment: **Head**
- Event source: **Time-driven**
- Type: **Day timer** → **Midnight to 1am** (or any early-morning hour)
- **Save**

Done. No web-app re-deploy needed — the `/exec` URL is unchanged, so `data.js` stays as is.

## How it fits the app
- A future-dated request is saved with status **Reserved** (no approval needed yet).
- On its start date, `promoteReservations` (or the admin opening the app) flips it to
  **Pending**, which appears in the admin **Requests** tab — the normal approve flow resumes.
- Overlapping reservations are blocked client-side at submit time.
