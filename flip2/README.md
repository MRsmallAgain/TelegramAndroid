# Flip 2 patch build — Forkgram 13.0.2.0

Unofficial patched build of [Forkgram](https://github.com/Forkgram/TelegramAndroid)
for the **TCL Flip 2 (4058G)** paired with the
[null-keyboard IME](https://github.com/MRsmallAgain/flip2). Not affiliated with
Telegram or the Forkgram maintainers; use at your own risk.

## What changed

The APK is produced from the F-Droid `org.forkgram.classic` **13.0.2.0**
(versionCode **717607**) binary, patched at the smali level. The source-level
equivalent of the main change is proposed upstream in
[PR #539](https://github.com/Forkgram/TelegramAndroid/pull/539).

1. **BACK deletes in text fields.** `LaunchActivity.onBackPressed()` deletes one
   character when the focused view is a non-empty `EditText` and consumes the
   event. Physical BACK and the navigation-bar BACK both deliver `KEYCODE_BACK`,
   so both are covered. An empty field still navigates normally.
2. **IME visibility relaxed.** `setShowSoftInputOnFocus(true)` is forced,
   `usingHardwareInput` is false, and `windowSoftInputMode` includes
   `stateVisible`, so the null-keyboard IME can attach and handle dictation.

### Why

On this device Telegram keeps hardware keys in the app window; the IME never
receives BACK. The app therefore has to handle BACK itself. The IME-side
workarounds cannot override that window behaviour.

## Install

```sh
adb install -r forkgram-13.0.2.0-flip2-patched.apk
```

The APK is signed with a local debug key, so it does not update over an F-Droid
install. Switching from F-Droid requires uninstalling first (app data is lost).
Keep the original APK for rollback: uninstall this build, then
`adb install -r forkgram-original-13.0.2.0.apk`.

## Reproduce

- **Base:** Forkgram 13.0.2.0 from F-Droid (versionCode 717607).
- **Smali patch:** `forkgram-13.0.2.0-back.patch`, applied with `patch -p1`
  after `apktool d -r`.
- **Helper:** `patch-forkgram.sh` (decode → patch → build → zipalign →
  debug-sign → install).
- **Source patch:** `0001-back-key-deletes-in-text-field.patch`.
- **Verify:** `sha256sum -c SHA256SUMS`.

## Checksums

    bc378c74c378c23c2115ca1755b5322b15d30132098bcc8136ef1f13a817ce38  forkgram-13.0.2.0-flip2-patched.apk

## License

Forkgram / Telegram for Android is GPL-2.0; this fork and the patches are
provided under the same terms. The APK bundles upstream code and third-party
libraries.
