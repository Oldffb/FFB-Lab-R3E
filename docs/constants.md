# Constants

Cars behave differently: a kart is not a GT3, and a front-wheel-drive touring car is not a
prototype. **Constants** are per-car reference values — physical data such as the wheelbase, and
reference scales for the effects — that calculation scripts read to adapt to the car on track.

The force itself is still calculated by the script. Constants are only numbers the script can
use; a script that does not read them is not affected by them.

---

## How a car gets its constants

Constants are organised in **profiles**. Each car is linked to one profile, and many cars share
the same profile.

```
car on track (id) ──► car → profile list ──► profile ──► car.<field> in the script
```

FFB Lab ships with **11 profiles** and **232 cars** already linked:

| Profile | Category (`car.Category`) |
|---|---|
| Kart | Kart |
| FWD | FWD |
| RWD GT | RWD GT |
| RWD LMP | RWD LMP |
| AWD | AWD |
| GT3 RWD Rear | GT3 |
| DTM2020 RWD Front | DTM |
| GTE RWD | GTE |
| GT4 RWD | GT4 |
| Touring RWD | Touring |
| Formula RWD | Formula |

- A car that is not in the list uses the **RWD GT** profile.
- The constants arrive by themselves when you go on track, and stay while you switch modes.
- The car's name at the top of the window comes from a separate list of car names. A car that is
  not in that list is shown by its number (`#1234`).

## In a script

- `car.<field>` — a value of the profile of the car on track, e.g. `car.WheelBase`.
- `car.Category` — the category of the profile, to compare with `cat.<name>`:
  ```
  is_gt3 = car.Category == cat.GT3
  ```
  In `cat.` names, spaces and symbols become `_` (a category *RWD GT* is `cat.RWD_GT`).
- A field that the profile does not have reads as **0**. A field that no profile has is a compile
  error.

Each profile has about 55 fields. They are grouped by their prefix:

| Prefix | Used for |
|---|---|
| `Physical…`, `WheelBase` | physical data of the car (caster, steering ratio, weight, wheelbase) |
| `Us…` | understeer references |
| `Os…` | oversteer references |
| `Sat…` | self-aligning torque references |
| `Wt…` | weight transfer references |
| `Df…`, `DownforceRef` | downforce references |
| `Susp…` | suspension references |
| `Gyr…` | gyroscopic references |
| `LatGRef`, `LongGRef`, `KSteerToYaw`, `SlewMinDelta` | general references |

The **Basic** mode reads the understeer and oversteer references (`car.Us…`, `car.Os…`),
`car.WheelBase` and `car.KSteerToYaw`. Most of the other fields are there for scripts you write.

---

## Overriding constants

The built-in profiles are inside the executable. To change them, put your own files in:

```
%APPDATA%\FFB_Lab\R3E\constantes\
```

| File | Contents |
|---|---|
| `vehicle_profiles.json` | A list of profiles. Each has a `Name`, a `Category` and its numbers. |
| `car_definitions.json` | Car id → profile name, e.g. `{ "13524": "GT3 RWD Rear" }`. |
| `r3e-data.json` | The car names shown at the top of the window. Put there on the first start; replace it with a newer one when the game gets new cars. |

Your files are read **on top of** the built-in ones:

- A profile with the **same `Name`** as a built-in one **replaces it completely** — include every
  field, or the missing ones read as 0.
- A profile with a **new `Name`** is added. Link cars to it in `car_definitions.json`.
- A car in your `car_definitions.json` is linked to the profile you give, instead of the built-in
  one.
- Only numbers are read; text values are ignored.

**Restart FFB Lab after editing these files** — constants are read when the application starts.
A file that cannot be read, or a car linked to a profile that does not exist, never stops the
application: it is written as a warning in the log
(`%APPDATA%\FFB_Lab\R3E\logs\ffblab-r3e.log`), and the car uses the RWD GT profile.

Example — a new profile for one car (13524 is the Porsche 911 GT3 R (992.2)):

```json
[
  {
    "Name": "My GT3",
    "Category": "GT3",
    "WheelBase": 2.6
  }
]
```

```json
{ "13524": "My GT3" }
```

In this example every field other than `WheelBase` reads as 0, so a script that uses other
fields would behave differently. Give a profile all the fields your mode reads.
