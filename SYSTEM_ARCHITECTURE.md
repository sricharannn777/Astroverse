# ASTROVERSE — System Architecture

## 1. Architectural Philosophy

ASTROVERSE is engineered on Roblox's **Client-Server Distributed Architecture** using modern **Luau**. The architecture strictly adheres to three fundamental engineering principles:

1. **Server Authority & Anticheat Security:** All game state transitions, mission objective completions, NASA data caching, and DataStore profile modifications are authoritatively validated on the server. The client cannot forge mission completions or badge awards.
2. **Client Predictability & Responsive UX:** User inputs, camera transitions, sound effects, particle feedback, and UI animations execute immediately on the client using tweens and spring algorithms to guarantee smooth 60 FPS responsiveness across PC and mobile devices.
3. **Graceful Failover & Network Resilience:** Network operations (such as external calls to the NASA CCMC DONKI API or cloud DataStore queries) are decoupled from the main game loop, featuring non-blocking asynchronous threads, bounded caches, and seamless offline mock fallbacks.

---

## 2. Global System Hierarchy

```
DataModel (game)
│
├── ServerScriptService (Server-Authoritative Execution)
│   ├── JuniorMissionService.lua       -- Mission progression, objective validation, badge granting
│   ├── NASADataService.lua            -- CCMC DONKI API client, caching & historical fallback
│   ├── PersistenceService.lua         -- DataStore lifecycle, 300s session lock, autosave
│   ├── PersistenceLoader.lua          -- Safe bootstrap for persistence layer
│   ├── RoverService.lua               -- Network ownership delegation & hover boost timer
│   ├── PowerGridSystem.lua            -- Outpost electrical bus state management
│   ├── OxygenSystem.lua               -- Habitat life support & atmospheric pressure
│   ├── AirlockSystem.lua              -- Cycling protocol & player transit
│   └── WorldSafetyService.lua         -- Void rescue & boundary enforcement
│
├── ReplicatedStorage (Shared Modules, Remotes & Configurations)
│   ├── JuniorMissionConfig.lua        -- Mission metadata, coordinates, hold durations, facts
│   ├── JuniorSketchbookTheme.lua      -- Design tokens: cream paper, colors, fonts, card styles
│   ├── JuniorAerialCameraConfig.lua   -- Camera coordinates, FOV, cubic tween durations
│   ├── GeologicalTargetRegistry.lua   -- 41 registered sample collection markers
│   │
│   ├── JuniorMissionRemotes/          -- Remotes for mission workflow
│   │   ├── RequestStartTraining       -- RemoteFunction (Client -> Server)
│   │   ├── RequestMissionAction       -- RemoteFunction (Client -> Server)
│   │   ├── GetMissionState            -- RemoteFunction (Client -> Server)
│   │   ├── GetSpaceWeather            -- RemoteFunction (Client -> Server)
│   │   ├── MissionStateUpdated        -- RemoteEvent (Server -> Client)
│   │   └── ShowCelebration            -- RemoteEvent (Server -> Client)
│   │
│   ├── RoverRemotes/                  -- Remotes for rover interactions
│   │   ├── RequestBoard               -- RemoteEvent (Client -> Server)
│   │   ├── ExitRover                  -- RemoteEvent (Client -> Server)
│   │   ├── ToggleHeadlights           -- RemoteEvent (Client -> Server)
│   │   ├── SetHandbrake               -- RemoteEvent (Client -> Server)
│   │   ├── ActivateHoverBoost         -- RemoteEvent (Client -> Server)
│   │   └── HoverStateChanged          -- RemoteEvent (Server -> Client)
│   │
│   └── PersistenceRemotes/            -- Remotes for player save data
│       ├── GetPersistenceStatus       -- RemoteFunction (Client -> Server)
│       ├── SavePreferences            -- RemoteFunction (Client -> Server)
│       └── PreferencesLoaded          -- RemoteEvent (Server -> Client)
│
├── StarterPlayer / StarterPlayerScripts (Client Controllers)
│   ├── JuniorClientController.lua     -- UI state machine, action hold bar, compass beacons
│   ├── RoverClientController.lua      -- Vehicle input handling, pedal GUI, anti-flip leveling
│   ├── SprintControls.lua             -- Astronaut stamina & dynamic field-of-view
│   └── AudioClientController.lua      -- Atmospheric lunar wind, mechanical clicks, audio ducking
│
└── StarterGui (User Interfaces)
    ├── JuniorHUD                      -- Master Sketchbook ScreenGui
    │   ├── WelcomeScreen              -- Cream paper introductory journal card
    │   ├── InGameHUD                  -- Objective badge, top tracker, beacon compass
    │   ├── MissionBriefing            -- Detailed mission introduction card with science tips
    │   ├── MissionSuccess             -- Celebration card with earned badge & seal
    │   ├── ScienceFactCard            -- Deep-dive educational NASA knowledge modal
    │   ├── JuniorLogbook              -- Graduation Certificate & Badges Grid modal
    │   ├── HelpSettings               -- How-to-play instructions & audio volume sliders
    │   └── AerialOverviewUI           -- Bird's-eye camera HUD with return button
    └── RoverDrivingUI                 -- Mobile touch pedal cluster, gear, and telemetry
```

---

## 3. Subsystem Deep Dives

### 3.1. Junior Mission Service (`JuniorMissionService.lua`)
- **Location:** `ServerScriptService.JuniorMissionService`
- **Scope:** Server-authoritative state engine managing player training progression.
- **Key Responsibilities:**
  - Tracks player progress using player instance attributes (`JuniorCurrentMission`, `JuniorTrainingMode`, `JuniorMissionsCompleted`).
  - Listens to `RequestStartTraining` RemoteFunction. Initializes the player profile to Mission 1.
  - Listens to `RequestMissionAction(missionId)`. Validates that the player's character is within `TargetRadius` (typically `12` to `15` studs) of `TargetPosition`.
  - Enforces the server-side action hold duration (`HoldDuration`) before registering completion.
  - Grants mission completion badges and synchronizes state across clients via `MissionStateUpdated`.

### 3.2. NASA Space Weather Provider (`NASADataService.lua`)
- **Location:** `ServerScriptService.NASADataService`
- **Scope:** Dedicated server module interfacing with NASA's open science infrastructure.
- **Key Responsibilities:**
  - Executes HTTPS GET requests to the official CCMC DONKI endpoint:  
    `https://ccmc.gsfc.nasa.gov/DONKI-API/get/FLR?startDate=...&endDate=...`
  - Implements an in-memory cache with a **30-minute Time-To-Live (`CACHE_TTL_SECONDS = 1800`)** to respect NASA server load limits.
  - In offline Studio test sessions or when HTTP requests fail/timeout, gracefully returns a curated, authentic historical Solar Cycle 25 observation:
    - **Instrument:** `GOES-16 Satellite (SUVI)`
    - **Active Region:** Sunspot Region AR3664
    - **Classification:** `Class M2.5 Solar Flare` (Peak: `2024-05-10 12:45 UTC`)
  - Ensures zero client exposure of raw tokens or web infrastructure.

### 3.3. Session-Hardened Persistence (`PersistenceService.lua`)
- **Location:** `ServerScriptService.PersistenceService`
- **Scope:** Industrial-grade DataStore engine preventing profile corruption and race conditions.
- **Key Responsibilities:**
  - Manages player data under DataStore key `ASTROVERSE_PlayerProgress_DEV_v1`.
  - **Session Lease Locking:** Implements a 300-second session lock (`SESSION_LOCK_TIMEOUT = 300`) with a 90-second heartbeat renewal (`LEASE_REFRESH_INTERVAL = 90`) to prevent multi-server profile overwrites.
  - **Autosave Pipeline:** Performs periodic profile backups every 180 seconds (`AUTOSAVE_INTERVAL = 180`) with bounded random jitter (`0–10s`) to avoid DataStore throttling spikes.
  - **Studio Fallback:** In local Studio sessions where cloud DataStores are inactive, transparently runs against `StudioMockCache` in memory.
  - **Production Circuit Breaker:** Explicitly halts if executed in Production Place ID `111861430801322` without authorized keys.

### 3.4. Lunar Rover Simulation (`RoverService.lua` & `RoverClientController.lua`)
- **Location:** `ServerScriptService.RoverService` & `StarterPlayerScripts.RoverClientController`
- **Scope:** Distributed physics vehicle system optimized for low lunar gravity (`32.4 studs/s²`).
- **Key Responsibilities:**
  - **Boarding Protocol:** Player prompts `RequestBoard`. Server transfers network ownership of the rover chassis and wheels to the player's client for lag-free local physics calculation.
  - **Suspension Tuning:** 6-wheel independent suspension using `SpringConstraint` instances (`Stiffness: 9000`, `Damping: 2200`).
  - **Dynamic Downforce:** Client applies downward `VectorForce` (`500–2400 N`) scaled with speed, preventing the rover from floating off crater rims.
  - **Dual-Stage Gyroscopic Leveling:** Client monitors pitch and roll angles:
    - Roll > 18° or Pitch > 22°: Proportional leveling torque is applied to chassis.
    - Roll/Pitch > 45°: Anti-flip auto-righting activates, gently righting the vehicle.
  - **Safe Raycast Dismount:** On `ExitRover`, server raycasts downwards from an offset (`X = -4.2` studs), places the player on solid terrain, zeroes residual velocity, and restores standard astronaut `JumpPower = 25.0`.

---

## 4. Client UI Architecture (`JuniorHUD`)

The UI is built using standard Roblox `ScreenGui` components organized into independent procedural cards:

```
ScreenGui (JuniorHUD)
├── WelcomeScreen
│   └── Card (MainCream #F8F4E8, Kalam Title, PatrickHand Subtitle, Golden StartButton)
├── InGameHUD
│   ├── TopLeftBanner (Mission Badge Icon, Mission Name, Progress Text)
│   ├── TopRightBar (Help Button, Logbook Button, Aerial View Button)
│   └── CompassBar (3D World Navigational Beacon pointing to active objective)
├── MissionBriefing (Modal card introducing mission context, science source, and instructions)
├── MissionSuccess (Celebration modal with rotating badge, sparkles, and graduation button)
├── ScienceFactCard (Educational modal detailing authentic NASA open data references)
├── JuniorLogbook (Certificate of Lunar Exploration, Official Seal, and 5-Badge Grid)
└── AerialOverviewUI (Top-down viewport overlay with Return to Astronaut button)
```

---

## 5. Architectural Diagram Links

For visual representations of this architecture, see:
- [Full System Architecture Diagram](diagrams/system-architecture.md)
- [Player Gameplay Flowchart](diagrams/gameplay-flow.md)
- [Mission Progression Diagram](diagrams/mission-progression.md)
- [NASA Data Pipeline](diagrams/nasa-data-pipeline.md)
- [Persistence & Session Locking Flow](diagrams/persistence-flow.md)
