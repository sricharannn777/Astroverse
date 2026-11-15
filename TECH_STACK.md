# ASTROVERSE — Technology Stack & Dependencies

## 1. Core Engine & Platform

- **Game Engine:** [Roblox Engine](https://create.roblox.com/)
  - **Studio Client Version:** `0.747.0.7471481` (Build `version-63eb6055af4e4688`)
  - **Architecture:** Client-Server Distributed Simulation Engine
  - **Target Deployment Platform:** PC (Windows / macOS), Mobile (iOS / Android), Tablet (iPadOS / Android Tablet), Console (Xbox One / Series X / PlayStation 4 / 5)

---

## 2. Programming Language & Runtime

- **Language:** **Luau** (Open-source, sandboxed derivative of Lua 5.1 developed by Roblox)
- **Runtime Features Utilized:**
  - **Task Library (`task.spawn`, `task.wait`, `task.delay`, `task.cancel`):** High-precision asynchronous coroutine scheduling integrated with the engine's Task Scheduler.
  - **Strict Type Annotations:** Function signatures, configuration records, and remote event payload schemas are strongly typed where applicable.
  - **String Interpolation & Vector Math:** Fast built-in Vector3 arithmetic and string formatting.
  - **Zero External DLL Dependencies:** 100% native Luau code running securely within the Roblox sandboxed virtual machine.

---

## 3. Roblox Engine Services Utilized

| Service | Primary Role in ASTROVERSE | Implementation Reference |
| :--- | :--- | :--- |
| **`HttpService`** | Fetches live solar flare data from NASA CCMC DONKI API via HTTPS GET; JSON encoding/decoding. | `ServerScriptService.NASADataService` |
| **`DataStoreService`** | Server-authoritative persistent profile storage with 300s session leases and autosaves. | `ServerScriptService.PersistenceService` |
| **`TweenService`** | Smooth camera interpolation (Aerial Overview) and sketchbook card slide-in/fade-out animations. | `StarterPlayerScripts.JuniorClientController` |
| **`UserInputService`** | Cross-platform keyboard, mouse, touch, and gamepad controller event handling. | `StarterPlayerScripts.RoverClientController` |
| **`RunService`** | High-frequency frame rendering loops (`RenderStepped`) and server physics validation (`Heartbeat`). | `RoverClientController`, `PersistenceService` |
| **`MarketplaceService`** | In-engine Place ID and Universe ID verification for production isolation safety. | `NASADataService`, `PersistenceService` |
| **`Workspace`** | Raycasting for safe rover dismount; global low-gravity simulation (`Gravity = 32.4 studs/s²`). | `RoverService`, `RoverClientController` |
| **`SoundService`** | Dynamic audio mixing, ambient lunar wind loops, tactile UI clicks, and mission fanfare. | `StarterPlayerScripts.AudioClientController` |
| **`ReplicatedStorage`** | Shared configurations (`JuniorMissionConfig`), remotes folders, and design tokens. | `ReplicatedStorage` root |
| **`ServerScriptService`** | Authoritative mission logic, vehicle physics ownership delegation, and persistence. | `ServerScriptService` root |

---

## 4. External Data APIs

- **NASA CCMC DONKI REST API:**
  - **Base URL:** `https://ccmc.gsfc.nasa.gov/DONKI-API/get/`
  - **Endpoint:** `FLR` (Solar Flares)
  - **Format:** RESTful JSON
  - **Authentication:** Public open access (No credentials required)
  - **Caching:** 30-minute in-memory cache managed by `NASADataService`

---

## 5. Physics & Simulation Architecture

- **Gravity Constant:** `32.4 studs/s²` (Configured in `Workspace.Gravity`, exactly 16.5% of standard Earth gravity `196.2 studs/s²`).
- **Vehicle Constraints:**
  - `CylindricalConstraint` for wheel axle rotation.
  - `SpringConstraint` for independent 6-wheel terrain compliance (`Stiffness: 9000`, `Damping: 2200`).
  - `VectorForce` for dynamic lunar regolith downforce (`500–2400 N`).
- **Collision Grouping:** `DriverCockpit.DriverSeat.CanCollide = false` to eliminate physics clipping between humanoid avatars and vehicle meshes.

---

## 6. User Interface Architecture

- **UI System:** Roblox `ScreenGui` using CanvasGroup, Frame, TextLabel, and ImageButton instances.
- **Responsive Layout:** Dynamic scale vectors (`UDim2.new(scaleX, offsetX, scaleY, offsetY)`) ensuring consistent layout from 720p mobile screens to 4K desktop displays.
- **Typography:**
  - `Enum.Font.Kalam`: Primary titles, mission stamps, and start buttons.
  - `Enum.Font.PatrickHand`: Subtitles, handwritten notes, and descriptions.
  - `Enum.Font.FredokaOne`: Tactile action buttons.
  - `Enum.Font.BuilderSansMedium`: Body science text and technical data values.

---

## 7. Development & QA Tooling

- **IDE:** Roblox Studio (Official 64-bit client)
- **MCP Integration:** Roblox Studio Model Context Protocol (MCP) server for programmatic inspection and telemetry logging.
- **Agent Orchestration:** Google Antigravity Agent for automated testing, static code analysis, and documentation generation.
- **Scripting Automation:** Windows PowerShell 5.1 / 7 for binary checksum validation and artifact staging.
