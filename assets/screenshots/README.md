# ASTROVERSE Screenshots Guide

This directory holds visual demonstration screenshots for the ASTROVERSE repository and documentation.

---

## 📷 Recommended Screenshot Roster

When preparing the GitHub repository for public presentation, capture or copy the following recommended high-resolution (1080p or 1440p) gameplay screenshots into this folder:

| Filename | In-Game View & Scene Description | Primary Usage in Documentation |
| :--- | :--- | :--- |
| **`screenshot_cleaned_lunar_environment.jpg`** | Wide scenic view of Moon Base Alpha, habitat dome, solar arrays, and unobstructed crater terrain under Earthrise. | `README.md` Hero Image |
| **`screenshot_original_welcome_screen_restored.jpg`** | The restored handwritten cream-paper WelcomeScreen card featuring the `ASTROVERSE 🚀` title, Kalam font, and golden `LET'S EXPLORE THE MOON 🌟` button. | `GAMEPLAY_FLOW.md` / UI Showcase |
| **`screenshot_rover_driving_unobstructed.jpg`** | Third-person view of the 6-wheeled Scientific Lunar Rover driving smoothly across the regolith, showing the `RoverDrivingUI` on-screen touch pedals. | `GAME_SYSTEMS.md` / Rover Section |
| **`screenshot_aerial_overview.jpg`** | High-altitude bird's-eye camera view (`Alt: 215 studs`) looking directly down at Moon Base Alpha with the `AerialOverviewUI` active. | `GAME_SYSTEMS.md` / Camera Section |
| **`screenshot_mission_control.jpg`** | Junior astronaut standing at the Central Mission Control Desk reviewing the NASA CCMC DONKI space weather alert. | `NASA_DATA_INTEGRATION.md` |
| **`screenshot_briefing_transition.jpg`** | Tactile Mission Briefing card displaying educational objectives, hold prompts, and NASA scientific sources. | `MISSION_DESIGN.md` |
| **`screenshot_apollo11_flag_memorial.jpg`** | Commemorative Apollo 11 landing site featuring the descent stage replica, historic plaque, and American flag. | `GAME_SYSTEMS.md` / Memorials |
| **`screenshot_indian_flag_memorial.jpg`** | Commemorative Chandrayaan-3 landing site featuring the lunar lander model, educational marker, and Indian Tricolor flag (Tiranga). | `GAME_SYSTEMS.md` / Memorials |
| **`screenshot_active_mission.jpg`** | In-game HUD showing the objective tracker, progress ring fill, and glowing 3D navigational world beacon. | `GAMEPLAY_FLOW.md` |
| **`screenshot_solarsystem_lunar_surface.jpg`** | High-contrast lunar horizon showing the stark vacuum shadow lines and glowing Earth in the starry black sky. | `PROJECT_OVERVIEW.md` |

---

## 🛠️ How to Capture Fresh Screenshots in Roblox Studio

1. Open `ASTROVERSE_Final_Rover_StoneFix.rbxl` in **Roblox Studio**.
2. Press **`F5`** to start a playtest session.
3. Position your character or camera to frame the scene cleanly.
4. Press **`F12`** in Roblox Studio to capture an uncompressed in-engine screenshot, or use Windows Snipping Tool (`Win + Shift + S`).
5. Save the image into this directory (`assets/screenshots/`) matching the exact filenames listed in the table above.
6. Commit the staged screenshots to git:
   ```bash
   git add assets/screenshots/*.jpg
   git commit -m "docs: add verified gameplay screenshots"
   ```
