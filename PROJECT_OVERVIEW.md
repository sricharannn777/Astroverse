# ASTROVERSE — Project Overview

## 1. Executive Summary

**ASTROVERSE** is an open-source, interactive lunar exploration and astronaut training experience developed for the **NASA Space Apps Challenge 2026**. Built on the Roblox platform, it provides children aged **7 to 12** with an intuitive, hands-on introduction to space science, planetary engineering, and lunar habitat survival.

Players step into the boots of a Junior Astronaut arriving at **Moon Base Alpha**, located in the rugged highlands of the Moon's South Pole. Guided by a tactile astronaut mission journal, players master five fundamental disciplines required for sustainable human presence on the Moon:
1. Harvesting solar energy on an airless celestial body.
2. Maintaining life-support oxygen systems (ECLSS).
3. Utilizing lunar regolith as radiation shielding against cosmic rays.
4. Cultivating space crops in a sealed hydroponic greenhouse.
5. Monitoring real-time and historical space weather alerts via the NASA CCMC DONKI API.

Upon completing all five missions, players earn official mission badges, unlock an astronaut graduation certificate with an official seal, and earn free-roam driving privileges in a 6-wheeled scientific lunar rover.

---

## 2. Educational Philosophy & Pedagogical Framework

Children often experience space science through passive media—textbooks, videos, and multiple-choice quizzes. ASTROVERSE is built upon a **"Learn by Doing" (Constructivist)** pedagogical model:

- **Tangible Cause-and-Effect:** Instead of reading about solar power, the player follows glowing navigational beacons to the solar array, physically activates the tracking switches, and witnesses the outpost power grid illuminate.
- **Cognitive Scaffolding:** Missions are sequenced logically. Power must be established before life support can operate; life support must be stable before radiation shields and greenhouses can function; finally, players learn to monitor solar flares that threaten the entire base.
- **Authentic Aerospace Data:** Science cards do not rely on generic sci-fi tropes. All terminology, concepts, and mission facts cite genuine NASA research programs, including the Artemis Program, Lunar Reconnaissance Orbiter (LRO), and the Advanced Plant Habitat (APH).
- **Handwritten Journal Aesthetic:** The user interface simulates a child's astronaut notebook—with cream-toned paper (`#F8F4E8`), handwritten pencil fonts (`Kalam` and `PatrickHand`), colorful sticky tape accents, and collectible badge stamps. This design removes intimidating technical UI barriers and creates an inviting, storybook atmosphere.

---

## 3. Target Audience & Accessibility

### Primary Audience: Junior Astronauts (Ages 7–12)
- **Reading Level:** Formatted with bite-sized, active sentences, clear bullet points, and highlighted keywords.
- **Controls:** Streamlined interaction model. Most actions require either a single button tap or an intuitive "Hold to Action" duration (`0.5s` to `1.8s`) with on-screen circular progress feedback.
- **Multi-Device Support:** Operates seamlessly across desktop PCs, laptops, iPads, tablets, and smartphones.

### Secondary Audience: STEM Educators & Parents
- **Classroom Applicability:** A full playthrough of all five missions takes approximately **15 to 25 minutes**, making it an ideal digital lab activity for a single class period.
- **Teacher Observation Mode:** The **Aerial Overview Camera** allows an educator or student to zoom out to a bird's-eye perspective of Moon Base Alpha (`LookAt: (4, 15, 220)`), visualizing the spatial relationship between power generation, habitat modules, and greenhouses.

---

## 4. Narrative & World Setting

```
                   [ Earthrise in the Lunar Sky ]
                                 🌍
                                 
                       ★ Moon Base Alpha ★
                     (Lunar South Pole Region)
                                 
   +---------------------------------------------------------------+
   | [Solar Arrays] <---> [Airlock] <---> [Central Habitat Hub]    |
   |                             |               |                 |
   |              [Hydroponic Bay]               [Command & Comms] |
   +---------------------------------------------------------------+
                                 |
           [Tiranga Memorial] <--+--> [Apollo 11 Memorial]
                                 |
                   [Crater Rim & Obsidian Ridge]
```

- **Moon Base Alpha:** A semi-permanent surface habitat built inside a shadowed crater basin near the lunar south pole. The location provides access to eternal sunlight peaks for solar power and permanently shadowed regions (PSRs) harboring water ice.
- **International Lunar Cooperation:** ASTROVERSE celebrates international exploration by featuring both the historic **Apollo 11 Memorial** (celebrating the first human lunar landing in 1969) and the **Tiranga Lunar Memorial** (honoring the Indian Space Research Organisation's Chandrayaan-3 successful landing in 2023).
- **Physical Environment:** Accurate lunar lighting with harsh, non-diffuse shadows, a high-contrast starry sky, visible Earthrise, and reduced lunar gravity (`32.4 studs/s²`, exactly 1/6th of Roblox standard gravity).

---

## 5. Project Roadmap & Documentation Structure

To explore the detailed design, engineering, and data systems of ASTROVERSE, please refer to the following companion documents:

- **System Architecture:** [SYSTEM_ARCHITECTURE.md](SYSTEM_ARCHITECTURE.md)
- **Player Gameplay Flow:** [GAMEPLAY_FLOW.md](GAMEPLAY_FLOW.md)
- **Mission Design & Science Facts:** [MISSION_DESIGN.md](MISSION_DESIGN.md)
- **NASA DONKI Integration:** [NASA_DATA_INTEGRATION.md](NASA_DATA_INTEGRATION.md)
- **Game Systems & Rover Physics:** [GAME_SYSTEMS.md](GAME_SYSTEMS.md)
- **Control Bindings & Accessibility:** [CONTROLS.md](CONTROLS.md)
- **Technology Stack:** [TECH_STACK.md](TECH_STACK.md)
- **Testing & QA Verification:** [TESTING_AND_QA.md](TESTING_AND_QA.md)
- **Installation & Local Studio Setup:** [INSTALLATION_AND_SETUP.md](INSTALLATION_AND_SETUP.md)
- **Assets, Fonts & Sound Credits:** [ASSETS_AND_CREDITS.md](ASSETS_AND_CREDITS.md)
- **Future Roadmap:** [FUTURE_ROADMAP.md](FUTURE_ROADMAP.md)
- **Contributing Guidelines:** [CONTRIBUTING.md](CONTRIBUTING.md)

---

## 6. NASA Independence Notice

> [!NOTE]
> ASTROVERSE is an independent project created for the **NASA Space Apps Challenge 2026**. While it consumes open APIs and educational facts produced by NASA, it is not an official NASA product, nor does it imply endorsement by NASA, the Goddard Space Flight Center, or any government agency.
