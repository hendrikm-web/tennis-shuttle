# Tennis-Shuttle

## Firebase access setup

The app uses Firebase Authentication (email/password) and the Realtime Database. Enable **Email/Password** in Firebase Authentication before using the app.
Host the app on an authorized web domain configured in Firebase Authentication; the `file://` preview is not a production deployment.

Database access is denied by default in the included rules. They do not protect the live database until deployed. Deploy them with the Firebase CLI:

```powershell
firebase deploy --only database --project tennis-shuttle
```

The app currently uses the group ID `main` and stores its shared state at `/tennis_shuttle/groups/main/main_state`. To approve a user, create the account in the app, then add a boolean `true` membership at:

`/tennis_shuttle/groups/main/members/{Firebase Auth UID}`

Only a trusted administrator should manage membership records. The rules permit each signed-in user to read their own membership flag, but do not permit clients to create or change memberships. Group members can read and write the shared group state.

## Existing data

The previous app stored data at `/tennis_shuttle/main_state` and did not require authentication. If that data must be retained:

1. Export a JSON backup from the previous app.
2. Deploy the new rules. This disables access to the old, unauthenticated path.
3. Create the first account, then add its membership record through the Firebase Console (trusted admins can write membership records).
4. Sign in and import the backup through **Einstellungen & Backup**.
5. Create and approve any other accounts.

The app does not automatically migrate old local or cloud data.
