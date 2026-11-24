# Design-file map (PULSE line follower)

Example layout; adjust the paths to the user's project folder. Re-check paths before use; read files read-only and never save over them.

| Part | Source | Notes |
|---|---|---|
| Carbon plate | The user's plate drawing (150 x 76 x 3 mm, centre hole Ø25, holes B/C/D, slots S1–S7) | `LineFollower2.3.FCStd` holds an older 150 x 56 plate; do not use it for the current robot. |
| Drive train | `<project folder>/FreeCad/models/LineFollower2.3.FCStd`: `n60_wheel_lock_L/R`, `N60_Motor`, screws, nuts | Lock mounting holes are 30 mm apart; on the current plate they sit on holes C (x = ±43.75). |
| Wheels | `Link_Wheel_Short7_L/R` → `Wheel_Short7.FCStd` (hub + 40T gear) plus `Tire_N60110_Short7_L/R` (rubber, Ø25) | The tyre is a separate part, hidden in the GUI; export it explicitly. |
| Side rails | `<project folder>/FreeCad/models/PulseLinefollower.FCStd` `Body001/Body002`: 100 x 6 mm | Mounted **under** the plate at x = ±72 with two black socket screws in the outer plate holes; the bar sits on top of the rail ends. |
| Sensor bar 16MuxV3 | `<project folder>/Kicad/16MuxV3/16MuxV3.kicad_pcb` | 150 x 27 mm, J1 = PH2.0 8-pin (bottom side), end pads H1–H8 at x = ±66/±72. The KiCad export comes upside down and back to front: rotate 180° about its long axis. |
| Main board PulseMini V2.2 | EasyEDA (Standard) project **PulseMiniV2.3** → `PCB_PulseMiniV2.3` | 75.7 x 29.7 mm, purple solder mask, Nano on top, eight vertical PH2.0-3P sockets on one long edge. On the robot the sockets face the sensor bar. Standoffs: white nylon M3 x 15 on plate holes B14/B15/B19/B20 (70 x 24). |
| Sensor loom | 8 wires from J1 into the board's PH2.0-3P sockets (3 + 3 + 2) | Ask which sockets are used if it matters. |
| Battery | 2S 350 mAh LiPo | Leave it out unless the user asks; size is not measured. |

Old V2.1 board (only if asked): `<project folder>/FreeCad/models/Pulse.FCStd`, group `EasyEDA_PCB_Model_0FP6`, 76 x 30 mm.

## Searching
- `find ~ -xdev -iname '*.kicad_pcb' -o -iname '*.FCStd' -o -iname '*.step'` finds files; then open assemblies and list their groups to find boards inside them.
- Check the user's saved notes, if available, for project folders. A note that a file "does not exist" can be wrong; search contents before saying so.
