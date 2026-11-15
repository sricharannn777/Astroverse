# ASTROVERSE Architecture Diagram

This document contains the complete system architecture diagram for ASTROVERSE, illustrating the communication flow between the Roblox client, shared replication modules, server-authoritative services, and external open science APIs.

```mermaid
flowchart TD
    subgraph ClientLayer ["Client Layer (StarterPlayerScripts & StarterGui)"]
        JCC["JuniorClientController<br/>(State Machine & Visuals)"]
        RCC["RoverClientController<br/>(Inputs, Downforce & Anti-Flip)"]
        ACC["AerialCameraController<br/>(Bird's-Eye View Interpolation)"]
        HUD["JuniorHUD<br/>(Sketchbook Journal & HUD Cards)"]
        RoverUI["RoverDrivingUI<br/>(Touch Pedals & Telemetry)"]
    end

    subgraph SharedLayer ["Shared Replication Layer (ReplicatedStorage)"]
        Config["JuniorMissionConfig<br/>(Objectives, Coordinates & Facts)"]
        Theme["JuniorSketchbookTheme<br/>(Paper Styles, Colors & Fonts)"]
        CamConfig["JuniorAerialCameraConfig<br/>(Coordinates, FOV & Tweens)"]
        GeolReg["GeologicalTargetRegistry<br/>(41 Exploration Coordinates)"]
        
        subgraph Remotes ["Remote Function & Event Busses"]
            R_Mission["JuniorMissionRemotes<br/>(Start, Action, State, Celebrate)"]
            R_Rover["RoverRemotes<br/>(Board, Exit, Headlights, Boost)"]
            R_Persist["PersistenceRemotes<br/>(GetStatus, Preferences)"]
        end
    end

    subgraph ServerLayer ["Server-Authoritative Layer (ServerScriptService)"]
        JMS["JuniorMissionService<br/>(Validation & Badge Management)"]
        RS["RoverService<br/>(Physics Ownership & Safe Dismount)"]
        NASA["NASADataService<br/>(DONKI Client & 30m Cache)"]
        PS["PersistenceService<br/>(Profile Lifecycle & Session Lock)"]
        Outpost["Outpost Subsystems<br/>(Airlock, Oxygen, Power Grid)"]
    end

    subgraph ExternalLayer ["External Data & Engine Services"]
        DONKI["NASA CCMC DONKI REST API<br/>(Open Solar Flare Feed)"]
        Fallback["Verified Historical Event<br/>(GOES-16 SUVI AR3664)"]
        DSS["Roblox DataStoreService<br/>(Cloud Storage)"]
        MockCache["StudioMockCache<br/>(In-Memory Safe Fallback)"]
    end

    %% Client Interactions
    JCC --> HUD
    RCC --> RoverUI
    ACC -.-> JCC
    JCC --> R_Mission
    RCC --> R_Rover
    JCC --> R_Persist

    %% Remotes to Server
    R_Mission --> JMS
    R_Rover --> RS
    R_Persist --> PS

    %% Server Logic
    JMS --> Config
    JMS --> NASA
    RS --> Outpost
    PS --> DSS
    PS -.->|Offline Mode| MockCache
    NASA -->|HTTPS GET| DONKI
    NASA -.->|Fallback on Failure| Fallback

    %% Replication
    JMS -.->|State Broadcast| R_Mission
    RS -.->|Hover Broadcast| R_Rover
    R_Mission -.->|Sync| JCC
    R_Rover -.->|Sync| RCC
```

---

[Back to System Architecture Documentation](../SYSTEM_ARCHITECTURE.md)
