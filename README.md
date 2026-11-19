# CircleFX Outreach

Read-only view of the CircleFX post-production outreach list: 1,281 producers
in 12 parts, with search and filters by group, part, role, email status and
tier, plus email templates for each group and outreach stage.

## What is in this repo

- `public/index.html` — the whole site in one static file. It has a VI/EN
  language toggle (Vietnamese is the default) and copy buttons for the
  email templates.
- `firebase.json` — Firebase Hosting config. The public folder is `public`,
  and there is no single-page-app rewrite.
- `.firebaserc` — Firebase project: `circlefx-outreach`.
- `.github/workflows/firebase-hosting-merge.yml` — deploys to Firebase
  Hosting automatically on every push to `main`.

## Live site

After the first deploy, the site is live at
https://circlefx-outreach.web.app
(and https://circlefx-outreach.firebaseapp.com).

The custom domain `circlefx.org` is not connected yet. That is a separate
step in the Firebase console: Hosting > Add custom domain.

## Deploy

Automatic: push to `main` and GitHub Actions deploys it. The workflow
needs a repository secret called `FIREBASE_SERVICE_ACCOUNT` first — see
below.

Manual, from a local checkout:

```bash
npm install -g firebase-tools
firebase login
firebase deploy --only hosting
```

## First-time setup for auto-deploy

The workflow will not run until the `FIREBASE_SERVICE_ACCOUNT` secret
exists. Get it one of these two ways:

1. In the Firebase console, open Project settings > Service accounts >
   Generate new private key, then add the whole JSON file contents as a
   GitHub repository secret named `FIREBASE_SERVICE_ACCOUNT`.
   (Or run the CLI below and paste the value it prints into the same
   secret.)

2. Or on your own computer, in this repo folder, run:

```bash
firebase init hosting:github
```

That command walks through the GitHub setup and creates the secret for
you. Do not paste any service-account key into chat, email or a file
in this repo.

## Working copy

This repo is the source of truth. If you also have a local folder on
Windows (for example `D:\____WebsiteDeploy\circlefx-outreach`), clone
this repo there or pull the latest `main` before editing, or your local
files will fall behind what is on GitHub.

## Notes

- The working, editable version of this app (with saved checklists and
  progress) is a separate app and is not in this repo. This repo holds
  the read-only view only.
- Contact details in the list are public contacts found in research.
  Most were not live-verified. Check each contact on the company site
  or LinkedIn before sending anything.
