# Security — required Firebase actions

The app code has been fixed (Google auth + XSS protection), but **two settings must be done manually in the Firebase console** for the security to actually take effect.

## 1. Publish the database rules ⚠️ CRITICAL

Without this, the database stays readable/writable by anyone on the internet.

1. Firebase Console → project **vegain-marketing** → **Realtime Database** → **Rules** tab.
2. Replace the content with [`database.rules.json`](database.rules.json).
3. Click **Publish**.

These rules allow read/write only to verified Google accounts on the `@vegain-th.com` domain.

## 2. Enable the Google provider

1. Firebase Console → **Authentication** → **Sign-in method**.
2. Enable **Google**.
3. Under **Authentication → Settings → Authorized domains**, make sure `tichakornvg-cloud.github.io` is present (add it if missing), otherwise the sign-in popup will be blocked.

## Verify

- Open the app → the login screen should appear.
- Sign in with a `@vegain-th.com` account → access granted.
- While signed out, run:
  ```bash
  curl "https://vegain-marketing-default-rtdb.asia-southeast1.firebasedatabase.app/entries.json"
  ```
  Once the rules are published, this must return `Permission denied`.
