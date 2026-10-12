# ShipShield iOS — No-Mac Build via GitHub Actions

This builds and uploads your iOS app to App Store Connect using GitHub's free
macOS runners. You never need a Mac. The workflow signs with an **App Store
Connect API key** (cloud signing), so there are no certificates to juggle.

The `export` step uploads the build straight to App Store Connect — the final
**Submit for Review** button in App Store Connect stays with you.

---

## One-time setup (clickable)

### 1. Get the PWABuilder iOS project into a GitHub repo
1. On [PWABuilder](https://www.pwabuilder.com), generate the **iOS** package for `shipshield.io`
   with Bundle ID `com.cloudonesoftware.shipshield`, and download the zip.
2. Create a new **private** repo: [github.com/new](https://github.com/new) → name it `shipshield-ios`.
3. Unzip the PWABuilder download. Put the Xcode project folder under `src/` so the
   path is `src/ShipShield.xcodeproj` (or edit `PROJECT_PATH` in the workflow to match
   the real path/name — open the zip to see what PWABuilder called it).
4. Copy `.github/workflows/ios-build.yml` (from this kit) into the repo at the same path.
5. Commit and push to `main`.

### 2. Create an App Store Connect API key
1. Open [App Store Connect → Users and Access → Integrations → App Store Connect API](https://appstoreconnect.apple.com/access/integrations/api)
2. Click **＋** to generate a key → Name `GitHub CI` → Access **App Manager** → **Generate**
3. **Download the `.p8` file** (you can only download it once — keep it safe)
4. Note the **Key ID** (next to the key) and the **Issuer ID** (shown at the top of the page)

### 3. Find your Team ID
- [developer.apple.com/account](https://developer.apple.com/account) → **Membership details** → **Team ID** (10 characters)

### 4. Add the secrets to GitHub
In your repo → **Settings → Secrets and variables → Actions → New repository secret**.
Add these four:

| Secret name | Value |
|---|---|
| `ASC_API_KEY_P8` | the `.p8` file contents, **base64-encoded** (see below) |
| `ASC_KEY_ID` | the Key ID from step 2 |
| `ASC_ISSUER_ID` | the Issuer ID from step 2 |
| `APPLE_TEAM_ID` | your 10-char Team ID from step 3 |

**To base64-encode the `.p8`** (so it pastes as one line):
- On any machine with a browser, you can use an online base64 encoder, OR
- the repo's own Actions can't do it for you — easiest is: open the `.p8` in a text editor,
  and if you have access to *any* terminal (even the GitHub Codespaces terminal in the browser):
  `base64 -i AuthKey_XXXX.p8` and paste the output.
- If you truly have no terminal, tell me and I'll give you a tiny browser page that encodes it locally.

### 5. Run it
- Repo → **Actions** tab → **Build & upload ShipShield iOS** → **Run workflow** → **main** → **Run**.
- ~5–10 min later the build appears in **App Store Connect → your app → TestFlight / Build**.

---

## After the build appears in App Store Connect
1. Open your **ShipShield** app → the version page.
2. Select the uploaded **Build**.
3. Paste the listing text + screenshots from the kits I gave you.
4. Add the **App Review notes** (the OAuth/no-password explanation).
5. **Submit for Review**.

## Notes
- Edit `SCHEME` and `PROJECT_PATH` in the workflow to match whatever PWABuilder named the
  project — open the downloaded zip; the folder ending in `.xcodeproj` is the project, and
  the scheme usually matches the app name.
- `macos-15` runners include recent Xcode. If a build complains about Xcode version, change
  the `Select latest Xcode` step or the runner label.
- First run may fail if the Bundle ID isn't registered or the app record doesn't exist yet —
  make sure you created the App ID (`com.cloudonesoftware.shipshield`) and the App Store
  Connect app record first.
