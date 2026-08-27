PDVoice Buddy
=============

A light that stays on while your voice is carrying.

For people with Parkinson's disease, whose voice quietens over time without
them noticing. PDVoice Buddy listens to the room, and shows a single glowing
ring in the corner of the screen: bright while the voice is carrying, dim
when it fades.

Version 0.1 — first field test
August 2026


-----------------------------------------------------------------------------
READ THIS FIRST
-----------------------------------------------------------------------------

Before you install anything, open:

    HOW TO USE - PDVoice Buddy.txt

It is two pages, written for the person who will actually use the app. It
covers starting it, the one-time room measurement, what the ring means, and
what to do when something looks wrong.

The room measurement matters most. The app does not work properly until it
has listened to five seconds of silence from the chair the person actually
sits in. Everything else is optional; that step is not.


-----------------------------------------------------------------------------
WHAT'S IN THIS FOLDER
-----------------------------------------------------------------------------

For the person using it
    HOW TO USE - PDVoice Buddy.txt      Read this first. Plain language.
    PDVoice Buddy.exe                   The app (once compiled).

For whoever sets it up
    README - START HERE.txt             Quick install, no compiling.
    COMPILE - STEP BY STEP.txt          Making a standalone .exe, step by
                                        step, assuming no prior experience.
    BUILD.md                            Fuller build notes, signing,
                                        notarization, calibration procedure.

The source
    python/pdvoice_buddy.py             The whole app, one file.
    python/requirements.txt             What it needs.
    python/tray.png                     The tray icon.
    python/PDVoice Buddy.bat            Double-click to run.
    python/Show errors.bat              Same, but shows errors if it fails.
    python/Install PDVoice Buddy.*      One-click setup for a fresh machine.

Design work
    PDVoice Buddy.dc.html               The interactive design prototype.
    Voice Cue Device.dc.html            Original concept: tabletop puck and
                                        desktop app compared.
    Voice Cue Desktop.dc.html           Earlier desktop exploration.

Alternative implementation
    native/                             The same app in Electron. Unused —
                                        kept for reference. The Python
                                        version is preferred: it reads the
                                        microphone directly, with no browser
                                        gain control distorting the levels.


-----------------------------------------------------------------------------
HOW IT WORKS
-----------------------------------------------------------------------------

Eight times a second the app measures the loudness of the room, keeps that
one number, and discards the audio. Nothing is recorded, stored, or
transmitted. There is no recording to find because none is ever made.

  1. Room measurement establishes a noise floor — what the room sounds like
     with nobody talking.
  2. The target is set above that floor, so a quiet room and a noisy one get
     different, appropriate targets.
  3. A hold timer keeps the ring lit through the natural gaps between words,
     so ordinary speech rhythm doesn't make it flicker.
  4. The ring's thickness and glow follow the level continuously, so you can
     see yourself approaching the target rather than only arriving at it.

The daily percentage — the share of speech that cleared the target — is kept
on the local machine only.


-----------------------------------------------------------------------------
IMPORTANT LIMITATIONS
-----------------------------------------------------------------------------

This is not a medical device. It must not be used to make clinical
decisions.

The decibel numbers are approximate. They are useful for comparing louder
against quieter in one fixed setting; they are not calibrated sound
pressure levels. Making them trustworthy requires calibration against a real
SPL meter at the seat the person uses — the procedure is in BUILD.md under
"Before anyone actually uses this."

Loudness falls roughly 6 dB every time the distance doubles. Move the laptop
and the calibration is void.

The app cannot tell voices apart. If someone else in the room is talking,
the ring may light for them. This is the known weakness most likely to show
up in real use.

Set the target level with a speech-language pathologist rather than guessing,
particularly for anyone already in speech therapy. Start where the person can
already succeed and raise it slowly. A light that stays dark all day teaches
people to ignore it.


-----------------------------------------------------------------------------
CREDITS
-----------------------------------------------------------------------------

Concept, direction, and field testing
    Cathi

Design and implementation
    Built with Claude (Anthropic)

Built with these open-source projects, with thanks
    Python              python.org — Python Software Foundation License
    PySide6 / Qt        Qt for Python — LGPL v3
    sounddevice         Matthias Geier — MIT License
    NumPy               numpy.org — BSD 3-Clause License
    PyInstaller         pyinstaller.org — GPL v2 with bootloader exception

The clinical rationale draws on the established finding that people with
Parkinson's systematically underestimate their own loudness — the reason a
cue has to come from outside rather than from self-monitoring. Intensity-
focused speech therapy (such as LSVT LOUD) is the standard approach; this
tool is intended to support that work between sessions, never to replace it.


-----------------------------------------------------------------------------
STATUS AND WHAT'S NEXT
-----------------------------------------------------------------------------

Version 0.1 is a first field test, not a finished product. The open question
is not whether a microphone can measure loudness — it plainly can — but
whether a glowing ring changes how someone speaks over the course of a real
day.

What we're watching for in this test:
  · Is the ring noticed while talking, or only when looked for?
  · Is it lit most of the time, or so rarely that it gets tuned out?
  · Does it survive a whole day, or get closed after twenty minutes?
  · Does it light up when other people in the room talk?

Under consideration afterwards:
  · Calibration against a real SPL meter, so the numbers mean something.
  · A tabletop puck — meals and visits are where projection matters most,
    and no laptop belongs at the dinner table.
  · An Android version, for the same reason.
  · A wrist tap instead of a light, which works when no screen is in view.


-----------------------------------------------------------------------------

Not a medical device. Provided as-is, with no warranty. Use at your own
discretion and in consultation with a qualified speech-language
pathologist.
