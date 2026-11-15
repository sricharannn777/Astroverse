# ASTROVERSE — Controls & User Interaction Guide

## 1. Input Philosophy & Kid-Friendly Ergonomics

ASTROVERSE is engineered to be instantly playable for children aged **7 to 12** on whatever hardware they have available—from low-end mobile phones and tablets to desktop gaming PCs.

Key interaction guidelines implemented across all control schemes:
- **No Complex Key Combos:** Single keys or taps perform all actions.
- **Visual Hold Feedback:** Multi-second interactions display a circular progress fill, giving immediate visual cues on how long to hold.
- **Accidental Release Protection:** Releasing an interaction key early cancels the action safely without penalty, allowing children to retry immediately.
- **Generous Touch Hitboxes:** Mobile buttons use a minimum size of `64x64` to `80x80` pixels with distinct color coding (Green = Accelerate, Red = Brake, Yellow = Action).

---

## 2. Desktop Controls (Keyboard & Mouse)

### 2.1. Astronaut Navigation & Exploration
| Action | Keybinding | Notes |
| :--- | :---: | :--- |
| **Move Forward** | `W` or `Up Arrow` | Standard character movement. |
| **Move Backward** | `S` or `Down Arrow` | Standard character movement. |
| **Strafe Left** | `A` or `Left Arrow` | Standard character movement. |
| **Strafe Right** | `D` or `Right Arrow` | Standard character movement. |
| **Lunar Jump** | `Spacebar` | High-arcing, low-gravity leap (`JumpPower = 25.0`, `Gravity = 32.4`). |
| **Sprint / Fast Walk** | `Left Shift` | Increases walking speed by 40% with subtle FOV widening. |
| **Interact / Hold** | `E` | Hold key until circular progress bar completes (`0.5s` to `1.8s`). |
| **Aerial Overview Camera** | `V` | Toggles top-down bird's-eye view of Moon Base Alpha. |
| **Camera Zoom** | `Mouse Wheel` | Zoom in to first-person or out to wide third-person. |
| **Camera Orbit** | `Right Mouse Button (Hold)` | Pan and rotate perspective around astronaut. |

### 2.2. Scientific Lunar Rover Driving
| Action | Keybinding | Notes |
| :--- | :---: | :--- |
| **Accelerate / Forward** | `W` or `Up Arrow` | Smooth forward drive up to `18.85 studs/s`. |
| **Reverse** | `S` or `Down Arrow` | Reverses vehicle at `1.78 studs/s`. |
| **Steer Left** | `A` or `Left Arrow` | Front wheel left turn (`-53.02°/s` yaw). |
| **Steer Right** | `D` or `Right Arrow` | Front wheel right turn (`+53.02°/s` yaw). |
| **Handbrake** | `Spacebar` or `P` | Engages `32,000 N·m` brake torque; stops rover instantly. |
| **Toggle Headlights** | `L` | Toggles high-intensity dual LED light beams. |
| **Hover Boost** | `H` or `F` | 5.5s auxiliary propulsion boost (12s cooldown). |
| **Exit Rover** | `Backspace` or `X` | Safe raycast dismount to the left of the vehicle (`X = -4.2`). |

---

## 3. Mobile & Tablet Controls (Touch Screen)

The mobile user interface automatically adapts when touch input is detected:

```
+-------------------------------------------------------------------------+
| [Mission Tracker]                              [Aerial] [Logbook] [?]   |
|                                                                         |
|                                                                         |
|                                                                         |
|   (Virtual Thumbstick)                        [ TAP / HOLD [E] ]        |
|      Movement                                     Action                |
|                                                                         |
|                                                     [ JUMP ]            |
+-------------------------------------------------------------------------+
```

### 3.1. On-Foot Touch Layout
- **Movement:** Dynamic virtual thumbstick on the lower-left corner of the screen.
- **Jump:** Large dedicated round button on lower-right.
- **Interact / Hold:** Context-sensitive action button appears on screen when within 12 studs of a mission target. Hold with your thumb until the ring fills.

### 3.2. Rover Driving Touch Layout (`RoverDrivingUI`)
When seated in the Lunar Rover, the UI switches to an authentic dual-pad vehicle cluster:

```
+-------------------------------------------------------------------------+
| [TopBar: Lights | Hover Boost | Exit Rover]                             |
|                                                                         |
|                                                                         |
|   [LeftPad]                                             [RightPad]      |
|    <-- [Steer L]                                        [FORWARD  ▲]    |
|        [Steer R] -->                                    [BRAKE    ■]    |
|                                                         [REVERSE  ▼]    |
+-------------------------------------------------------------------------+
```

- **Left Pad:**
  - `BtnSteerLeft`: Steer left.
  - `BtnSteerRight`: Steer right.
- **Right Pad:**
  - `BtnForward` (Green): Drive forward.
  - `BtnBrake` (Red): Emergency handbrake.
  - `BtnReverse` (Yellow/Orange): Reverse.
- **Top Bar:**
  - `BtnLights`: Headlights on/off.
  - `BtnHoverBoost`: Activate hover boost thrusters.
  - `BtnExit`: Safe dismount onto lunar terrain.

---

## 4. Gamepad & Controller Mappings (Xbox / PlayStation)

ASTROVERSE supports native gamepad controllers via Roblox `UserInputService`:

| Gamepad Button | Astronaut Mode | Rover Driving Mode |
| :--- | :--- | :--- |
| **Left Thumbstick** | Character Move (360°) | Steering (Left / Right) |
| **Right Thumbstick** | Camera Orbit | Camera Orbit |
| **A / Cross (✕)** | Jump | Handbrake / Emergency Stop |
| **X / Square (□)** | Interact / Hold (Mission action) | Toggle Headlights |
| **Y / Triangle (△)** | Open Logbook & Badges | Exit Rover (Safe Dismount) |
| **Right Trigger (R2)** | Sprint | Forward Throttle |
| **Left Trigger (L2)** | — | Reverse / Brake |
| **Left Bumper (L1)** | — | Hover Boost Thrusters |
| **D-Pad Up** | Toggle Aerial Overview Camera | Toggle Aerial Overview Camera |
| **Select / View** | Open Help & Settings | Open Help & Settings |

---

## 5. UI Navigation & Accessibility

- **Sketchbook Journal Access:** Clicking the journal icon at the bottom of the HUD opens the player's logbook at any time to review earned badges and the Certificate of Lunar Exploration.
- **Audio Feedback:** Every button click, hold completion, and rover mode change is accompanied by crisp, tactile audio cues:
  - Pencil sketch scribble (`rbxassetid://6026987399`)
  - Mechanical switch click (`rbxassetid://9114223170`)
  - Outpost airlock hiss (`rbxassetid://9114221580`)
- **Visual Contrast:** High contrast text (Deep Navy `#16223E` on Cream `#F8F4E8`) compliant with WCAG AA legibility guidelines for young readers.
