# Contributing to ASTROVERSE

First off, thank you for considering contributing to **ASTROVERSE**! As an open-source educational project developed for the **NASA Space Apps Challenge 2026**, our goal is to inspire the next generation of space explorers, scientists, and engineers.

We welcome contributions from game developers, 3D artists, STEM educators, translators, and space enthusiasts.

---

## 1. Code of Conduct

ASTROVERSE is an educational platform designed for children aged **7 to 12**, their teachers, and families. All contributors are expected to uphold a safe, respectful, inclusive, and encouraging environment.
- Use welcoming and inclusive language.
- Be respectful of differing viewpoints and experiences.
- Keep all educational content age-appropriate, scientifically accurate, and constructive.

---

## 2. Ways You Can Contribute

### 2.1. Proposing New Educational Missions
We are always eager to expand our curriculum with new space science topics (e.g., lunar geology, spacesuit cooling systems, 3D printing with regolith). To propose a mission:
1. Identify a real-world aerospace challenge faced by NASA, ESA, ISRO, or other space agencies.
2. Outline a concrete, physical in-game interaction (e.g., deploying a sensor, adjusting a valve).
3. Provide citations to genuine open-science research (NASA mission reports, peer-reviewed studies).
4. Open an issue on GitHub using the **Mission Proposal** template.

### 2.2. Luau Code Optimization & Bug Fixes
- Enhancing mobile rendering performance.
- Refining vehicle suspension physics and constraint tuning.
- Expanding offline fallback datasets for space weather events.

### 2.3. International Localization (Translating ASTROVERSE)
Astronauts work in international teams, and we want ASTROVERSE accessible worldwide! We welcome translations of our mission cards, sketchbook journals, and science facts into:
- Spanish, French, German, Hindi, Japanese, Portuguese, Arabic, and other languages.

### 2.4. Educational Curriculum & Teacher Guides
- Creating printable classroom worksheets, discussion guides, and post-game STEM quizzes.

---

## 3. Development Workflow & Contribution Rules

### 3.1. Prerequisites
- Roblox Studio (Latest 64-bit client).
- Basic understanding of Client-Server Luau architecture.

### 3.2. Strict Development Rules
To maintain the safety and integrity of the project:
1. **Never Commit API Keys or Credentials:** ASTROVERSE relies strictly on open, unauthenticated public endpoints (like CCMC DONKI). Never commit tokens, secrets, or cookies.
2. **Server Authority is Mandatory:** All mission state transitions, rewards, and persistence updates must be validated on the server. Never trust client requests without spatial and logical verification.
3. **No External DLLs or Exploits:** Use only standard, sandboxed Roblox Engine services and Luau libraries.
4. **Preserve Sketchbook UI/UX Consistency:** All new user interfaces must adhere to the design tokens in `ReplicatedStorage.JuniorSketchbookTheme` (cream paper `#F8F4E8`, `Kalam` and `PatrickHand` typography, tape accents).

### 3.3. Submitting a Pull Request
1. Fork the repository on GitHub.
2. Create a topic branch (`git checkout -b feature/lunar-core-drill`).
3. Make your modifications in a local copy of the RBXL place file.
4. Export updated scripts and document your changes clearly.
5. Verify your changes in Roblox Studio using both PC and Device Emulator (Mobile) viewports.
6. Open a Pull Request with a clear description of:
   - What problem your change solves.
   - How you tested it.
   - Any scientific citations or references.

---

## 4. Questions & Support

If you have questions about the codebase or want to coordinate on a larger educational feature, please open a GitHub Discussion or reach out through the project repository.
