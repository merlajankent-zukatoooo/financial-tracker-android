# Financial Tracker Android APK — Firebase live website version

This is the complete Android project, not the finished APK. It creates an installable Android APK containing a WebView that opens **https://finapp-ffbfa.web.app/**. The website's Firebase Authentication and Firestore remain the source of data. The Android package contains no Firebase password, service-account key, or financial records.

## Make the APK from GitHub on a Windows PC

1. Download and **extract** this ZIP file. You must see `settings.gradle`, `build.gradle`, `app/`, and `.github/` at the top level.
2. On GitHub, open your Android repository, e.g. `financial-tracker-android` (this is a separate repository from your Firebase website).
3. Upload the **contents** of the extracted folder into the **root** of the repository. **Do not upload only `build-apk.yml`, and do not upload the ZIP file itself.** Ensure `settings.gradle` is visible directly in the repository's **Code** tab, alongside `app/`.
4. If your repository contains older broken workflow files such as `.github/workflows/main.yml`, `.github/workflows/finapp.yml`, or `android-apk.yml`, delete those older files. Keep the new `.github/workflows/build-apk.yml`.
5. Open **Actions → Build Financial Tracker APK**. Select **Run workflow**, choose the `main` branch, and start it. A new upload to the main branch may also run it automatically.
6. When the workflow finishes with a green tick, open the successful run. Scroll to **Artifacts** and download **Financial-Tracker-APK**.
7. Extract the artifact ZIP to get `app-debug.apk`, then transfer it to your Android phone. Open the APK from a trusted file manager and approve installation if prompted.
8. Open **Financial Tracker** and sign in with your existing Firebase email/password.

## Updating the tracker later

The APK opens the **live Firebase website**. When you update your HTML and deploy it to Firebase Hosting, the installed Android app loads the updated website. You do **not** need to rebuild the APK for ordinary website updates. Reload the app when needed. The APK only needs rebuilding for changes to its native Android shell, permissions, name or icon.

To deploy the website, from the folder containing your website's `firebase.json` run:

```powershell
firebase.cmd deploy --only hosting --project finapp-ffbfa
```

This does not deploy the Android project. The Android project should be stored in a separate folder/repository from your Firebase Hosting website.

## Firebase and security

- You must deploy the revised Compact Financial Tracker HTML to Firebase Hosting first. The APK loads whatever is currently live at the website address.
- Your existing Firebase email/password login and Firestore rules remain unchanged. The APK does not independently store or synchronize your financial records.
- The Android WebView has its own browser storage. Sign in again inside the APK. A signed-in account sees the same cloud records provided Firestore synchronization is functioning.
- Access requires the internet for first load and login. Offline access is not guaranteed.
- The app supports JSON import using the Android file picker and a save-document bridge for export, subject to verification on a real device.
- Do not upload your private JSON backups, Firebase service-account keys, or financial records to GitHub.
- Debug builds are for private testing and are signed with the SDK debug key. For distribution or predictable APK upgrades, use a durable release signing key.
- No APK was compiled here, because the Android SDK and Gradle build environment are not installed. GitHub Actions or Android Studio will perform the compile; confirm a successful green build and test on your device.

## Troubleshooting

**Build says `does not contain a Gradle build`:** `settings.gradle` and `app/` are not at the root of your GitHub repo. Move them out of any extra nested folder and rerun.

**Android SDK licenses not accepted:** Make sure the workflow at `.github/workflows/build-apk.yml` uses `android-actions/setup-android@v4` with `accept-android-sdk-licenses: 'true'`.

**APK shows Firebase Hosting Setup Complete:** The live website is serving the Firebase welcome page. Deploy your Financial Tracker website to Firebase Hosting before using the APK.

**App signs in but records do not match PC:** Confirm Firestore rules and live sync in the website before troubleshooting the Android app.

**The APK does not install over an older version:** A different signing certificate might have been used. Export backups and confirm cloud sync before uninstalling the old version.

Official references:
- https://docs.github.com/en/actions/how-tos/manage-workflow-runs/manually-run-a-workflow
- https://developer.android.com/build/releases/agp-8-7-0-release-notes
- https://github.com/android-actions/setup-android
