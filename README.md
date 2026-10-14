# Workshop Desk — CRM copy

This is a separate Workshop Desk installation for `dewinsusedcars/CRM`. The browser app is served from `docs/` at <https://dewinsusedcars.github.io/CRM/>. Its records belong in [this Google Sheet](https://docs.google.com/spreadsheets/d/1a8iv0DDf43OzapWHq3QVR5t9jzSsCHrykgUVSVNLCPA/edit). It does not use the original workshop's Sheet or Apps Script deployment.

The app includes employer and employee accounts, job cards, customers, attendance approvals, inventory, invoices, estimates, printing, WhatsApp updates, and a vehicle catalog. The fresh setup starts with one example workflow. Catalog spare parts start at quantity and price zero.

## New Sheet setup

1. From the Sheet, open **Extensions → Apps Script**. Replace `Code.gs` with [apps-script/Code.gs](apps-script/Code.gs), and create an HTML file named `Index` using [apps-script/Index.html](apps-script/Index.html).
2. In Apps Script **Project Settings → Script properties**, set `ADMIN_INITIAL_PASSWORD` to the chosen new admin password. Do not commit the password. Run `initializeWorkshop` once as the Sheet owner. It creates only the `WorkshopDesk_Data` tab, refuses to overwrite an existing one, and removes the initial password property after setup.
3. Deploy a **web app** that executes as the Sheet owner and is accessible to anyone who needs the login page. Copy the `/exec` URL. Records still require a valid workshop login.
4. Put that URL in `work/design/pwa-bridge.js`, run the build and tests below, and publish `docs/` with GitHub Pages. Keep the Sheet and Apps Script project private.

```sh
node work/design/build-sheets.cjs
node work/design/test-sheets.cjs
```

The initial admin username is `admin`; the password is the value set in Script properties. Employee usernames are their phone numbers. Sessions expire within six hours, and the connected app keeps the current page through a refresh while a session is valid. Changes from other devices refresh about every eight seconds. The app needs an internet connection and does not queue offline edits.

The Apps Script bridge accepts the `dewinsusedcars.github.io` origin only. Changing the repo owner or hosting domain requires updating that origin in `work/design/server.gs`, rebuilding, and deploying a new Apps Script version.

No license has been selected. All rights remain with the repository owner.
