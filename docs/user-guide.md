# User guide

The window has a menu on the left (**Main**, **Post-processing**, and **Settings** at the
bottom), a top bar with the profile, car, track and the **Load / Save** buttons, and a footer.

- **Footer, left:** the game state — *waiting for game*, *connected*, *on track* — and the
  calculation mode in use. Problems show here too (no force, script error, wheel not acquired).
- **Footer, right:** the wheel (green dot = acquired), the version and the Discord link.

## Main

![Main page](../img/main.png)

**Output**

- **Output FFB** — the master strength of the force sent to the wheel (0–100 %).
- **Invert FFB** — flips the direction of the force, for wheels that need it.
- **FFB Calculation Mode** — the calculation script in use. Each `.ffb` file in the scripts
  folder is a mode (see [Custom scripts](custom-scripts.md)).
- **Output Type** — shown only by modes that blend the game's native force with a force built
  from telemetry. *Basic* does not use it.

**Effects** — one row per effect declared by the calculation mode. With *Basic*:

| Effect | Bars |
|---|---|
| ABS vibration | Intensity · Frequency |
| Understeer | Intensity (with a second cursor: the minimum level before the vibration starts) · Linearity · Frequency |
| Oversteer | Same as Understeer |

The frequency can be **a single value** or **a range**; the button before the bar switches
between them. With a range, the arrow button after the bar chooses whether the frequency rises
(↗) or falls (↘) as the effect grows. A frequency of 0 Hz means no vibration.

All effects start at 0 % — off.

## Post-processing

![Post-processing page](../img/post-processing.png)

- **Linearity**, **Slew**, **LP Hz**, **LP Q** — the post-processing chain
  (see [How it works](how-it-works.md#3-post-processing)).
- **Equalizer** — drag a band up or down to boost or cut it (±20 dB); the orange bars between
  the bands set how wide each adjustment is (Q).

These values are **global**: they are not part of the car profiles.

## Settings

![Settings page](../img/settings.png)

| Section | What it does |
|---|---|
| **FFB Device** | The wheel base FFB Lab drives. Only devices with force feedback are listed. Next to the list: *Acquired*, or the reason it could not be. |
| **Startup** | *Start with Windows* (opens when you sign in) and *Start minimized*. |
| **Overlays** | *Show overlay in simulator* (on by default) and *Show VR overlay* (off by default). See [Overlays](overlays.md). |
| **Device shortcuts** | Four steps (−10 %, −1 %, +1 %, +10 %) for *Output Type* and for *Output FFB*. Click a box and press a wheel button, a hat direction or a key; right-click a box to clear it. |
| **General** | Language: English, Português (Brasil), Español, Français, Deutsch. |

## Profiles

FFB Lab keeps two kinds of profile:

- **Global** — your default values. Used when no car is loaded, and as the starting point for a
  car that has no profile yet.
- **Car profile** — the values for one car on one track.

**When you go on track** FFB Lab loads, on its own:

1. the most recent profile of **this car on this track**; if there is none,
2. the most recent profile of **this car on any other track**; if there is none,
3. the **Global** values.

The top bar shows which one is in use, and the footer says where it came from.

**Save** (with a car loaded) saves the car profile for the current track. If one already exists
you can overwrite it or save it under a new name (e.g. `quali-dry`).

**Load** lists the saved profiles, best matches first: ✅ this car on this track, 🔵 same car
category, ⚠ other cars — loading one of those asks for confirmation, since it may bring
settings that do not suit the car.

**When you leave the track** with changes that were not saved, FFB Lab asks whether to save the
car profile.

A car profile holds the Main page values (*Output FFB*, *Invert FFB*, *Output Type*), the
equalizer and the effect settings of the calculation mode. Post-processing is always global.

If you edit a calculation mode and change the declaration of an effect (add or remove a
property, change a range), the saved values of that effect no longer apply and go back to the
default — the footer tells you when this happens.

## Where your files are

Everything FFB Lab writes is in **`%APPDATA%\FFB_Lab\R3E\`** (paste that into the Explorer
address bar):

| Folder / file | Contents |
|---|---|
| `perfis\global.json` | The Global profile and the settings. |
| `perfis\carros\<car id>\<track>\` | Car profiles. |
| `scripts\` | The calculation modes (`basic.ffb` and your own). |
| `constantes\` | Car constants and car names you can override — see [Constants](constants.md). |
| `logs\ffblab-r3e.log` | The log. Useful for bug reports. |

Nothing is sent anywhere: all of it stays on your computer. Files you edited are never written
over by the application.

## First start

1. **Before you start** — turn RaceRoom's force feedback off, read the disclaimer, and accept.
   Closing this screen exits FFB Lab; it comes back until you accept.
2. A short guide in **Settings**: pick your wheel and wait for *Acquired*; choose whether FFB
   Lab starts with Windows; choose whether to show the VR overlay. Then you are taken to Main.
