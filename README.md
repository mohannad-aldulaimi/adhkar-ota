# Adhkar App - Over-The-Air (OTA) Updates Repository

This repository hosts the encrypted application bundles, direct APK downloads, and version metadata for the Adhkar Android application across both **Test** and **Real / Release** channels.

---

## 🚀 Supported Update Channels

| Channel | Metadata JSON | Encrypted Vault | Compiled APK | Package ID |
| :--- | :--- | :--- | :--- | :--- |
| **Real / Release (النسخة الرسمية)** | `version.json` | `app.bin` | `app-release.apk` | `com.adhkar.mohannad_alayash` |
| **Test / Beta (نسخة تجريبية)** | `version-test.json` | `app-test.bin` | `app-test.apk` | `com.adhkar.mohannad_alayash.test` |

---

## 🔒 How OTA Works
1. When the app launches, `VaultManager` checks the matching channel's metadata:
   - **Test channel**: `https://raw.githubusercontent.com/mohannad-aldulaimi/adhkar-ota/refs/heads/main/version-test.json`
   - **Real channel**: `https://raw.githubusercontent.com/mohannad-aldulaimi/adhkar-ota/refs/heads/main/version.json`
2. If `versionCode` is greater than the active local version, the new encrypted vault (`app-test.bin` or `app.bin`) is downloaded securely over HTTPS.
3. Integrity is verified using the SHA-256 hash.
4. `VaultManager` decrypts the vault directly into RAM using the AES-256 key protected by the native C++ module (`native-lib.so`).
5. Plaintext code and assets are never written to disk or device storage.
6. If a major native update is available (`latestApkVersionCode`), the user is prompted to download the matching APK directly (`app-test.apk` or `app-release.apk`).

---

## 📦 Pushing Updates to Cloud
To publish updates, use `push_ota.bat`:
- Option `1`: Push Test Updates Only (`version-test.json`, `app-test.bin`, `app-test.apk`)
- Option `2`: Push Real / Release Updates Only (`version.json`, `app.bin`, `app-release.apk`)
- Option `3`: Push All Updates (Test + Real)

