# ASTROVERSE — Core Game Systems

## 1. Lunar Rover Vehicle Simulation

The **Scientific Lunar Rover** is a custom-engineered 6-wheel rover designed specifically for lunar exploration under realistic low gravity (`32.4 studs/s²`).

```
                    [ 6-Wheel Scientific Lunar Rover ]
                    
       [ Front Steer Wheels ]  [ Mid Drive Wheels ]  [ Rear Drive Wheels ]
       Left:  Wheel_FL         Wheel_ML              Wheel_RL
       Right: Wheel_FR         Wheel_MR              Wheel_RR
                                      ▲
                                      │
              [ Central Chassis with Dynamic Downforce & Gyro Leveling ]
```

### 1.1. Physical Architecture & Suspension Tuning
- **Chassis Structure:** Built with an unanchored main frame (`Chassis`), cockpit windshield, front sensor pod, and roll cage.
- **Seat Physics:** `DriverCockpit.DriverSeat.CanCollide` is set to `false`. This prevents physics collision overlap between the astronaut avatar and the vehicle frame, completely eliminating avatar-seat physics jitter.
- **Independent 6-Wheel Suspension:** Each of the 6 wheels is attached via a `CylindricalConstraint` and an independent `SpringConstraint`:
  - **Spring Stiffness:** `9000` N/m
  - **Spring Damping:** `2200` N·s/m
  - **Free Length:** `1.2` studs (`MinLength: 0.4`, `MaxLength: 1.8`)
  - **Result:** Excellent bump absorption across rugged crater boulders without bottoming out or bouncing erratically.

### 1.2. Dynamic Regolith Downforce & Low-Gravity Handling
In standard Roblox gravity (`196.2 studs/s²`), wheeled vehicles remain grounded by weight. In lunar gravity (`32.4 studs/s²`, 1/6th Earth), standard vehicles bounce into space upon hitting small rocks.
- **Active Downforce (`Chassis.HoverForce`):** While driving on the surface, a downward `VectorForce` is dynamically calculated based on forward speed:
  $$\text{Downforce} = 500 + \left( \frac{\text{CurrentSpeed}}{\text{MaxSpeed}} \times 1900 \right) \quad [500\text{ N to } 2400\text{ N}]$$
- **Low Center of Mass (CoM):** An internal invisible ballast part (`CoM_Stabilizer`) is positioned below the axle line, creating an inherently self-righting pendulum effect.

### 1.3. Dual-Stage Gyroscopic Anti-Flip Recovery
To prevent children from getting stuck upside down while exploring:
1. **Stage 1 (Proportional Leveling):** If vehicle roll exceeds **18°** or pitch exceeds **22°**, proportional corrective torque is applied to gently stabilize the chassis.
2. **Stage 2 (Auto-Righting Recovery):** If extreme rollover occurs (roll or pitch > **45°**), an automated uprighting torque activates, rotating the rover right-side-up and settling it onto its wheels.

### 1.4. Performance Benchmarks (Empirically Verified in Studio QA)
- **Forward Acceleration:** Reaches `18.85 studs/s` smoothly without wheel slip (covering 17.2 studs in 1.5 seconds).
- **Steering Agility:** Delivers `-53.02°` yaw rotation per second for responsive turning around boulders.
- **Reverse Speed:** Stable reverse maneuvering at `1.78 studs/s`.
- **Handbrake Stop:** Active brake torque (`32,000 N·m`) stops the rover from top speed in under 1 second (`< 0.98 studs/s`).
- **Static Resting Attitude:** Zero drift on 12° lunar slopes (Roll: `1.62°`, Pitch: `-11.98°`).

### 1.5. Hover Boost Capability
- **Duration:** `5.5` seconds of auxiliary vertical and forward propulsion.
- **Cooldown:** `12.0` seconds enforced authoritatively by `ServerScriptService.RoverService`.
- **Purpose:** Allows the rover to glide over impassable chasms, steep crater lips, and boulder fields.

### 1.6. Safe Dismount Raycasting
Exiting a vehicle in physics games often causes the avatar to spawn inside wheels or fly into the air:
- When the player presses `[Backspace]` or taps `Exit Rover`, the server executes a vertical downward raycast starting at an offset of **4.2 studs left** of the driver's seat (`X = -4.2`).
- The player character is positioned precisely at the raycast intersection point with the terrain surface (`Y + 3.0` studs).
- The player's linear and angular velocities are zeroed (`AssemblyLinearVelocity = Vector3.zero`).
- Normal walking physics are restored (`JumpPower = 25.0`, `Sit = false`, `PlatformStand = false`).

---

## 2. Aerial Overview Camera System

The **Aerial Overview Camera** allows players and educators to transition smoothly from third-person astronaut perspective to a tactical bird's-eye view of Moon Base Alpha.

```
       Camera Target: (4, 215, 65)  [ Alt: 215 studs ]
               \
                \  70° Field of View
                 \
                  ▼
       Focus Point: (4, 15, 220)  [ Moon Base Alpha Center ]
```

### 2.1. Coordinate & Transition Specifications
- **Overview Camera CFrame:** `CFrame.new(4, 215, 65, -1, 0, 0, 0, 0.612, 0.790, 0, 0.790, -0.612)`
- **LookAt Target:** `Vector3.new(4, 15, 220)` (Focuses directly on the central habitat hub, solar array corridor, and airlock).
- **Field of View (FOV):** `70°` (Encompasses the entire base within a 16:9 viewport).
- **Enter Duration:** `1.5` seconds (`Enum.EasingStyle.Cubic`, `Enum.EasingDirection.InOut`).
- **Exit Duration:** `1.2` seconds (`Enum.EasingStyle.Cubic`, `Enum.EasingDirection.InOut`).
- **Input Triggers:**
  - Desktop: Keyboard `[V]`.
  - Gamepad: `[D-Pad Up]`.
  - UI: Top-right `Aerial View` button.

---

## 3. Geological Target Registry & Rock Formations

Moon Base Alpha is situated in an authentic crater field surrounded by highland massifs:

### 3.1. Target Registry (`GeologicalTargetRegistry.lua`)
- Maintains a registry of exactly **41 geological exploration targets** across the basin, crater rim, and highland ridge.
- Targets support scientific rock sampling, spectrometer scanning, and mineral classification.

### 3.2. Cleaned Environment & Obstruction Removal
During Phase 8E, three specific pillar-like stone obstacles that obstructed the lunar rover's primary driving lane were cleanly removed:
- Removed: `RockFormations.Basin.BasinRock_2`
- Removed: `RockFormations.Basin.BasinRock_5`
- Removed: `CurvedTerrainSection.Rocks.Stone_9_Curved`
- All remaining natural geology is intact:
  - `Basin` retains `BasinRock_1`, `BasinRock_3`, `BasinRock_4`.
  - `CurvedTerrainSection.Rocks` retains all 15 natural curved boundary rocks.
  - Driving lanes between the airlock, memorials, and solar field are 100% clear.

---

## 4. Lunar Outpost Architecture

Moon Base Alpha comprises interconnected modular pressure vessels built for extreme thermal shifts (-130°C to +120°C):
- **Central Habitat Dome:** Core crew quarters, briefing tables, and mission logbook terminal.
- **Airlock Module:** Dual-door pressure lock with functional cycling protocol between habitat and vacuum.
- **Science & Lab Modules:** Houses mineral spectrometers, centrifuges, and specimen archives.
- **Greenhouse Bio-Dome:** Sealed agricultural environment with tiered LED grow trays and nutrient reservoirs.
- **Power Grid Pod:** Dual tracking photovoltaic solar arrays connected to transformer stations.
- **Communications Tower:** High-gain parabolic antenna aligned toward Earth.

---

## 5. Historic & Cultural Memorials

ASTROVERSE includes two historically significant lunar landing memorials:

### 5.1. Apollo 11 Memorial (1969)
- **Historical Context:** Commemorates the historic Apollo 11 mission (Neil Armstrong and Buzz Aldrin, July 20, 1969).
- **In-Game Assets:** Features a commemorative replica of the lunar descent stage, an American flag, and the historic plaque reading: *"Here men from the planet Earth first set foot upon the Moon, July 1969 A.D. We came in peace for all mankind."*

### 5.2. Tiranga Lunar Memorial (Chandrayaan-3, 2023)
- **Historical Context:** Commemorates the Indian Space Research Organisation (ISRO) Chandrayaan-3 mission, which achieved humanity's first successful soft landing in the Moon's South Pole region on August 23, 2023 (now celebrated as National Space Day).
- **In-Game Assets:** Features a commemorative lunar lander model, the Indian Tricolor flag (Tiranga), and an educational marker highlighting international space exploration milestones.
