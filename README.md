# PDVoice Buddy

**A light that stays on while your voice is carrying.**

Parkinson's quietens the voice without the speaker noticing. You feel like
you're speaking normally; the room hears a whisper. PDVoice Buddy listens to
the room and shows one thing — a glowing dot. Bright means your voice is
carrying. Dim means speak up.

That's the whole app. No exercises, no scores, nothing to read mid-sentence.

**Android and Windows. Free. English and Spanish.**

> **This is a prototype in beta testing, not a medical device.** It is good at
> telling louder from quieter in one place. Its dB readings are approximations
> from an uncalibrated microphone, not measurements, and no decision about
> treatment should rest on them. Set the level with a speech-language
> pathologist rather than guessing.

---

## If you are here to use it

**Read [`HOW TO USE - PDVoice Buddy (All Versions).txt`](HOW%20TO%20USE%20-%20PDVoice%20Buddy%20%28All%20Versions%29.txt)
before you install anything.** It is written in plain language and it covers
the two things people get wrong: where to put the device, and which
sensitivity step to start on.

Then get the latest build from the [**Releases**](../../releases) page:

| You have | Download | Notes |
|---|---|---|
| Android phone or tablet | the `.apk` | Android will warn you three times. All three are expected — the guide walks through them. |
| Windows PC | `PDVoice Buddy.exe` | No installer. Save it somewhere and double-click. Windows will warn you once. |

There is no iPhone, iPad or Mac version.

### Updating

The app checks once a day and shows a banner when a new version exists.

- **Windows** downloads, replaces itself and restarts. One button.
- **Android** hands the file to your browser; you tap to install it, same as
  the first time. Android does not permit anything more automatic outside the
  Play Store.

Your settings are kept either way.

---

## How it works

The dot needs your voice to clear **two bars at once**:

1. **Above the room**, by a margin. Being audible is relative — the voice that
   carries in a kitchen is lost in a restaurant, and no fixed number can
   express that. The app tracks the background level continuously and adapts
   within a couple of seconds.

2. **Above an absolute level**, roughly 60–70 dB. Because the goal is not
   merely to be audible: it is to practise projecting at a normal
   conversational volume. In a silent room the relative test alone would light
   up for a whisper, teaching exactly the wrong habit.

Whichever bar is higher governs. The room decides in a restaurant; the
absolute level decides in a quiet living room.

The second bar exists because a Parkinson's physician's assistant pointed out
that the first one alone was rewarding whispering. That correction is the most
useful piece of feedback the project has had.

### Privacy

**Nothing is recorded, ever.** Audio becomes a single loudness number about
twenty times a second and is discarded immediately. Nothing is stored and
nothing is transmitted. The only network request the app ever makes is reading
[`latest.json`](latest.json) to see whether a newer version exists.

---

## Repository layout

```
android/                  Android app (Kotlin, no dependencies but JUnit)
  app/src/main/java/com/pdvoicebuddy/
    VoiceCue.kt           The measurement. The only file worth arguing about.
    MeterService.kt       Microphone loop, foreground service
    RingView.kt           The full-screen dot
    PillView.kt           The floating pill
    OverlayService.kt     Hosts the pill over other apps
    SettingsActivity.kt   Settings, built for hands that shake
    UpdateCheck.kt        Reads latest.json
    Prefs.kt              Stored settings and the five sensitivity steps
  app/src/test/           VoiceCueTest.kt — runs on a PC, no device needed

python/                   Windows and Mac app (PySide6)
  pdvoice_buddy.py        The whole app
  updates.py              Reads latest.json, replaces the .exe
  build-exe.bat           Double-click to build

latest.json               The update manifest both apps read
```

`VoiceCue.kt` and the `VoiceCue` class in `pdvoice_buddy.py` are a deliberate
line-for-line port of each other. **If you change one, change the other**, or
the two platforms will quietly start behaving differently.

### Building

**Android** — open the `android` folder in Android Studio, press Run. For a
release build see `SIGNED APK - STEP BY STEP.txt`.

**Windows** — double-click `python/build-exe.bat`. It makes its own virtual
environment. Step-by-step instructions are in `COMPILE - STEP BY STEP.txt`.

**Tests** — right-click `VoiceCueTest.kt` in Android Studio and Run. No tablet
required. Run them before every release; they encode behaviour that was got
wrong at least once.

### Publishing a release

See [`GITHUB - STEP BY STEP.txt`](GITHUB%20-%20STEP%20BY%20STEP.txt). The short
version: upload the built files to a new Release, then edit `latest.json` to
match. The apps notice within a day.

---

## Documentation

| File | For |
|---|---|
| `HOW TO USE - PDVoice Buddy (All Versions).txt` | The person using it. Start here. |
| `CHANGELOG.txt` | What changed, and which decisions were reversed and why |
| `FEEDBACK SURVEY - PDVoice Buddy (Tablet).txt` | Testers, after a week |
| `SPANISH - PROOFREADING SHEET.txt` | A fluent Spanish speaker reviewing the translation |
| `GITHUB - STEP BY STEP.txt` | Cutting a release |
| `SIGNED APK - STEP BY STEP.txt` | Building a shareable Android build |
| `COMPILE - STEP BY STEP.txt` | Building the Windows .exe |
| `WEBSITE COPY - PDVoice Buddy.txt` | Describing it publicly |

---

## Help wanted

Most useful first:

1. **Testing in genuinely noisy rooms.** The adaptive half of the model is the
   app's central claim and it has never been properly challenged. A quiet
   kitchen proves nothing.
2. **Automatic driving detection.** Car mode is currently manual — the user
   must select it every launch. A dot pulsing in a driver's eyeline is unsafe
   and illegal in many places, so this should not depend on anyone
   remembering. Bluetooth connection or activity recognition would do it.
3. **Spanish proofreading** by a native Latin American speaker. See the
   proofreading sheet; the translation is unreviewed.
4. **An Apple version.** Attempted and abandoned for want of a Mac and
   patience. The Python app should be close to running already.
5. **Progress history that survives restarts.** Deliberately removed rather
   than shipped broken. If it returns it must be encouraging, not a score.

## Standing principles

Each was arrived at by getting it wrong first. Please keep them.

- **Nothing is recorded, ever.** No exceptions, no debug mode that saves audio.
- **The dot is for the corner of the eye.** Anything that makes someone look
  directly at it — text, numbers, animation for its own sake — is a
  regression.
- **Subtract before adding.** No scores, no streaks, no notifications about
  the voice. The tool most likely to still be in use next month is the one
  that asks for the least.
- **Do not overclaim.** "Prototype", "free" and "not a medical device" stay on
  every page. Overselling a homemade tool to people managing a degenerative
  illness would be a genuine wrong, not merely bad marketing.
- **The person is not a patient.** They are someone who wants to be heard at
  dinner.

## Credits

Built for a family member with Parkinson's, and shaped by her use of it.

Threshold design corrected by a Parkinson's physician's assistant, whose
observation that a quiet room let a whisper through changed the core of the
measurement.

## Licence

[MIT](LICENSE) — use it, change it, give it to anyone who needs it. If you
make it better, a note to say so would be welcome.
