# How it works

FFB Lab - R3E sits between RaceRoom and your wheel base. The game publishes its telemetry in
shared memory for third-party programs; FFB Lab reads it, calculates the force, and sends it to
the wheel. It does not modify, patch or inject anything into the game.

```
RaceRoom ──shared memory──► FFB Lab ──► calculation script ──► post-processing ──► safety ──► wheel
                                         (your .ffb mode)       (EQ, slew,           (limits,
                                                                 low-pass,            invert,
                                                                 linearity)           Output FFB)
```

## 1. Reading the game

FFB Lab waits for RaceRoom (`RRRE64.exe`). The footer on the left shows where it is:
*waiting for game*, *connected*, *on track*. Everything the game publishes — speed, steering,
yaw, tyre grip and load, the game's own force feedback value, ABS, and so on — is available to
the calculation script.

The car on track is recognised by its id. Its name and the track appear at
the top of the window, and the car's [constants](constants.md) are loaded for the script.

## 2. The calculation script

The force is calculated by a **calculation mode**: a plain-text `.ffb` file you pick in the
*FFB Calculation Mode* list on the Main page. It runs **500 times per second**.

The mode that ships with the application is **Basic**:

- it takes the force the game itself calculates (the "native" force) as the base;
- on top of it, three effects you control in the *Effects* table:
  - **Understeer** — the force lightens when the front tyres lose grip;
  - **Oversteer** — the force pushes in the direction of the slide when the rear lets go;
  - **ABS vibration** — a vibration while ABS is working.
- all three start **off** (intensity 0 %), so out of the box Basic feels like the game's own
  force feedback.

Each effect row has an intensity bar, and depending on the effect a linearity bar, a
vibration threshold and a frequency (single, or a range that rises or falls with the effect).
These rows are not hard-coded: the script declares them, and the application draws them.

A mode can also offer an **Output Type** bar that blends the native force with a force built
from telemetry. Basic works only on the native force, so it does not show that bar.

### The Default mode

The package also brings **Default** (`default.ffb`, in `Custom FFB Scripts\`). It is not put in
the scripts folder on its own: copy it to `%APPDATA%\FFB_Lab\R3E\scripts\` and restart FFB Lab,
and it appears in the mode list. Basic stays the main mode.

Default builds a force from the telemetry and blends it with the game's own force through the
**Output Type** bar: *Nat 100 %* is the game's force only, *Tel 100 %* is the telemetry force
only. Its effects, all **off** (0 %) to start with:

- **Self-aligning torque** — the pull of the front tyres as they take cornering force; it goes
  light as they reach their limit.
- **Lateral G** — weight in the wheel that follows the cornering force.
- **Gyroscopic** — the extra weight of the spinning front wheels in fast corners.
- **Weight transfer** — resistance to turning the wheel while braking, with a light parking
  effort when the car is (almost) stopped.
- **Understeer** — lightens the telemetry force when the front tyres slide.
- **Oversteer** — pushes in the direction of the slide when the rear lets go.
- **ABS vibration** — a vibration while ABS is working.

The ABS vibration and the understeer/oversteer vibrations are added at the very end, each with
its own intensity: they are felt whatever the Output Type bar says. With every effect at 0 and
the bar at *Nat 100 %*, the force is exactly the game's — a good starting point to bring the
effects in one at a time.

You can write your own modes — see [Custom scripts](custom-scripts.md).

## 3. Post-processing

What the script produces then goes through a fixed chain, set on the *Post-processing* page:

| Step | What it does |
|---|---|
| **Equalizer** | 8 bands (1, 5, 10, 20, 50, 80, 120, 240 Hz), ±20 dB each, with an adjustable Q. Grouped as *Weight & Grip*, *Kerbs & Big Bumps*, *Bumps & Surface* and *Road Texture*. |
| **Slew** | Limits how fast the force can change. 0 % = off. |
| **LP Hz / LP Q** | A low-pass filter that removes fine detail above the chosen frequency. *off* = no filter. |
| **Linearity** | Reshapes the force curve (from −0.50 to +0.50). 0 = linear. |

## 4. Safety and output

Before anything reaches the wheel:

- a value that is not a valid number becomes 0;
- the force is limited to the wheel's range (−100 % … +100 %);
- **Invert FFB** flips the direction, for wheels that need it;
- **Output FFB** scales the whole force (0–100 %) — this is your master strength control;
- when you go on track the force **fades in over 3 seconds**;
- in menus, pause, replays and the pit lane the force is **zero**.

If a calculation script has an error, it does not run at all: the force stays at zero and the
footer shows the first error with its line number.

## 5. The wheel

FFB Lab talks to the wheel through DirectInput and takes it in **exclusive** mode — that is why
the game's own force feedback must be off, and why no other force feedback program can drive the
same wheel at the same time. The device is chosen in *Settings → FFB Device*; the footer on the
right shows it with a green dot when it is acquired.

If the wheel cannot be acquired, the footer says why (another program has the wheel, the device
is not connected, the device was lost after USB or a lock screen…). The application keeps
trying.
