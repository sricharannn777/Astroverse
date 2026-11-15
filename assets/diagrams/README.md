# ASTROVERSE Architecture & Technical Diagrams

This directory is reserved for static image renders (SVG / PNG) of ASTROVERSE technical architecture diagrams.

---

## 📊 Live Interactive Mermaid Diagrams

ASTROVERSE utilizes GitHub-native **Mermaid diagrams** embedded directly into Markdown documents. These render dynamically in web browsers and GitHub desktop without requiring static image exports:

1. [Full System Architecture Diagram](../../docs/diagrams/system-architecture.md)
2. [Player Gameplay Flowchart](../../docs/diagrams/gameplay-flow.md)
3. [Five-Mission Progression Diagram](../../docs/diagrams/mission-progression.md)
4. [NASA CCMC DONKI Data Pipeline](../../docs/diagrams/nasa-data-pipeline.md)
5. [Data Persistence & Session Locking Flow](../../docs/diagrams/persistence-flow.md)

---

## 🛠️ Generating Static SVG / PNG Renders (Optional)

If you wish to export offline vector diagrams for whitepapers or slide presentations, you can use the open-source **Mermaid CLI (`@mermaid-js/mermaid-cli`)**:

```bash
# Install Mermaid CLI globally via npm
npm install -g @mermaid-js/mermaid-cli

# Export system architecture diagram to high-resolution PNG
mmdc -i docs/diagrams/system-architecture.md -o assets/diagrams/system-architecture.png -b transparent

# Export NASA data pipeline diagram to scalable SVG
mmdc -i docs/diagrams/nasa-data-pipeline.md -o assets/diagrams/nasa-data-pipeline.svg -b transparent
```
