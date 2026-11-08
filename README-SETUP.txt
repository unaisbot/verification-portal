# TBARC Firebase Verification Portal

## Included
- `index.html`: public verification portal (no public login).
- `admin/index.html`: Firebase Email/Password admin login.
- `admin/dashboard.html`: add/edit/revoke records, list records, CSV import and QR generation.
- `firebase-config.js`: Firebase web app configuration.
- `firestore.rules`: recommended Firestore security rules.
- `data/TBARC-Certificates-Import.csv`: uploaded five-record CSV.

## Before deployment
1. Open `firebase-config.js` and replace `PASTE_YOUR_FIREBASE_API_KEY` and `PASTE_YOUR_STORAGE_BUCKET` with the exact values from Firebase Console > Project settings > General > Your apps > SDK setup.
2. Confirm the `projectId`, `authDomain`, `messagingSenderId`, and `appId` match your Firebase web app.
3. Publish `firestore.rules` in Firestore > Rules.
4. Confirm Firebase Authentication > Sign-in method > Email/Password is enabled.
5. Confirm your administrator Auth UID has a Firestore document at `admins/{UID}` with `role` string `admin`.
6. Upload the contents of this folder to the root of the GitHub Pages repository. Keep your existing `images/` folder (logo, favicon, hero image) if present.
7. Visit `/admin/`, sign in, select the supplied CSV and click Import CSV to Firebase.
8. Test public verification with `TBARC-MEM-001`.

## Collections
- `admins/{uid}`: administrator role document.
- `certificates/{certificateNumber}`: public verification fields.
- `documents/{documentNumber}`: public verification fields.

## Security notes
- Public visitors can `get` a known certificate/document ID, but cannot list collections.
- Admins can list and manage certificates/documents.
- Admins collection is not publicly readable; an authenticated user can read only their own role document.
- Keep only information suitable for public verification in `certificates` and `documents`.
- The current certificate numbers are sequential, so a determined visitor could guess IDs. Do not put sensitive/private fields in these public records. For stronger anti-enumeration, use a server-side verification endpoint or unguessable public verification tokens.
- Firestore public `get` is intentional for no-login verification.
