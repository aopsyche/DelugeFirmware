# DX7 synth

## Description:

Synth type fully compatible with DX7 patches with full editing support.

The implementation is based on
[Music Synthesizer for Android](https://github.com/google/music-synthesizer-for-android) for the
"modern" engine, and [Dexed](https://github.com/asb2m10/dexed), for loading
of .syx patch banks as well as a tuned "vintage" engine.

Implemented as a special case of the subtractive engine, where OSC1 is changed to a new DX7 oscillator type.
It is possible to use the filters on a DX7 patch (though this increase CPU usage and decrease polyphony).

## Usage:

Optional: place DX7 compatible patch banks in .syx format in a "DX7/" folder in the sdcard.

Enable the "DX SHORTUCTS" community setting for full access to the relevant UI behaviors.

To create a new synth in DX7 mode, use the new shortcut "CUSTOM 1" + "SYNTH".
This shortcut has to be used to create a new DX7 synth
rather than changing the osc type of a existing patch (or the default init patch)
as it sets up better defaults, like opening the map ENV1, so the
DX7 envelopes can be heard, and no default VELOCITY->global volume
modulation, as velocity sensitivity is set per operator anyway.

This shortcut will show up a menu. The first option in this menu lets you load patches.
First you browse for .syx files in the folder (nested subdirs are allowed).
After selecting a file, you will get a list with the 32 patches in the back.
Make sure to enable "KEYBOARD", to be able to audition different patches while browsing through.

If you want to create a patch from scratch, you can use the second "DX7 PARAMETERS" option,
or just exit the menu to use the shortcuts for editing.

To re-enter this menu later, use the shift+osc1 type shortcut and press SELECT when DX7 value is active.
However for interactive editing you likely want to use the DX7 sidebar to quickly jump to relevant operator parameters.

With the community setting active, this column will automatically appear as the right column,
unless it was manually set to something else, when KEYBOARD screen is active.
The first 6 rows shows the operators as green for a carrier and blue for a modulator, respectively
taping a single pad in isolation lets you switch an operator on or off. Shift+colored pad
opens the editor for that operator. First, the level parameter is selected, and shift+pad again
gives you the COARSE tuning parameter.
Likewise, shift+press the seventh pad will open up the editor for global parameters,
starting with algorithm and feedback.

Inside the editor view (when the sidebar is flashing), the LEFT-RIGHT encoder can be used
to browse between parameters. Some parameters have a shortcut in a layer which is only
active when editing any DX7 parameter (otherwise the regular synth shortcuts apply)

For the first 6 rows these are the operators (the bold row in the table repeated six times).
The seventh and eight row contains some of the global parameters

| rate1 | 2 | 3 | 4| level1 | 2 | 3 | 4 | level | mode | coarse | fine | detune | velo |
| -- | -- | -- | --| -- | --  | --  | --  | -- | -- | -- | -- | -- | -- |
| rate1 | 2 | 3 | 4| level1 | 2 | 3 | 4 | algo | feedback | sync | | | |
| LFO rate | delay | pitch | amp | sync | shape | sens | detune |  | |  |  |  |  |

If OLED screen is available a group of related parameters is displayed at a time, like all the envelope levels and rates
for a specific envelope. With 7SEG the parameter name will be shown briefly before the value

Press back/undo to exit the dx layer, and access the standard synth shortcuts again.

## Engine versions

Two engine versions are available, "modern" and "vintage", as well as the "auto" engine
mode used by default.

Auto will use the "modern" engine for _most_ algorithms
is chosen as it offers higher polyphony via an optimized SIMD implementation.
However when feedback is activated on algorithm 4 and 6 (those will multi-operator feedback),
"auto" will instead use the "vintage" engine as the modern one doesn't implement these algorithms properly.
"vintage" engine is meant to more accurately model a DX7 synth, but has lower polyphony.

## Missing features:

- [ ] midi implementation
- [ ] modulating/automating individual operators