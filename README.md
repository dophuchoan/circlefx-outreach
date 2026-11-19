# CircleFX Outreach

Outreach list for the CircleFX post-production company: 1,281 producers
in 12 parts, with search and filters by group, part, role, email status and
tier, plus email templates for each group and outreach stage. The site has
a VI/EN language toggle (Vietnamese is the default) and copy buttons for
the email templates.

This version has a real backend: Firebase Authentication (email and
password) plus Cloud Firestore. The producer list is **not** in the HTML
file anymore — it lives in Firestore, collection `producers`, one document
per producer, doc id `part-number` (for example `1-001`). Only signed-in
users can read or write it.

## What is in this repo

- `public/index.html` — the whole site in one static file. It signs in with
  Firebase Auth, then loads the producers from Firestore. It has no
  producer data embedded in it.
- `public/producers-data.js` — one-time migration data: the 1,281 producers
  as `window.PRODUCERS_DATA`. It is loaded on demand only when a signed-in
  user clicks the Import button. **After the import succeeds, delete this
  file from `public/` and redeploy** — while it is hosted, anyone can
  download the full list from it, which defeats the point of moving the
  data to Firestore.
- `firestore.rules` — Firestore security rules: only signed-in users
  (`request.auth != null`) can read or write `producers`. Everything else
  is denied.
- `firebase.json` — Firebase Hosting config (public folder is `public`, no
  single-page-app rewrite) and the Firestore rules file.
- `.firebaserc` — Firebase project: `circlefx-outreach`.
- `.github/workflows/firebase-hosting-merge.yml` — deploys to Firebase
  Hosting automatically on every push to `main`.
- `.github/workflows/firebase-hosting-pull-request.yml` — preview deploys
  for pull requests.

## Live site

https://circlefx-outreach.web.app
(and https://circlefx-outreach.firebaseapp.com).

The custom domain `circlefx.org` is not connected yet. That is a separate
step in the Firebase console: Hosting > Add custom domain.

## Accounts — admin creates them, nobody self-registers

There is no sign-up button on the site. The admin creates every account in
the Firebase console: project `circlefx-outreach` > Authentication >
Users > Add user (email + password). To remove someone's access, delete
their user there. All signed-in users can currently read, add, edit and
delete producers; an admin-only distinction (custom claim) is a later step
and is not in this version.

## First-time setup (do these once, in this order)

1. Create the first account in the Firebase console (Authentication >
   Users > Add user). Nobody can sign in until at least one account
   exists.
2. Deploy the Firestore rules from a local checkout of this repo:

   ```bash
   npm install -g firebase-tools
   firebase login
   firebase deploy --only firestore:rules
   ```

   The Firestore database itself was created in `asia-southeast1`
   (Singapore), Production mode. Its starting rules denied everything;
   the command above replaces them with the signed-in-users-only rules
   in `firestore.rules`.
3. Deploy the site (automatic on push to `main`, or manually with
   `firebase deploy --only hosting`).
4. Sign in on the live site with the account from step 1. The list will
   be empty at first. Click **“Import 1281 producers to Firestore”**
   once. It writes the 1,281 producers in batches of 400–500 and then
   hides itself. Do not click it twice — it only shows while the
   `producers` collection is empty.
5. Delete `public/producers-data.js`, commit, and push again so the
   migration file is no longer hosted (see the warning above).

## Editing producers in the app

After signing in, anyone can:

- Add a producer with the Add form (name is required; the rest is
  optional).
- Edit any producer from its card — the form opens prefilled with that
  producer's current details.
- Delete a producer from its card — the app asks for confirmation first.

Changes go straight to Firestore and the list refreshes without a page
reload. New producers get the next free number in their part; their doc
id is `part-number` like the imported ones.

## Deploy

Automatic: push to `main` and GitHub Actions deploys it. The workflow
needs a repository secret called `FIREBASE_SERVICE_ACCOUNT_CIRCLEFX_OUTREACH`
— it was created by `firebase init hosting:github` (service account
`github-action-1463681274`). If it ever goes missing, re-run that command
in a local checkout, or create it from Project settings > Service
accounts > Generate new private key and add the whole JSON as a GitHub
repository secret. Do not paste any service-account key into chat, email
or a file in this repo.

Manual, from a local checkout:

```bash
firebase deploy --only hosting
firebase deploy --only firestore:rules
```

## Working copy

This repo is the source of truth. If you also have a local folder on
Windows (for example `D:\____WebsiteDeploy\circlefx-outreach`), clone
this repo there or pull the latest `main` before editing, or your local
files will fall behind what is on GitHub.

## Notes

- The working, editable version of this app (with saved checklists and
  progress) is a separate app and is not in this repo.
- Contact details in the list are public contacts found in research.
  Most were not live-verified. Check each contact on the company site
  or LinkedIn before sending anything.
- Firestore is billed per read/write; loading the full list once per
  sign-in is 1,281 document reads. That is fine at this size, but do not
  add a live listener that re-reads the whole collection on every edit.
