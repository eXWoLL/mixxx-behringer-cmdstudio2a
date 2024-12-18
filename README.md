# Behringer CMD Studio 2a controller, version 1.1.0 (Aug 2023)

Updated controller mappings for Mixxx for the Behringer CMD Studio 2a controller.

Placeholder repo until I update it for merge to upstream.

## New in 1.1.0 (Dec 2024)

- QuickEffects Super Knob (Filter Knob) is controlled via MODE/SHIFT key + Pitch Knob
- Changed the jog wheel sensitivity x10 increase when the MODE key is pressed to apply to CD-mode as well ( I didn't saw why it wasnt done and seems as useful for moving around :))
- Modified the VINYL-mode, adding some FX functionality to it (didnt thought of any other button to do this lol) When it is active, the following happens:

The sample buttons (1-4) of the decks (overriding all other instructions for them) control the effects panel (Deck A for the FX1 panel, and Deck B for the FX2 panel) as follows:

a)Sample buttons 1-3 :

    When pressed once: Toggles FX 1-3 ON/OFF
    While held down: Allows the pitch knob to control the meta knob of each FX (1-3); and the UP/DOWN keys to change the controlled effect
    If pressed twice: resets the FX knob to the original position

b) Sample Button 4:

    When held down: Allows the pitch knob to control the mix knob of the FX panel
    When pressed twice: resets the FX mix knob to its default position
    When pressed while the MODE key is active: Resets the Quick Effect Super knob of the deck to the Default effect (Filter)

c) The pitch blend buttons (+ and -) scroll the selected effect of the QuickEffect Super knob up and down respectively.

# Control scheme

Based off https://github.com/mixxxdj/mixxx/wiki/behringer_cmd_studio_2a, refer to those docs for a starter.

## Additional changes

### Preferences File

Preferences have been split into a separate file `Behringer-CMDStudio2a-Enhanced-preferences.js`. If you take a copy of this file and install it to `~/.mixxx/controllers` then any edits you make will persist through version upgrades. This avoids you needing to edit the file every time the controller defintion is updated.

As new options are added you'll have sensible defaults, but if you want to change an option added since you copied the file, you'll either need to copy the option in yourself, or take a fresh copy and make your changes by hand again.

### Shift Mode configuration

The MODE button has changed from the orginal mapping to enable a more traditional "shift is only active while you hold the MODE key down" behaviour.

If you wish it to return to the old default where the MODE button uses "press once to enable mode-shift for the next command, or press twice to enable mode-shift lock", you can set `holdToModeShift` value in the preferences file to `true`.

### Jog Wheel sensitivity

In vinyl mode the top of the jog wheels enable scratching, and when MODE SHIFTED they'll function at 10x sensitivity to let you scrub through a track quickly.

You can also edit the jog wheel sensitivity in the preferences file if you need to.

### Headphone Volume and Headphone Mix

The headphone knob controls headphone volume/gain by default, and when mode-shifted controls the headphone mix. If you'd prefer to swap these behaviours then change the `headGainIsDefault` value to `false` in the preferences file.

### Master Volume/Gain

While mode-shifted, the Deck A high FX knob controls the master volume/gain.

### Deck Pre-fader Gain

While mode-shifted, the mid FX knob controls the gain applied to the deck before EQ or fader is applied. (Use to avoid clipping.)

### Vinyl Mode

By default the decks now start in vinyl mode, if you'd rather start in CD mode like the original controller mapping you can change the `startInVinylMode` option in the preferences file to `false`.

The *Vinyl* button will toggle between vinyl mode on/off, unless you're mode-shifting, in which case it toggles recording on/off.

### Recording

With mode-shift or mode-lock on, pressing the *Vinyl* button will toggle recording mode.

This keybind doesn't make a whole lotta logical sense, but I wanted to have recording toggleable from just the controller and this was the least-confusing place to put it.

### Sync Button.

With mode-shift or mode-lock on, pressing the *Sync* button will disable sync lock and reset the deck playback rate to default.

### Deck Cloning

Load A/Load B work as normal without the mode-shift key active. If you have mode-shift or mode-lock enabled then Load A/Load B will clone the playing track into deck A or B. (Deck cloning loads the cloned deck and sets it playing from the same point/bpm/etc.)

### Load To Inactive Deck

Pressing the File button with a track selected will load it into the first available stopped deck.

### Track Previewing

Pressing the File button while mode-shift is active will start previewing the currently selected track, or stop the current preview from playing.

You can stop the preview deck from automatically opening and closing as you do this by setting `autoOpenPreviewDeck` to `false` in the preferences file.

### Beat Jumping

With mode-shift or mode-lock active the plus/minus buttons will beatjump forwards or backwards by the beatjump amount. This takes priority over other uses of the plus/minus buttons.

### Assign A button

The *Assign A* button is now an "edit mode" switch that cycles between three editing modes:

* First is "sample" editing mode, with red LED.
* Second is "loop" editing mode with blue LED.
* This is "intro-outro" editing mode with flashing red/blue LED.

If you want to change the order of the "edit modes", you can do this in the `editModes` section of the preferences file. You can also customize the `startInEditMode` to pick which edit mode to start in when you fire up Mixxx.

#### Sample editing mode.

* The hotcue buttons give access to hotcues 1-4.
* The sample buttons give access to sample banks 1-4.

This is the default behaviour from the old controller mapping.

#### Loop editing mode

* Assign B toggles loop enabled. (This applies to all modes, not just loop editing mode.)
* Plus/minus control loop in/loop out markers.
* The hotcue buttons and sample buttons become beatloop buttons, or when you mode-shift with the mode button they will alter the beatjump size instead. From the top: fractional beats on the left from 1/8 to 1, and multiple-beat loops on the right from 2 to 16.

If you wish to have a different range of durations to bind for the beatloops you can edit the `beatLoops` property in the preferences file.

#### Intro-Outro editing mode (aka: "prepare" mode)

* The hotcue buttons now grant access to hotcues 5-8 instead of 1-4.
* The sampler buttons now activate intro-start and intro-end on 1 and 2; and outro-start and outro-end on 3 and 4. If you mode-shift with the mode button it will clear the markers.

Intent of this mode is to help you prepare or cue up a track.

How you make use of the hotcues is up to you, but I like to stick the start/end of the beat or the melody (whichever makes sense for the track) on hotcue 5 and 7, and colour them Green for Groove; then stick start/end vocals on 6 and 8, colouring them Violet for Vocals.

### Assign B button

The *Assign B* button is your loop enable/disable/remove and save/load button.

* Press once to toggle whether your current loop is enabled or disabled.
* With mode-shift or mode-lock active *Assign B* will delete the current loop.
* If you hold *Assign B* down, pressing the hotcue 1-4 buttons will save the current loop to the hotcue or load it if one is already saved.
* Holding *Assign B* while mode-shifted or mode-locked and pressing hotcue 1-4 will clear the hotcue. This finger-twister is provided to preserve expected button combo behaviours, but you may find it easier to just use the regular hotcue clearing controls from within sample editing mode.

For simplicity, hotcues 5-8 aren't reachable for now to save loops to. This may change in a future version.

### Fader Start

With mode-shift active the channel faders will behave in "fader start/stop" mode, where fading up from a zero position will start the track playing from the current position, and fading down to zero will stop the track and return to the cue point.

If you just want the track to stop at the current position on fader stop, the `faderStopGotoCue` setting can be set to `false` in your preferences file.

### Quantize

You can toggle quantize for a deck with mode-shift + pfl (the "cue a" and "cue b" buttons). This binding doesn't have a handy mnemonic, but it seems to be used in a few other controller bindings - so I shamelessly borrowed it from there.
