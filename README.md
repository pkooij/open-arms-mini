# Open Arms Mini

**Small, low-inertia leader arms for robot teleoperation**

<img src="images/openarm-mini2.jpg" width="480" alt="Open Arms Mini with wrist strap" />

---

## What is Open Arms Mini?

Open Arms Mini is a compact, Feetech-based, 3D-printed leader arm for robot teleoperation. It has 7 DOF with human-like kinematics — 3 shoulder joints, 1 elbow, 1 forearm rotation, and 2 wrist joints — plus a gripper. Designed to be paired with the Open Arms follower arm and integrates with [LeRobot](https://github.com/huggingface/lerobot) as the `openarm_mini` teleoperator.

- **Low inertia** — quick, deliberate motions for precision tasks like cloth folding
- **Arm-length agnostic** — wrist-grip design works for teleoperators of any height
- **Affordable** — ~150 EUR per arm, ~300 EUR for a pair

### The wrist strap

The wrist strap makes a big difference for wrist roll precision — without it the hand tends to slip. Highly recommended.

<img src="images/openarm-mini1.jpg" width="480" alt="Open Arms Mini paired with follower arm" />

---

## Bill of Materials

See [BOM.md](BOM.md) for full details. **~150 EUR per arm / ~300 EUR for a pair** (excl. filament and screws).

---

## 3D Printing

| Setting | Recommendation |
|---|---|
| Material | PLA or PETG |
| Layer height | 0.2 mm |
| Infill | 20–40% |
| Supports | Yes, where needed |
| Nozzle | 0.4 mm |

STL files: [`STL/`](STL/) — STEP files (for modification): [`STEP/`](STEP/)

Quantities are for a **complete pair (left + right arm)**. Universal parts (no L/R) = 2, gripper L/R parts = 1 each, WaveShare plate shared = 1. **Total: 32 parts for a pair** (16 per arm).

| File | Description | Qty (pair) |
|---|---|---|
| `J1 v5.stl` | Shoulder pan joint | 2 |
| `J1_holder v1.stl` | Shoulder pan motor holder | 2 |
| `J2 v2.stl` | Shoulder lift joint | 2 |
| `J2_holder v1.stl` | Shoulder lift motor holder | 2 |
| `J3 v2.stl` | Shoulder roll joint | 2 |
| `J4 v3.stl` | Elbow joint | 2 |
| `J4_holder v1.stl` | Elbow motor holder | 2 |
| `J5 v4.stl` | Forearm rotation joint | 2 |
| `J6 v6.stl` | Wrist flex joint | 2 |
| `J6 holder with strap v4.stl` | Wrist strap holder | 2 |
| `J7 v4.stl` | Wrist roll joint | 2 |
| `J7_holder v1.stl` | Wrist roll motor holder | 2 |
| `J Handle v12.stl` | Main handle | 2 |
| `J8 L v4.stl` | Gripper claw — left | 1 |
| `J8 R v10.stl` | Gripper claw — right | 1 |
| `J8 holder L v2.stl` | Gripper claw holder — left | 1 |
| `J8 holder R v6.stl` | Gripper claw holder — right | 1 |
| `J trigger L v2.stl` | Gripper trigger — left | 1 |
| `J trigger R v2.stl` | Gripper trigger — right | 1 |
| `WaveShare_Mounting_Plate_SO101 v1.stl` | WaveShare controller mounting plate (shared) | 1 |
| `arducam_holder v6.stl` | Arducam camera mount (optional) | — |

---

## Assembly

**Per arm:** 8× Feetech STS3215-C046 servo (7.4 V, 1:147), 1× Waveshare Serial Bus Servo Driver Board, M2×6 and M3×6 screws (~35 each), 8× 3-pin servo cables, 7.5 V power supply, 1× velcro wrist strap (~25 mm).

### Motor IDs

Configure motors before assembly (easier when not yet installed):

```bash
lerobot-setup-motors \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751
```

| ID | Joint | Part |
|---|---|---|
| 1 | Shoulder Pan | J1 |
| 2 | Shoulder Lift | J2 |
| 3 | Shoulder Roll | J3 |
| 4 | Elbow Flex | J4 |
| 5 | Forearm Rotation | J5 |
| 6 | Wrist Flex | J6 |
| 7 | Wrist Roll | J7 |
| 8 | Gripper | J8 L/R |

> **Joint swap note:** Wrist flex (6) and wrist roll (7) are deliberately swapped in software relative to the follower arm — this feels more natural given how the human wrist moves. Handled automatically by the `openarm_mini` driver.

### Assembly order

1. **Shoulder Pan (J1)** — Insert motor 1, secure with 4× M2×6. Attach J1_holder with 2× M2×6. Install motor horns, secure top horn with M3×6.
2. **Shoulder Lift (J2)** — Slide motor 2 in, fasten with 4× M2×6. Attach motor horns. Connect J2 to J1 with 4× M3×6 each side.
3. **Shoulder Roll (J3)** — Insert motor 3, fasten with 4× M2×6. Attach motor horns. Connect J3 to J2 with 4× M3×6 each side.
4. **Elbow (J4)** — Slide J4_holder over J3. Insert motor 4, fasten with 4× M2×6. Attach motor horns.
5. **Forearm Rotation (J5)** — Insert motor 5, secure with 2× M2×6. Install one motor horn. Secure J5 to motor 4 with 4× M3×6 each side.
6. **Wrist Flex (J6)** — Attach J6 to motor 5 horn with 4× M3×6. Insert motor 6, secure with 2× M2×6. Attach motor horns.
7. **Wrist Roll (J7)** — Attach J7 to motor 6 with J7_holder. Insert motor 7, fasten with 4× M2×6.
8. **Gripper** — Insert motor 8 into J8 holders. Attach J8 L/R claws with 4× M3×6 each side. Attach triggers.
9. **Handle & strap** — Attach J Handle. Mount J6 holder with strap at the wrist. Thread velcro strap and adjust for a snug fit.
10. **Controller board** — Mount Waveshare board on WaveShare_Mounting_Plate. Daisy-chain motors 1→8.

---

## LeRobot Integration

Natively integrated into [LeRobot](https://github.com/huggingface/lerobot) — source: [`lerobot/teleoperators/openarm_mini`](https://github.com/huggingface/lerobot/tree/main/src/lerobot/teleoperators/openarm_mini).

```bash
pip install lerobot[feetech]
```

```bash
# Find port
lerobot-find-port

# Calibrate
lerobot-calibrate \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751 \
    --teleop.id=my_mini_leader_arm

# Teleoperate
lerobot-teleoperate \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751 \
    --teleop.id=my_mini_leader_arm
```

---

## License

Hardware designs released under [Apache 2.0](LICENSE). Based on the [SO-ARM100](https://github.com/TheRobotStudio/SO-ARM100) design.
