# ADI-2 Control (unofficial Stream Deck plugin)

## What Is This (and what does it do?)

It's a plugin for the [Elgato Stream Deck][Stream Deck] on Windows and macOS that puts an [RME ADI-2][] converter on your keys and dials: volume, mute, source, EQ presets, loudness, setups and almost every setting from the unit's menus, with live values read from the unit, so the deck always shows what the ADI-2 is doing.

Works with the ADI-2 Pro EX, ADI-2 Pro FS / FS R BE, ADI-2/4 Pro SE and ADI-2 DAC. Runs on every Stream Deck: all actions work on keys (Stream Deck MK.2, Mini, XL, Neo, +, + XL and Stream Deck Mobile), and on a Stream Deck + or + XL they also work on the dials.

<img src="images/deck_plus.png" width="451" alt="Stream Deck+ layout">

## Release / Installation

Install it from the Elgato Marketplace inside the Stream Deck app, or download the plugin file from the GitHub Release and open it; either way it lands in Stream Deck's action list as *ADI-2 Control*. The plugin brings a ready-made profile for the Stream Deck MK.2, + and + XL (the layouts pictured below). Stream Deck offers to install it the first time the plugin is installed; at any other time, *Advanced → Stream Deck → Install the ADI-2 Control profile* in any action's settings asks Stream Deck to install it and switch to it.

Requires Stream Deck 7.1 or newer on Windows 10+ or macOS 12+, and the ADI-2 connected to the same computer by USB. On Windows, RME's MADIface series driver must be installed (the same one ADI-2 Remote needs). No other software is needed.

The ADI-2 can also be connected to a Raspberry Pi running [ADI-2-Pi][], and the deck controls it over the network (see [Over the network](#over-the-network-adi-2-pi)).

## Setup

1. Update the ADI-2 to the latest firmware (RME's Flash Update Tool, rme-audio.de → Downloads).
2. On the unit, set **SETUP → Options → Device Mode / DSD → MIDI Control → ON**. On the ADI-2 DAC this is under *Remap Keys / Diag*.
3. Drag an action onto a key or dial and pick the **Output** and **Function**, for example *Phones 3/4 → Volume*. The key shows the unit's current value straight away, and the status line at the bottom of the settings says *Connected*.

Everything technical sits in a collapsed *Advanced* section of each action; you normally don't need it.

### Over the network (ADI-2-Pi)

[ADI-2-Pi][] is a separate open-source project: the ADI-2 is connected to a Raspberry Pi by USB, and apps on the network control it through the Pi. If your ADI-2 is set up that way:

1. Open any action's settings, *Advanced → Unit → Connection* and choose **ADI-2-Pi on the network**.
2. **Address**: the Pi's name or IP address, for example `adi2-pi.local` (the default) or `192.168.1.20`. Add `:port` if the bridge doesn't run on port 80, e.g. `192.168.1.20:8080`. **Find** searches the network and lists each Pi by name and by IP address; pick one. **Connect** connects again right away.
   **Access key**: only if an access key is set on the Pi (System tab); enter the same key here, otherwise leave it empty.
3. The status line says *Connected · ADI-2 Pro via ADI-2-Pi*. Everything works as with USB, including the names of setups and EQ presets. Changes from the unit's knob, its remote or other apps using the bridge show up immediately.

With the ADI-2-Pi connection chosen, the actions also offer what the Pi keeps (these entries don't appear with USB):

- **Headphone profile** (Setting action, per output): apply one of the Pi's profiles; a key steps through them, selects one and lights while it is active, or switches between two; a dial browses them.
- **Scene** (Load Setup action): the Pi's scenes are listed after the nine setups.
- **Quick level** (Level action, volume keys): *Set to −25.0 dB* and so on, from the quick levels set on the Pi; the key lights while the volume is there. On a dial, *Push: Next ADI-2-Pi quick level* steps through them.
- **Sleep timer** (Setting action, Device): a key steps Off → 15 → 30 → 60 → 120 min and shows the minutes left; a dial browses the times.
- **Now playing** (Status action): title and artist of what the Pi plays (AirPlay, Spotify Connect, Bluetooth).

The setting applies to all actions. MIDI Control must still be on at the unit. A fixed IP address (a DHCP reservation in your router) is the most reliable choice if the name isn't found. On macOS, allow Stream Deck access to the local network if macOS asks (System Settings → Privacy & Security → Local Network). The Pi's volume limits and rise-speed limit apply to the deck too.

If the Pi is set to need the access key for changes only, the deck connects without the key *read-only*: keys and dials show the unit's values, but pressing or turning them shows an alert, and the Status key shows *Read only*. Enter the key for full access.

## Actions

| Action | Where | What it does |
|---|---|---|
| Level | Key or dial | Volume, bass, treble, balance, width, loudness, every parametric EQ band and input trim, per output. Rotate to adjust, push and turn for fine steps (0.1 dB on volume). On a dial, push / tap is assignable: mute, reset to 0 dB or centre, Recall, or Loudness / EQ / Bass-Treble / Dim on/off, so one dial adjusts e.g. the bass and switches Bass/Treble. Keys mute, step up or down, or recall a value; to switch Loudness, EQ and so on with a key, use a Setting key. |
| Setting | Key or dial | Source, output level / Hi-Power, crossfeed, DA and AD filter, mono, polarity, EQ and EQ preset, Bass/Treble, loudness, dim, mute, plus device settings like Basic Mode, clock source, SPDIF input, SRC, Dual / Balanced Phones, Auto Dark and display. A key steps through the options, selects one option and lights while it is active, or switches between two. On a dial: turn to choose and push to confirm, or change directly. |
| Load Setup | Key or dial | Loads one of the unit's nine stored setups, shown by the name stored in the unit. The setup last loaded from Stream Deck is marked. Hold the key to save the unit's current state into that setup. |
| Status | Key or dial | Read-only: the connection to the unit (green *Connected*, red *No device*, amber *Port busy*; through a Pi also red *No ADI-2-Pi* / *Access key?* and amber *MIDI off* / *Read only*), captioned with the unit's name so it stands apart from other plugins' connection keys; the unit itself (name and device ID); its clock source and sample rate; or the digital input signal (whether the AES and the SPDIF input receive a valid signal – ADI-2 Pro models). Press to re-read the unit; without a connection it reconnects. |

**Hold**: a key can carry a second action that runs when it is held for about half a second (*Hold* in the key's settings); a short press then acts on release. Level keys: mute, recall or reset; EQ preset keys: store the unit's current EQ (bands and Bass/Treble) as the key's preset, keeping the preset's name; Load Setup keys: save the current state into the setup.

**Titles**: a title typed into Stream Deck's own Title field replaces the control's caption (output and function). On a key Stream Deck draws it; on a Stream Deck + dial the plugin draws it at the top of its panel.

**Recall and Link** work like in ADI-2 Remote, where they are app features too: Recall jumps to a stored value and back on the next press; a volume can be linked to other outputs so they move and mute together.

When a key steps through options, those the unit refuses at the moment, such as a source without a signal, are skipped.

## Look

Under *Advanced → Look* in the Level, Setting and Load Setup actions (it applies to all of them):

- **Colours**: follow the unit's *Meter Color* (default), or ADI-2 Remote's colours per output. Change Meter Color on the unit and the deck follows.
- **Level style** (Level controls only): ADI-2 Remote knobs; the unit's volume screen; or the knob, switching to the unit's volume screen while a value changes (default). The volume screen follows the unit's *Display Mode* (Default / Dark).

## Layout examples

A standard Stream Deck (MK.2, 5 × 3) set up for headphone listening: volume down / volume / up for Phones 3/4, two keys that switch the phones between AES and Analog like radio buttons (say two PCs), EQ preset, EQ, loudness, crossfeed and bass. The bottom row has the speaker volume and mute, two setups and a Status key showing the connection.

<img src="images/deck_standard.png" width="560" alt="Stream Deck layout">

A Stream Deck + XL (9 × 4 keys, six dials; official Elgato picture) has room for the rest: filters, Bass/Treble, balance, output level and Hi-Power, Line Out with loudness and EQ, Phones 1/2, the headphone/line switching options, four setups and the device settings. The dials carry the phones volume (here in the unit-display look), the line out, bass, treble, an EQ preset browser and the setup loader.

<img src="images/deck_xl.png" width="976" alt="Stream Deck + XL layout">

## Good to know

- **No level meters.** The ADI-2 doesn't send audio levels over MIDI, only settings.
- **Changes made in RME's ADI-2 Remote** (USB connection) reach the deck within 15 seconds; the unit doesn't report them to other programs. Changes on the unit itself or by IR remote show up immediately. Through an ADI-2-Pi, changes made by other apps using the Pi show up immediately.
- **Windows: "BLOCKED BY WINDOWS"** means Windows refused to start the plugin's small MIDI helper (`adi2midi-win.exe`). The usual cause is **Smart App Control** (Windows Security → App & browser control → Smart App Control), which blocks every program without a paid code-signing certificate; the Windows helper isn't signed. Smart App Control has no per-app exception, so the plugin can only run with it set to *Off* (or *Evaluation*). Less often, an antivirus has removed the file; reinstalling the plugin brings it back. The macOS version is signed and notarized by Apple and isn't affected.
- **"NEEDS ADI-2-PI"**: the key uses something the Pi keeps (profile, scene, quick level, sleep timer, now playing) but the connection is set to USB.
- **"NO ADI-2-PI"** (network connection only): the Pi doesn't answer at the address entered. Check that it's on and in the same network, and try its IP address instead of the name. **"MIDI CONTROL OFF"**: the Pi reaches the unit but the unit doesn't answer; switch MIDI Control on. **"ACCESS KEY?"**: the Pi has an access key and the one entered is missing or wrong; the plugin then stops trying until you enter it (or press the Status key). After ten wrong keys in a minute the Pi refuses everything for a minute (**"KEY LOCKED 1 MIN"**); the plugin waits and tries again by itself.
- **Windows: "PORT BUSY"** means another program (usually ADI-2 Remote or a DAW) has the ADI-2's MIDI port open. Windows lets one program use a MIDI port at a time; close the other one. macOS and Windows 11 with Windows MIDI Services don't have this limit.
- **DAWs**: if a DAW uses the ADI-2's MIDI port as an input (seen with Logic Pro), the unit may ignore the deck while audio plays. Disable that MIDI input in the DAW.
- The plugin follows RME's published MIDI table for the ADI-2 series. The ADI-2 Pro EX identifies itself as an ADI-2 Pro and works the same way.

# I have an issue or miss a feature?

You can submit an issue or request a feature with [GitHub issues]. Please describe as precisely as possible what went wrong and also include the log file, it is incredibly helpful for me to figure out what went wrong. Logs can be found in `%APPDATA%\Elgato\StreamDeck\Plugins\de.shells.adi2control.sdPlugin\logs` on Windows and `~/Library/Application Support/com.elgato.StreamDeck/Plugins/de.shells.adi2control.sdPlugin/logs` on macOS.

If a setting shows the wrong name or doesn't react, the *MIDI monitor* under *Advanced* in the action's settings shows what goes to and comes from the unit; a copy of that helps too.

# Contribute

If you're interested in using this plugin but something you really need is missing, let me know. I don't have access to every ADI-2 model, so I can't try things on all the boxes myself, but eventually we might find a way to work something out.

# Support

If you'd like to drop me a coffee for the hours I've spent on this: [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/dwshells) [!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/dwshells)

# Disclaimer

This is a private project, I am not affiliated with RME or Elgato. I wrote this plugin out of personal interest.

<!-- Reference Links -->

[Stream Deck]: https://www.elgato.com/gaming/stream-deck/ "Elgato's Stream Deck product page"
[RME ADI-2]: https://www.rme-audio.de/converter/adi-2-pro-ex.html "RME's ADI-2 Pro EX product page"
[GitHub issues]: https://github.com/shells-dw/streamdeck-adi2/issues "GitHub issues link"
[ADI-2-Pi]: https://github.com/shells-dw/adi2-pi "ADI-2-Pi on GitHub"
