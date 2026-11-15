# ASTROVERSE Data Persistence & Session Locking Flowchart

This diagram illustrates the session-hardened player persistence lifecycle implemented in `ServerScriptService.PersistenceService`, showing session lease acquisition, heartbeat renewal, periodic autosaving, and shutdown flushing.

```mermaid
sequenceDiagram
    autonumber
    actor Player as Player joins Server
    participant Server as PersistenceService
    participant Memory as SessionCache
    participant DSS as DataStoreService (Cloud)
    participant Mock as StudioMockCache (Offline)

    %% Session Initialization
    Player->>Server: PlayerAdded Event
    
    alt In-Studio Session without Cloud APIs
        Server->>Mock: Load or Initialize Profile (Dev Schema v1)
        Mock-->>Server: Return Profile Data
        Server->>Memory: Cache Profile (Lease = Local)
    else Live Cloud Server / Studio Cloud Access
        Server->>DSS: UpdateAsync(Key, AcquireLease)
        alt Key Locked by Another Active Session (< 300s old)
            DSS-->>Server: Conflict: Lease Active Elsewhere
            Server->>Server: Retry with Exponential Backoff (max 3 retries)
        else Key Free or Lease Expired (> 300s)
            DSS-->>Server: Grant Lease (SessionID, Expires = Now + 300s)
            Server->>Memory: Store Active Profile & Lease Token
        end
    end
    
    Server->>Player: Set Attributes (JuniorCurrentMission, ProfileLoaded = true)

    %% Periodic Runtime Heartbeat & Autosave
    loop Every 90 Seconds (Heartbeat Lease Renewal)
        Server->>DSS: UpdateAsync: Refresh Lease (+300s from Now)
        DSS-->>Server: Lease Confirmed
    end

    loop Every 180 Seconds + Jitter (0-10s)
        Server->>DSS: UpdateAsync: Full Profile State Flush
        DSS-->>Server: Save Acknowledged
    end

    %% Graceful Teardown
    critical Player Disconnects or Server Shuts Down
        Player->>Server: PlayerRemoving / game:BindToClose()
        Server->>Memory: Extract Final Attributes & Badges
        alt Live Cloud Server
            Server->>DSS: UpdateAsync: Save Final Data & Release Lease (SessionID = nil)
            DSS-->>Server: Profile Finalized
        else Offline Studio
            Server->>Mock: Commit Final State to Memory
        end
    end
```

---

[Back to System Architecture Documentation](../SYSTEM_ARCHITECTURE.md)
