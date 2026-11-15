# ASTROVERSE — Player Gameplay Flow

## 1. High-Level Player Journey

The core gameplay loop of ASTROVERSE is structured as a guided, story-driven educational journey designed to be completed in approximately **15 to 25 minutes**:

```
[ Game Launch / Spawn ]
          │
          ▼
[ Welcome Screen & Field Journal ]
          │
          ▼
[ Mission 1: Power the Moon Base ] ───► [ Earn Badge 1: Power Pioneer ]
          │
          ▼
[ Mission 2: Keep Astronaut Safe ] ───► [ Earn Badge 2: Life Support Hero ]
          │
          ▼
[ Mission 3: Shield Our Base ] ───────► [ Earn Badge 3: Radiation Defender ]
          │
          ▼
[ Mission 4: Grow Food on Moon ] ─────► [ Earn Badge 4: Space Botanist ]
          │
          ▼
[ Mission 5: Space Weather Watch ] ───► [ Earn Badge 5: Master Astronaut ]
          │
          ▼
[ Graduation Certificate & Official Seal ]
          │
          ▼
[ Free-Roam Exploration: Rover Driving & Memorials ]
```

---

## 2. Step-by-Step Experience Walkthrough

### Phase 1: Arrival & Welcome (`WelcomeScreen`)
1. **Spawn:** The player spawns inside the airlock corridor of Moon Base Alpha.
2. **Camera Focus:** The camera smoothly eases onto the player's astronaut avatar in third-person view.
3. **Journal Presentation:** The `WelcomeScreen` card appears centered on screen:
   - **Title:** `ASTROVERSE 🚀` (Rendered in handwritten `Kalam` font).
   - **Subtitle:** `★ JUNIOR ASTRONAUT MISSION JOURNAL ★` (`PatrickHand`).
   - **Body:** *"Every great discovery begins with a curious explorer. Complete 5 lunar training missions on Moon Base Alpha to earn your official Junior Astronaut Explorer Badge!"*
   - **Action:** Player clicks the bright golden-yellow button: **`LET'S EXPLORE THE MOON 🌟`**.
4. **Initialization:**
   - Client fires `JuniorMissionRemotes.RequestStartTraining`.
   - Server initializes player profile attributes (`JuniorTrainingMode = true`, `JuniorCurrentMission = 1`).
   - InGameHUD unfolds smoothly, displaying the Mission 1 Tracker at top-left.

---

### Phase 2: The Core Mission Loop (Missions 1 to 5)

Each mission follows an intuitive, consistent 5-step loop:

```
  1. Mission Tracker Appears
           │
           ▼
  2. Follow 3D Navigational Beacon
           │
           ▼
  3. Reach Target & Hold Interaction Key [E]
           │
           ▼
  4. Mission Success Card & Badge Celebration
           │
           ▼
  5. Read NASA Science Fact & Advance
```

#### Step Details:
1. **Objective Tracker:** Displays current mission title, icon, and actionable prompt (e.g., *"Follow the yellow beacon outside to the solar arrays"*).
2. **3D World Beacon:** A glowing, pulsing holographic ring and vertical light beam appears at the target object in the world. A dynamic compass reticle on the player's HUD guides them toward the beacon if it is outside their screen view.
3. **Target Interaction:**
   - As the player steps within `TargetRadius` (`12` studs), a prompt card appears.
   - **Desktop:** Player presses and holds `[E]`.
   - **Mobile:** Player taps and holds the on-screen action button.
   - A circular sketchbook progress fill animates. If released early, the progress resets safely.
4. **Server Validation & Celebration:**
   - Upon completion of the required hold duration (`0.5s` to `1.8s`), the server validates proximity and fires `MissionStateUpdated` and `ShowCelebration`.
   - The `MissionSuccess` modal slides into view with a rotating gold starburst, playing a triumphant chime (`rbxassetid://6026987399`).
   - The player's newly awarded badge stamp is stamped into their journal.
5. **NASA Science Fact Card:**
   - Clicking *"READ SCIENCE FACT"* opens the `ScienceFactCard`.
   - Presents authentic educational context explaining how real NASA engineers solve that challenge (e.g., how the ISS recycles 98% of human moisture into potable water).
   - Clicking *"NEXT MISSION"* smoothly initializes the subsequent training objective.

---

### Phase 3: Scientific Rover Exploration

Upon completing early missions or during free exploration, the player can operate the **6-Wheeled Scientific Lunar Rover** parked outside the outpost airlock:

1. **Boarding:** Approaching the rover cockpit displays a billboard prompt: `Press [E] to Drive Rover`.
2. **Network Ownership:** Server delegates chassis physics to the client for zero-latency steering.
3. **HUD Transition:**
   - Astronaut walk HUD retracts.
   - `RoverDrivingUI` activates with speedometer gauge, headlight toggle, and on-screen touch pedals (forward, brake, reverse, steer left/right).
4. **Driving Dynamics:**
   - The rover navigates crater rims and regolith slopes with authentic lunar gravity (`32.4 studs/s²`).
   - Active downforce keeps tires firmly planted without bouncing uncontrollably.
   - Pressing `[H]` or `[F]` triggers the **Hover Boost** (5.5-second auxiliary thrust burst to clear jagged boulders).
5. **Safe Dismount:**
   - Pressing `[Backspace]`, `[X]`, or tapping `Exit Rover` triggers a downward raycast on the left side of the vehicle (`X = -4.2` studs).
   - The player character is safely spawned standing on solid terrain with zero residual inertia.

---

### Phase 4: Aerial Overview Camera

At any point during exploration, players and educators can inspect the base layout from above:
1. **Activation:** Press `[V]` on desktop, `[D-Pad Up]` on controller, or click the **Aerial View** button on the top-right HUD.
2. **Camera Tween:** The camera smoothly detaches from the player avatar and eases upward into a bird's-eye perspective (`Position: (4, 215, 65)`, `LookAt: (4, 15, 220)`) with a 70° Field of View.
3. **Overview HUD:** Moon Base Alpha, solar arrays, hydroponic greenhouses, and surrounding crater ridges are visible in a single tactical view.
4. **Return:** Clicking **`RETURN TO ASTRONAUT 👨‍🚀`** smoothly transitions the camera back to third-person follow mode.

---

### Phase 5: Graduation & The Explorer's Logbook

When Mission 5 (*Solar Weather Watch*) is successfully completed:
1. **Graduation Fanfare:** A special grand celebration plays with sound effects and star confetti.
2. **Certificate of Lunar Exploration:** The player's `JuniorLogbook` opens automatically to display:
   - **Header:** `★ CERTIFICATE OF LUNAR EXPLORATION ★`
   - **Program:** `NASA SPACE APPS • JUNIOR ASTRONAUT PROGRAM`
   - **Official Seal:** Embossed gold `🎖️ OFFICIAL LUNAR EXPLORER SEAL 🎖️`.
   - **Astronaut Name:** Dynamically rendered with the player's Roblox username.
   - **Badge Grid:** All 5 earned badges displayed in full color:
     - ☀️ `Power Pioneer`
     - 💨 `Life Support Hero`
     - 🛡️ `Radiation Defender`
     - 🌱 `Space Botanist`
     - ⚡ `Master Junior Astronaut`
3. **Permanent Progress:** The certificate and badges are permanently recorded in the player's server DataStore profile.
4. **Open Sandbox:** Players can re-visit historical landmarks (Apollo 11 Memorial and Tiranga Memorial), drive the rover across the vast crater basin, and inspect geological targets freely.

---

## 3. Related Diagrams

- [Full Gameplay Flowchart](diagrams/gameplay-flow.md)
- [Mission Progression State Diagram](diagrams/mission-progression.md)
