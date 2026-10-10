# CNC Auto Door Retrofit

**Retrofit mechanism that automates a horizontally sliding CNC machine door using a compliant friction roller on the door's top edge. Sized for a 21.5 kg door and a 5 s open/close cycle.**

<table>
<tr>
<td width="50%" valign="top">

![CNC door drive mechanism, isometric view](images/assm%20main.png)

</td>
<td width="50%" valign="top">

![Roller module with bevel gears and clamp slot](images/assm%20iso%202.png)

</td>
</tr>
</table>

- **Role:** Sole designer
- **Software:** Onshape
- **Context:** Take-home design challenge for a CNC automation company
- **Status:** Design exercise. CAD, manufacturing drawings complete. Not built or tested.

---
## Overview

This repository contains a design for a modular retrofit that opens and closes a single horizontally sliding door on a CNC machine. A silicone roller presses on grip tape along the top edge of the door and drives it. The module has a fixed size, so the same design works for any door length.

---
## Problem and requirements

The task was to automate a door that slides strictly horizontally, without pivoting like a van door. The key requirements were:

- A door mass of 21.5 kg
- A stroke of up to 1600 mm (the worst case)
- A 5 s open or close time
- A specified encoder-equipped goBILDA 5203 gear motor (19.2:1, 312 RPM)
- A mechanism that mounts above the door and can be fitted to different machines

---
## Engineering assumptions

1. The door rolls on a fixed bottom rail with linear roller bearings.
2. The environment is not ideal. Airborne dust, metal chips, lubricants, coolants and sweat are all expected.
3. The door has a flat top surface that is accessible along its full length.
4. The door is sturdy enough to take some downward force on its top surface, which the clamp needs.
5. The top of the machine is flat and allows the mechanism to be bolted or clamped on.

---
## Concepts considered

| Concept | Verdict | Reason |
|---|---|---|
| Friction roller on grip tape | Selected | The mechanism is a fixed size and only the length of tape changes with the door length. |
| Rack and pinion | Rejected | Very long racks are impractical, and the design limits scalability and the ability to drive double doors. |
| Lead screw | Rejected | Long lead screws are costly and the assembly is bulky. |
| Belt drive | Rejected | It needs a non-standard belt loop, belt tensioning and a torque transfer mechanism. These are costly, bulky, and harder to assemble and maintain. |

---
## Selected design

### How it works

The motor drives a bevel gear stage that turns the drive through 90 degrees, so the motor lies along the door and the module stays compact. The output drives a silicone roller that runs on grip tape stuck to the top edge of the door. The module fits in 220 x 160 x 105 mm.

### Standoff adjustment

The roller sits in a 90 degree circular slot and swings vertically through 45 mm. This lets it reach the top edge of the door on machines with different mounting heights. The roller is powered by a train of helical spur gears which are spaced 45 mm apart so they stay in mesh within the slot. If a machine's mounting surface is more than 45 mm from the top of the door, the slot must be changed.

### Clamping

A cutout in the guide slot lets a bolt clamp the roller down onto the top surface of the door. The clamp force sets the grip.

## Calculations

### Inputs

| Parameter | Value | Note |
|---|---|---|
| Door mass | 21.5 kg | |
| Stroke | 1600 mm | Worst case |
| Cycle time | 5 s | 320 mm/s average |
| Motion profile | 0.8 s accel, 3 s cruise, 1.2 s decel | |
| Door rail friction | 0.003 to 0.005 | Conservative estimate for a well lubricated rail |
| Roller to tape friction | 1.05 to 1.35 | Manufacturer data for anti slip tape |
| Transmission efficiency | 85% | Conservative estimate |
| Motor | goBILDA 5203, 312 RPM, max torque 24.3 kgcm (2.38 N·m) | |

### Motion profile

The peak speed follows from the distance covered. The distance is v × (0.8/2 + 3 + 1.2/2) = 4.0 s × v = 1600 mm, so v = 400 mm/s.

| Phase | Duration | Acceleration | Distance |
|---|---|---|---|
| Accelerate | 0.8 s | 0.50 m/s² | 160 mm |
| Cruise | 3.0 s | 0 | 1200 mm |
| Decelerate | 1.2 s | 0.33 m/s² | 240 mm |

### Drive force at the roller

The worst case uses the upper rail friction value of 0.005, so rail friction is 21.5 × 9.81 × 0.005 = 1.05 N.

| Phase | Force at roller |
|---|---|
| Accelerate | 21.5 × 0.50 + 1.05 = 11.8 N |
| Cruise | 1.1 N |
| Decelerate (braking) | 21.5 × 0.33 − 1.05 = 6.1 N |

The peak is 11.8 N, during acceleration.

### Clamp force

To transmit 11.8 N without slipping at the lowest tape friction of 1.05, the roller needs a normal force of at least 11.8 / 1.05 = 11.2 N.

### Motor torque

Roller torque is the peak force times the roller radius. Motor torque is that divided by the efficiency and the bevel stage ratio.

| Quantity | Value |
|---|---|
| Roller radius | 40 mm |
| Bevel stage ratio | 2:3 |
| Required roller torque | 11.8 N × 0.04 m = 0.472 N·m |
| Required motor torque | 0.472 / (0.85 × 0.667) = 0.833 N·m |
| Available motor torque | 2.38 N·m |
| Margin | 1.57 N.m |

---
## Scalability

The roller rolls along the top of the door whatever its length, so only the length of tape changes between machines.

---
## Safety and maintenance

- The grip tape and roller wear and need periodic replacement. This is like a skateboard wheel, where even a worn wheel still grips the road.
- The clamping force can drop over time through Hertzian contact stress if the top of the door is not very flat, so the bolt needs periodic retightening.
- The enclosure prevents anything from interfering with the gears but the mechanism needs periodic cleaning

---
## Limitations and open items

- Slip and tape wear are inherent to a friction drive and are reduced, not removed, by the clamp.
- The design has not been built or tested, so grip and wear figures are estimates.
- Obstruction detection and fallbacks must be implemented in software

## Repository contents

| Item | Description |
|---|---|
| `docs/design_report.pdf` | Design report with concept selection and calculations |
| `3D_files/` | STEP assembly and gears |
| `images/` | Renders used in this README |

## AI usage

Generative AI was used to help with ideation, to help assume constants such as sizing and friction values, to generate concept sketches, and to proofread. It has only assisted the design process and was not the main designer.
