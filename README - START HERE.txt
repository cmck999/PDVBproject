PDVoice Buddy
=============

A light that stays on while your voice is carrying.


INSTALL
-------

Mac:      open the "python" folder, double-click  Install PDVoice Buddy.command
Windows:  open the "python" folder, double-click  Install PDVoice Buddy.bat

That's it. The installer sets everything up, puts PDVoice Buddy on your
Desktop, and starts it.

First time only, your computer will ask for microphone permission. Say yes.
Nothing is ever recorded — the app measures how loud you are, eight times a
second, and throws the sound away.


MAC: "cannot be opened because it is from an unidentified developer"
--------------------------------------------------------------------

Right-click the installer instead of double-clicking it, choose Open, then
click Open in the dialog. You only have to do this once.


USING IT
--------

The flag sits in the bottom-right corner of your screen. Drag it anywhere.

  Ring lit and green ....... your voice is carrying
  Ring dim ................. speak up a little
  The round button ......... pause / start listening
  "Settings" row ........... threshold and room measurement

FIRST THING TO DO: open Settings and press "Measure the room". Sit where you
normally sit, stay quiet for five seconds. This teaches it what your room
sounds like with nobody talking, and sets a sensible target.


IMPORTANT, BEFORE YOU TRUST THE NUMBERS
---------------------------------------

The dB numbers are approximate. They are not real sound levels until you
calibrate against an actual sound meter — see BUILD.md, "Before anyone
actually uses this".

The light is still useful before then: it will tell you consistently whether
you are louder or quieter than you were a minute ago, which is the part that
changes how people speak.

Set the target level with a speech-language pathologist. Start somewhere you
can already reach, and raise it slowly.

This is not a medical device and must not be used for clinical decisions.


IF YOU'D RATHER JUST TRY IT IN A BROWSER
----------------------------------------

Open "PDVoice Buddy.dc.html" in a browser tab (not inside a preview pane),
click the app icon, and allow the microphone. Same engine, no install.
