# ASTROVERSE — Installation & Local Setup Guide

## 1. System Requirements & Prerequisites

To open, run, or develop ASTROVERSE locally, you will need:

- **Roblox Studio:** Latest 64-bit client (Windows 10/11 or macOS 11+). [Download Roblox Studio](https://create.roblox.com/).
- **Roblox Account:** Standard free Roblox developer account.
- **Hardware:**
  - Dedicated or modern integrated GPU supporting DirectX 11 / Metal.
  - 8 GB RAM minimum (16 GB recommended).
  - Internet connection for initial package streaming and live NASA CCMC queries.

---

## 2. Project File Structure

The core game is distributed as a single, self-contained binary place file:
- **Primary File:** `ASTROVERSE_Final_Rover_StoneFix.rbxl`
- **File Size:** `1,731,826 bytes`
- **SHA-256 Checksum:** `A53CBE6EA1F8730048AF9FD1D75F36DEC18009A2A6937919636EA16F2D80E45A`

---

## 3. Opening the Project

1. Launch **Roblox Studio**.
2. Click **File → Open from Disk...** (or press `Ctrl+O`).
3. Select `ASTROVERSE_Final_Rover_StoneFix.rbxl`.
4. Allow Roblox Studio to load the terrain, models, and scripts into memory.

---

## 4. Studio Configuration & Permissions

### 4.1. Enabling Live NASA Open Science Data
By default, Roblox Studio restricts outbound web requests. To allow `NASADataService` to query the live NASA CCMC DONKI API:
1. In the top ribbon, click **Home → Game Settings**.  
   *(If prompted to save the place to the cloud first, do so to your designated development place).*
2. Navigate to the **Security** tab on the left.
3. Toggle **Allow HTTP Requests** to **ON**.
4. Click **Save**.

> [!NOTE]
> If HTTP requests remain disabled, ASTROVERSE will automatically and gracefully fall back to its verified historical observation from Solar Cycle 25 (GOES-16 SUVI data from Sunspot AR3664). The game remains 100% playable even without internet access!

### 4.2. Enabling Persistent DataStores in Studio (Optional)
By default, ASTROVERSE runs against an in-memory `StudioMockCache` during Studio playtests so local tests never overwrite real cloud player data. If you want to test against live Roblox Cloud DataStores:
1. In **Game Settings → Security**, toggle **Enable Studio Access to API Services** to **ON**.
2. Click **Save**.

---

## 5. Running & Testing the Game

### 5.1. Quick Single-Player Playtest
- Press **`F5`** (or click the **Play** button in the Home ribbon).
- Your character will spawn in the airlock corridor.
- The handwritten `WelcomeScreen` card will appear.
- Click **`LET'S EXPLORE THE MOON 🌟`** to begin Mission 1.

### 5.2. Mobile & Tablet Responsive Testing
ASTROVERSE includes dynamic UI scaling that adapts to any screen resolution:
1. In the top ribbon, switch to the **Test** tab.
2. Click **Device** (or press `Ctrl+Shift+I` to open the Device Emulator).
3. Select various test devices:
   - `iPhone 14 Pro` (Mobile aspect ratio, verifies touch pedals)
   - `iPad Pro` (Tablet aspect ratio, verifies journal layout)
   - `1080p Desktop` (Standard widescreen PC)
4. Verify that buttons, text cards, and pedals scale proportionally without clipping.

### 5.3. Multi-Player Testing
To test multi-astronaut cooperative play or teacher-student sessions:
1. Open the **Test** tab.
2. In the **Simulation** section, set **Clients** to `2 Players` and click **Start**.
3. Two independent game client windows and a dedicated local server window will launch.
4. Verify that player missions progress independently and that rover network ownership transfers cleanly to the first seated player.

---

## 6. Publishing Guidelines & Safety Precautions

When saving or publishing changes to Roblox Cloud:

1. Click **File → Publish to Roblox As...**.
2. When the experience selection dialog appears, select your **DEVELOPMENT** experience:
   - **Target Experience Name:** `ASTROVERSE_FlatMoon_WorkingCopy.rbxl` (or `ASTROVERSE_Flat...`)
   - **Target Place ID:** `120587418966837`
   - **Target Universe ID:** `10775519566`
3. Click the root place to confirm.

> [!CAUTION]
> **DO NOT PUBLISH TO PRODUCTION WITHOUT AUTHORIZATION:**  
> The production experience (`AstraVerse` / Place ID `111861430801322` / Universe ID `10775323670`) is the live public release. Never publish experimental development builds to production.
