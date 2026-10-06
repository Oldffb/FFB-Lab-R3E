# Overlays

FFB Lab has two overlays. Both are a compact copy of the **Main** and **Post-processing**
pages, so you can adjust the force without leaving the car:

- the **on-screen overlay**, a window on top of the game;
- the **SteamVR overlay**, the same panel inside your headset.

The overlays and the main window all show the **same values**: what you change in one appears in
the others at once. *Load* and *Save* work from the overlays too. The Settings page is only in the
main window.

Both are switched on and off in **Settings → Overlays**.

---

## On-screen overlay

![On-screen overlay, Main page](../img/overlay-screen.png)

![On-screen overlay, Post-processing page](../img/overlay-screen-post.png)

**When it shows:** with *Show overlay in simulator* on (the default), the overlay appears when
RaceRoom is in a session — on track or in the pit lane — and hides in the menus and when the game
is closed.

**Using it:**

- It stays on top of the game and does not take the focus away from it when it appears. Click it
  to use it, as with any window.
- **Moving it:** drag it by any place that is not a control — the top bar, the footer, the menu,
  the panel titles or backgrounds.
- **Transparency:** the *Transp.* bar (0–85 %) makes the backgrounds see-through. Text, bars and
  buttons always stay opaque, so they can be read on track.
- **Minimising panels:** *Effects* and *Equalizer* have a button on the right of their title.
  A minimised panel shows only its title.
- It is not in the Alt+Tab list.

Position, transparency and minimised panels are remembered (`overlay.json` in the data folder).

> If the overlay does not appear over the game, run RaceRoom in **borderless** or **windowed**
> mode: a window cannot be drawn over a game in exclusive full screen.

---

## SteamVR overlay

![SteamVR overlay panel](../img/overlay-vr.png)

**Requirements:** SteamVR and a headset, with RaceRoom running **in VR through SteamVR**. When
the game runs on a flat screen or through the Oculus runtime, the panel would not be visible in
the game, so FFB Lab does not show it (the log says why).

**Turning it on:** *Settings → Overlays → Show VR overlay* (off by default).

- FFB Lab **never starts SteamVR**. It waits for you to open it and then connects. If SteamVR
  closes, FFB Lab lets go and waits again.
- The panel shows when you are in the pit or on track, and hides in the menus.
- Turning the setting off disconnects from SteamVR immediately.

**Using it with the controllers:**

- Point a controller's laser at the panel and pull the trigger to click, or hold it to drag a
  bar or an equalizer band.
- Lists open as a choice inside the headset; text boxes (a profile name, for example) use the
  SteamVR keyboard.
- **Moving the panel:** point at any place that is not a control, hold, and move the controller.

**The headset row** at the top of the panel:

| Button | What it does |
|---|---|
| **Dist. − / +** | Brings the panel closer or pushes it away, 5 cm at a time. The controller's scroll does the same. |
| **Curve − / +** | Curves the panel around you, step by step (0 = flat). |
| **Face me** | Turns the panel towards your head without moving it. A curved panel only turns horizontally. |
| **Reset** | Puts the panel back in front of the headset, level. |

The first time, the panel appears in front of the driver's head. The VR panel has its own transparency and minimised panels (the first time, it takes those of the
on-screen overlay). Its position and curve are remembered (`vr-overlay.json` in the data folder).
