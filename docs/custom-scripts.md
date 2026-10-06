# Custom scripts

Every calculation mode in FFB Lab is a plain-text `.ffb` file. The application reads it,
compiles it when you pick the mode, and runs it **500 times per second**. What the script writes
to `output` is the force that goes on to the wheel.

The mode that ships with the application, **Basic** (`basic.ffb`), is also the template: it is
heavily commented and explains, section by section, everything on this page with examples.
Read it first.

> **A script that compiles is not a safe script.** FFB Lab cannot tell a good force from a wrong
> one. Read [Take care](#take-care) before driving with a mode you wrote or edited.

---

## Where the scripts are

The application reads its modes from:

```
%APPDATA%\FFB_Lab\R3E\scripts\
```

(paste that line into the Windows Explorer address bar). On the first start FFB Lab puts
`basic.ffb` there. After that it never writes over it: your edits stay.

The release zip also has a `Custom FFB Scripts\` folder next to the executable, with a copy of
`basic.ffb`. **That copy is for reading only** — the application does not read that folder.

## Making your own mode

1. Copy `basic.ffb` into `%APPDATA%\FFB_Lab\R3E\scripts\` with a **new name**, e.g.
   `my_mode.ffb`. Do not overwrite `basic.ffb`: it is the mode that always works, and the one to go
   back to.
2. Change the first two lines of the copy:
   ```
   # mode: My mode
   # description: what it does, in one line
   ```
   Without `# mode:` the list shows the file name. Two files with the same `# mode:` show up as two
   modes with the same name.
3. Restart FFB Lab — a new file appears in the list only after a restart.
4. Pick it in **FFB Calculation Mode** on the Main page.

**After editing a mode that is already in the list:** save the file, pick another mode, then pick
yours again. It is read from the disk and compiled again.

Save the files as plain text (UTF-8), and keep your own copies of what you write.

## Errors and warnings

- A script with an error **does not load**. The force stays at **zero**, the footer shows the
  first error with its line, and the log has all of them, with line and column:
  `%APPDATA%\FFB_Lab\R3E\logs\ffblab-r3e.log`
- If the wheel goes limp after an edit, look at the footer or the log first.
- Warnings (for example, reading a variable before writing it) do not stop the mode from loading.

---

## The language

FFB Lab scripts use the application's own small calculation language — not C#, Lua or Python.

- **One assignment per line:** `name = expression`. An expression can continue on the next line
  if the line ends with an operator, or the next line starts with one:
  ```
  total = a
        + b
        + c
  ```
- **Comments** start with `#` or `//`.
- **All values are decimal numbers.** Conditions give `1` (true) or `0` (false).
- **Operators:** `+ - * / %` · `< <= > >= == !=` · `&& || !` · `condition ? if_true : if_false`
- **Functions:** `min max abs sqrt sin cos tanh sign pow clamp lerp add_raw`
- **Functions with memory** (filters, oscillators): `smooth lp1 hp1 slew osc`
- **No loops and no user-defined functions**: every run always finishes.

Two things that surprise — both explained with examples in section 4 of `basic.ffb`:

- **Variables remember the previous run.** Reading a name before writing it in the same run gives
  the value it had in the previous run (0 on the first). The compiler warns when this happens.
- **`target = target + x` is not a plain sum.** In that form a term that opposes the total can
  cancel it but never invert it. That is right for combining forces, and wrong inside a filter.
  For a plain sum, use `add_raw(target, x)`.

## Names you can use

| Name | What it is | |
|---|---|---|
| `sm.<field>` | What RaceRoom's shared memory delivers, untreated. See the [list below](#game-values-sm). | read-only |
| `car.<field>` | The constants of the car on track. See [Constants](constants.md). | read-only |
| `cat.<name>` | A car category, to compare with `car.Category` — e.g. `car.Category == cat.GT3`. | read-only |
| `ui.<field>` | The application's controls (see below). | read-only |
| `fx.<id>.<prop>` | The on-screen settings of an effect declared in the file, e.g. `fx.abs.amp`. | read-only |
| `dt` | Seconds since the previous run (about 0.002). | read-only |
| `first` | `1` on the first run, `0` after. | read-only |
| `new_physics_frame` | `1` when the game delivered new physics since the previous run. | read-only |
| `blend_pct` | The *Output Type* bar: 0 = native, 100 = telemetry. | read-only |
| `output` | **What you write: the force that goes to the wheel. Required** — a script that never writes it does not load. | write |
| any other name | Your own variable, visible only inside the file. | read / write |

Writing to a read-only name is an error. Names are case-sensitive; a name that does not exist is an
error (if only the capitalisation is wrong, the message gives the right name). Names from before
v1.7 (in Portuguese) still compile, with a warning that gives the new name.

**`ui.` fields:** `ui.BlendPct` (Output Type), `ui.OutputScalePct` (Output FFB), `ui.Invert`,
`ui.EqGain0` … `ui.EqGain7` and `ui.EqQ0` … `ui.EqQ6` (equalizer), `ui.PostLinearity`,
`ui.PostSlew`, `ui.PostLowPassHz`, `ui.PostLowPassQ` (post-processing).

**Directions in RaceRoom:** a negative force turns the wheel to the left, and a positive
`sm.YawRate` means the car is turning left.

## Effects with on-screen settings

A mode can declare effects at the top of the file, before any calculation. Each effect becomes a
row in the *Effects* table, and each property a control in that row:

```
effect <id> "<text shown in the Effects table>"
  <property> <min>..<max> = <default>
  ...
end
```

The list of properties is closed:

| Property | Control | Example |
|---|---|---|
| `mix` | percentage bar | `mix 0..100 = 50` |
| `lin` | linearity bar (negative to positive) | `lin -50..50 = 0` |
| `slew` | bar | `slew 0..100 = 0` |
| `damper` | bar | `damper 0..200 = 70` |
| `amp` | intensity bar; `minact` adds a second cursor, the minimum level before the vibration starts | `amp 0..100 = 40 minact 5` |
| `vib` | on / off switch | `vib = on` |
| `freq` | a single frequency, or a range that rises (`up`) or falls (`down`) with the effect | `freq 0..100 = 30` · `freq 5..250 = 20..60 up` |

- Frequencies go up to **250 Hz**.
- A bar can have its own colour at the end of the line, in quotes: `amp 0..100 = 40 color "#7d8fa3"`.
- The script reads the settings as `fx.<id>.<property>` — and, for a range, `fx.<id>.freq_min`,
  `fx.<id>.freq_max`, `fx.<id>.dir` (1 = rising); with `minact`, `fx.<id>.amp_min`.
- The settings are saved in the profiles (global and per car), separately for each mode.
- **Changing an effect's declaration** (adding or removing a property, changing a range, switching
  between a single frequency and a range) resets that effect's saved settings to the default. The
  footer tells you when this happens.

## What happens after `output`

The script does not control this part:

```
game → [ your script ] → output → post-processing → safety → Invert FFB → Output FFB % → wheel
```

- **Post-processing:** equalizer, slew, low-pass filter and linearity, from the *Post-processing*
  page.
- **Safety:** a value that is not a valid number becomes 0, and the force is limited to −1 … +1,
  where 1 is the full force of the wheel times *Output FFB*.
- Going on track, the force rises from 0 over 3 seconds. In menus, pause, replays and the pit lane
  it is 0.
- A mode change is applied between two runs, never in the middle of one.

**What the application protects you from:** a script with an error does not load; division by 0
gives 0; the square root of a negative number gives 0; a value that is not a number becomes 0 on
every assignment; any failure while running gives force 0 and the application keeps going.

---

## Take care

- **Start low.** *Output FFB* low, hands on the wheel, the car stopped or slow. Raise it only when
  the behaviour is right. With direct-drive wheels a wrong force can hurt. Keep your wheelbase's
  own torque limit and emergency stop active.
- **Wrong sign** = a wheel that pushes away from the centre instead of back to it. It can build up
  into a violent oscillation. If the wheel pulls to one side by itself, or shakes harder and
  harder, let go and switch back to Basic.
- **Clipping.** 1 (or −1) is already 100 % of the wheel. Adding terms can keep the force at the
  limit for long periods: the detail disappears and the wheel can overheat.
- **Steps.** A value that jumps from one run to the next is felt as a knock. Smooth sudden changes
  (`slew`, `lp1`, `smooth`).
- **Vibrations.** High amplitude at high frequency is harsh, noisy and heats the motor. Keep
  vibrations small.
- **Filters** written as `x = x + …` get stuck at zero — use `add_raw` (see above).
- **Keep scripts lean.** Each run has about 2 milliseconds. A very long script can make the
  application miss its 500 runs per second.

---

## Game values (`sm.`)

The values the script can read from RaceRoom (a few raw diagnostic fields are left out). Wheels: `FL` front left, `FR` front right,
`RL` rear left, `RR` rear right. Switches (on/off) read as 1 or 0.

| Group | Names |
|---|---|
| Steering | `SteeringForcePct` (the game's own force feedback, −1 … +1), `SteeringForce`, `SteeringInputNorm` (−1 … +1), `SteeringAngle` (radians) |
| Speed | `VelocityMs`, `VelocityKmh`, `LocalVelX` (sideways), `LocalVelY` (vertical), `LocalVelZ` (forward), `PlayerVelWX`, `PlayerVelWY`, `PlayerVelWZ`, `ChassisSlipAngleDeg` |
| Acceleration | `AccelerationX` (lateral), `AccelerationY` (longitudinal), `AccelerationZ` (vertical), `GforceX`, `GforceY`, `GforceZ`, `BodyAccelX`, `BodyAccelY`, `BodyAccelZ` |
| Rotation | `YawRate`, `YawRatePrev`, `LocalAngVelRoll`, `LocalAngVelPitch`, `PlayerOrientRoll`, `OrientPitch` |
| Tyres | `GripFL/FR/RL/RR` (0 … 1), `GripFrontAvg`, `GripRearAvg`, `TireRps…`, `TireLoad…`, `TireSpeed…`, `TireWear…`, `TirePressure…`, `TireOnMtrl…` (surface), `TireDirt…`, `TireFlat…`, `TireTempFL_L/_C/_R` … `TireTempRR_L/_C/_R` |
| Suspension | `SuspVel…`, `SuspVelFrontAvg`, `SuspDefl…`, `RideHeight…`, `Camber…`, `FrontRollAngle`, `RearRollAngle`, `DamageSuspension` |
| Pedals and aids | `ThrottlePedal`, `BrakePedal`, `BrakePedalRaw`, `BrakePressure`, `BrakePressFL/FR/RL/RR`, `BrakeBias`, `AbsActive`, `TcActive`, `TractionControlPct`, `TcSetting` |
| Car | `EngineRpm`, `EngineTorque`, `CurrentGear`, `Downforce`, `TotalMass`, `FuelLeft`, `CarModelId` |
| Session | `GameState`, `InPitlane`, `TrackId`, `LayoutId`, `SimTicks`, `PlayerSimTime` |

`…` stands for the four wheels — e.g. `TireLoadFL`, `TireLoadFR`, `TireLoadRL`, `TireLoadRR`.
