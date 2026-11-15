# ASTROVERSE Player Gameplay Flowchart

This diagram visualizes the end-to-end player gameplay loop, from avatar spawn and sketchbook journal introduction, through the five training missions, to rover driving, aerial observation, and graduation.

```mermaid
flowchart TD
    Start(["Player Spawns in Moon Base Alpha Airlock"]) --> WelcomeCard["Display WelcomeScreen Journal Card<br/>('ASTROVERSE 🚀')"]
    
    WelcomeCard --> ClickStart["Player Clicks 'LET'S EXPLORE THE MOON 🌟'"]
    ClickStart --> InitServer["Server Initializes Profile<br/>(JuniorTrainingMode = true, Mission = 1)"]
    
    InitServer --> DisplayHUD["HUD Displays Mission 1 Tracker & 3D Nav Beacon"]
    
    subgraph MissionLoop ["Core Mission Loop (Missions 1 to 5)"]
        DisplayHUD --> FollowBeacon["Follow 3D Holographic Nav Beacon to Target"]
        FollowBeacon --> ReachTarget["Step within 12 studs of Interactive Console"]
        ReachTarget --> PromptCard["Display Action Prompt (e.g., 'Hold [E] to Align Panels')"]
        
        PromptCard --> HoldAction{"Player Holds [E] or Taps Screen"}
        HoldAction -- "Released Early" --> CancelAction["Reset Progress Ring Safely"]
        CancelAction --> PromptCard
        
        HoldAction -- "Duration Met (0.5s - 1.8s)" --> ServerValidate["Server Validates Distance & Duration"]
        ServerValidate --> GrantBadge["Server Grants Mission Badge & Updates Profile"]
        GrantBadge --> Celebration["Show Celebration Fanfare & Stamped Badge"]
        Celebration --> FactCard["Display NASA Science Fact Card"]
        FactCard --> ClickNext["Player Clicks 'NEXT MISSION'"]
        ClickNext --> AdvanceCheck{"All 5 Missions Complete?"}
        AdvanceCheck -- "No (Missions 1-4)" --> NextMission["Load Next Mission Data"]
        NextMission --> DisplayHUD
    end

    AdvanceCheck -- "Yes (Mission 5 Finished)" --> Graduation["Grand Celebration: Certificate of Lunar Exploration"]
    Graduation --> OpenLogbook["Open JuniorLogbook with Official Gold Explorer Seal"]
    
    subgraph FreeRoam ["Free-Roam Lunar Sandbox"]
        OpenLogbook --> FreeExplore["Free-Roam Exploration Unlocked"]
        FreeExplore --> RoverDrive["Drive 6-Wheeled Lunar Rover across Crater Basin"]
        FreeExplore --> AerialView["Activate Aerial Overview Camera (Bird's-Eye View)"]
        FreeExplore --> Memorials["Visit Apollo 11 & Chandrayaan-3 Tiranga Memorials"]
        FreeExplore --> ScienceScan["Inspect 41 Geological Rock Targets"]
    end
```

---

[Back to Gameplay Flow Documentation](../GAMEPLAY_FLOW.md)
