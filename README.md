# Open Arms Mini

**Small, low-inertia leader arms for robot teleoperation**

<img src="images/openarm-mini2.jpg" width="480" alt="Open Arms Mini with wrist strap" />

---

## What is Open Arms Mini?

Open Arms Mini is a compact, Feetech-based, 3D-printed leader arm for robot teleoperation. It has 7 DOF with human-like kinematics — 3 shoulder joints, 1 elbow, 1 forearm rotation, and 2 wrist joints — plus a gripper. It is designed to be paired with the [Open Arms follower arm](https://github.com/huggingface/lerobot) and integrates directly with [LeRobot](https://github.com/huggingface/lerobot) as the `openarm_mini` teleoperator.

### Design goals

Open Arms Mini was designed around three requirements that matter for real teleoperation deployments:

- **Low inertia** — operators can make quick, deliberate motions that precision tasks like cloth folding demand
- **Arm-length agnostic** — wrist-grip design works for teleoperators of any height
- **Affordable** — ~150 EUR per arm makes it easy to set up multiple stations

### The wrist strap

The wrist strap makes a big difference for wrist roll precision — without it the hand tends to slip. With it, wrist control is solid. Highly recommended.

<img src="images/openarm-mini1.jpg" width="480" alt="Open Arms Mini paired with follower arm" />

---

## Bill of Materials

See [BOM.md](BOM.md) for the full bill of materials with links and prices.

**Approximate cost: ~150 EUR per arm / ~300 EUR for a pair** (excluding 3D printing filament and screws).

---

## 3D Printing

### Print settings

| Setting | Recommendation |
|---|---|
| Material | PLA or PETG |
| Layer height | 0.2 mm |
| Infill | 20–40% |
| Supports | Yes, where needed |
| Nozzle | 0.4 mm |

All parts print without post-processing beyond removing supports. A standard FDM printer with a ~220×220 mm bed is sufficient.

### Parts list

All STL files are in the [`STL/`](STL/) directory. STEP files (for modification) are in [`STEP/`](STEP/).

Quantities are for a **complete pair (left + right arm)**. Parts without an L/R suffix are universal — print 2, one per arm. The WaveShare mounting plate is shared between both arms (qty 1). **Total: 32 printed parts for a pair** (16 per arm).

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

### Hardware required

Per arm:

- 8× Feetech STS3215-C046 servo (7.4 V, 1:147 gear ratio)
- 1× Waveshare Serial Bus Servo Driver Board
- M2×6 mm screws (×~35)
- M3×6 mm screws (×~35)
- Motor horn screws M3×6 mm (×8, one per motor — usually included with motors)
- 3-pin servo cables (×8)
- 7.5 V DC power supply (2 A or more)
- 1× Velcro/elastic wrist strap (~25 mm wide)

### Step-by-step

#### Prepare motors

Before assembly, configure each motor's ID and baudrate using LeRobot (see [LeRobot Integration](#lerobot-integration) below). It is easier to do this before the motors are installed in the arm.

Connect each motor individually to the Waveshare board and run:

```bash
lerobot-setup-motors \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751
```

Follow the prompts to set motor IDs 1–8 in order.

#### Motor ID assignment

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

> **Note on joint ordering:** The wrist flex (joint 6) and wrist roll (joint 7) are deliberately swapped in software relative to the follower arm's joint numbering. This order feels more natural to operate because of how the human wrist moves. The swap is handled in the `openarm_mini` LeRobot driver — no manual remapping needed.

#### Assembly order

1. **Shoulder Pan (J1)** — Insert motor 1 into J1. Secure with 4× M2×6 screws. Attach J1_holder with 2× M2×6 screws. Install both motor horns; secure top horn with 1× M3×6 screw.
2. **Shoulder Lift (J2)** — Slide motor 2 into J2. Fasten with 4× M2×6 screws. Attach both motor horns with horn screw. Connect J2 to J1 with 4× M3×6 screws each side.
3. **Shoulder Roll (J3)** — Insert motor 3 into J3. Fasten with 4× M2×6 screws. Attach motor horns. Connect J3 to J2 with 4× M3×6 screws each side.
4. **Elbow (J4)** — Slide J4_holder over J3. Insert motor 4 and fasten with 4× M2×6 screws. Attach both motor horns with horn screw.
5. **Forearm Rotation (J5)** — Insert motor 5 into J5 and secure with 2× M2×6 front screws. Install one motor horn only. Secure J5 to motor 4 with 4× M3×6 screws each side.
6. **Wrist Flex (J6)** — Attach J6 to motor 5 horn with 4× M3×6 screws. Insert motor 6 and secure with 2× M2×6 screws. Attach both motor horns.
7. **Wrist Roll (J7)** — Attach J7 to motor 6 with J7_holder. Insert motor 7 and fasten with 4× M2×6 screws.
8. **Gripper** — Insert motor 8 into J8 L/R holders. Attach gripper claws with 4× M3×6 screws each side. Attach triggers (J trigger L, J trigger R).
9. **Handle** — Attach J Handle to the gripper body. Mount J6 holder with strap around the wrist area.
10. **Wrist strap** — Thread a 25 mm elastic velcro strap through the strap holder. Adjust for a snug fit.
11. **Controller board** — Mount the Waveshare board on the WaveShare_Mounting_Plate_SO101 and attach to the base. Daisy-chain all motors with 3-pin cables from motor 1 through to motor 8.

---

## LeRobot Integration

Open Arms Mini is natively integrated into [LeRobot](https://github.com/huggingface/lerobot) as a first-class teleoperator. The implementation lives at [`lerobot/teleoperators/openarm_mini`](https://github.com/huggingface/lerobot/tree/main/src/lerobot/teleoperators/openarm_mini).

### Install LeRobot

```bash
pip install lerobot[feetech]
```

### Find USB port

```bash
lerobot-find-port
```

### Configure motors

```bash
lerobot-setup-motors \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751
```

### Calibrate

```bash
lerobot-calibrate \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751 \
    --teleop.id=my_mini_leader_arm
```

### Teleoperate

Pair Open Arms Mini with an Open Arms follower arm:

```bash
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/tty.usbmodem585A0076841 \
    --robot.id=my_follower \
    --teleop.type=openarm_mini \
    --teleop.port=/dev/tty.usbmodem575E0031751 \
    --teleop.id=my_mini_leader_arm
```

Full instructions: [LeRobot SO-101 documentation](https://huggingface.co/docs/lerobot/so101)

LeRobot teleoperator source: [`lerobot/teleoperators/openarm_mini`](https://github.com/huggingface/lerobot/tree/main/src/lerobot/teleoperators/openarm_mini)

---

## License

Hardware designs released under [Apache 2.0](LICENSE). Based on the [SO-ARM100](https://github.com/TheRobotStudio/SO-ARM100) design.
