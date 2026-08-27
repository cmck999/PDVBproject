# Building PDVoice Buddy

Two complete implementations are in this project. Both do the same thing; pick one.

| | `python/` (PySide6) | `native/` (Electron) |
|---|---|---|
| Language | Python | JavaScript |
| Run it in | ~2 min | ~3 min |
| App size | ~60 MB packaged | ~180 MB packaged |
| Audio | `sounddevice` — real device access, no browser gain control | Web Audio — the browser may still apply gain control |
| Best if | You want to read and change the DSP | You want the UI to match the prototype exactly |

**Recommendation: use the Python one.** For this app the audio path matters more than the UI polish, and `sounddevice` gives you the raw microphone with no automatic gain control fighting your measurements. Automatic gain control is the single thing most likely to make the readings meaningless.

---

## Python version

### Run it

```bash
cd python
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python pdvoice_buddy.py
```

The flag appears in the lower-right corner, already listening. Drag it anywhere. The tray icon holds Show/Hide, Pause, Open at login, and Quit.

On first run macOS asks for microphone access once. If you miss the prompt: System Settings → Privacy & Security → Microphone.

### Compile to a real app

```bash
pip install pyinstaller
```

**macOS**

```bash
pyinstaller --windowed --name "PDVoice Buddy" \
  --osx-bundle-identifier com.pdvoicebuddy.app \
  pdvoice_buddy.py
```

Then add the microphone usage string, or macOS kills the app the moment it opens the mic. Open `dist/PDVoice Buddy.app/Contents/Info.plist` and add:

```xml
<key>NSMicrophoneUsageDescription</key>
<string>PDVoice Buddy measures how loudly you are speaking so it can show you a light. Audio is never recorded or sent anywhere.</string>
<key>LSUIElement</key>
<true/>
```

`LSUIElement` keeps it out of the Dock — it lives in the menu bar only.

Ad-hoc sign it so Gatekeeper lets it run on your own machines:

```bash
codesign --force --deep --sign - "dist/PDVoice Buddy.app"
```

To hand it to someone else you need a paid Apple Developer account, a Developer ID certificate, and notarization:

```bash
codesign --force --deep --options runtime \
  --sign "Developer ID Application: YOUR NAME (TEAMID)" \
  "dist/PDVoice Buddy.app"
xcrun notarytool submit "PDVoice Buddy.zip" --apple-id you@example.com \
  --team-id TEAMID --password APP_SPECIFIC_PASSWORD --wait
xcrun stapler staple "dist/PDVoice Buddy.app"
```

**Windows**

```bash
pyinstaller --noconsole --name "PDVoice Buddy" pdvoice_buddy.py
```

Produces `dist/PDVoice Buddy/PDVoice Buddy.exe`. Unsigned executables trigger a SmartScreen warning; a code-signing certificate (~$100–300/yr) removes it. For an installer, point [Inno Setup](https://jrsoftware.org/isinfo.php) at the `dist` folder.

**Linux**

```bash
pyinstaller --name pdvoice-buddy pdvoice_buddy.py
```

---

## Electron version

```bash
cd native
npm install
npm start
```

To package:

```bash
npm run dist          # current platform
npm run dist:mac      # .dmg + .zip
npm run dist:win      # NSIS installer
```

`electron-builder` reads the `build` block in `package.json`, which already contains the macOS microphone usage string and app id. Output lands in `native/dist/`.

Signing works the same way as above; `electron-builder` will pick up a Developer ID certificate from your keychain automatically, and notarizes when you set `APPLE_ID`, `APPLE_APP_SPECIFIC_PASSWORD`, and `APPLE_TEAM_ID` in the environment.

Note: `assets/icon.png` and `assets/tray.png` are referenced but not included — drop in a 512×512 and an 18×18 PNG, or delete the `icon` lines to build with defaults.

---

## Before anyone actually uses this

**Calibrate against a real SPL meter.** Both versions ship `DB_OFFSET = 94.0` (Python) / `state.calibOffset` (Electron), a guess that converts the microphone's internal scale to something dB-like. It is not a real sound level. To fix it:

1. Get an SPL meter (a $25 handheld, or a calibrated phone app) and put it beside the laptop, at the seat the person actually uses.
2. Play a steady tone or talk continuously. Read both numbers.
3. Adjust the offset until the app agrees with the meter. In Python that is `DB_OFFSET` at the top of the file.
4. Repeat at that same seat and distance. Move the laptop and the calibration is void — loudness falls about 6 dB every time the distance doubles.

**Turn off automatic gain control.** The Python version bypasses the browser entirely, which is most of the battle. On Windows, also check Sound Settings → your microphone → disable "Automatic gain control" or any enhancement.

**Set the threshold with the speech-language pathologist,** not by guessing. Start at a level the person can already reach, and raise it over weeks. A light that stays dark all day teaches people to ignore it.

This is not a medical device and should not be used to make clinical decisions.
