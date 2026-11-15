# ASTROVERSE NASA Data Pipeline Diagram

This diagram maps the complete data lifecycle of the NASA CCMC DONKI integration within ASTROVERSE, illustrating request routing, in-memory caching, offline Studio failover, and client presentation.

```mermaid
flowchart TD
    subgraph TriggerStage ["1. Player Interaction Trigger"]
        P1(["Player reaches Mission Control Desk"]) --> Req["Client fires RequestMissionAction(5)"]
        Req --> Gateway["JuniorMissionService invokes NASADataService.GetSpaceWeather()"]
    end

    subgraph CacheEvaluation ["2. Cache Evaluation & Policy"]
        Gateway --> CheckCache{"Is cached data valid?<br/>(Age < 1800s / 30m)"}
        CheckCache -- "YES" --> ServeCache["Return In-Memory Cached Report"]
        CheckCache -- "NO" --> CheckNet{"Is HttpEnabled == true?<br/>(Published Cloud Server)"}
    end

    subgraph NetworkFetch ["3. External NASA CCMC Request"]
        CheckNet -- "YES" --> BuildURL["Construct CCMC DONKI URL<br/>/DONKI-API/get/FLR?startDate=...&endDate=..."]
        BuildURL --> HttpGet["Execute HttpService:RequestAsync(GET)"]
        
        HttpGet --> CheckHTTP{"HTTP Status 200 &<br/>Valid JSON returned?"}
        
        CheckHTTP -- "YES (Live Flares Found)" --> ParseJSON["Parse Array & Extract Latest Flare<br/>(FlareClass, PeakTime, Instrument, Region)"]
        ParseJSON --> CacheLive["Store in Memory (TTL: 1800s)<br/>DataStatus = 'LIVE NASA DONKI OBSERVATION'"]
        
        CheckHTTP -- "NO (Error, Timeout or Empty)" --> TriggerFallback["Activate Fallback Protocol"]
        CheckNet -- "NO (Offline Studio Mode)" --> TriggerFallback
    end

    subgraph FallbackProtocol ["4. Resilient Historical Fallback"]
        TriggerFallback --> LoadSUVI["Load Verified Solar Cycle 25 Event<br/>- Satellite: GOES-16 SUVI<br/>- Active Region: Sunspot AR3664<br/>- Classification: Class M2.5 Flare<br/>- Peak: 2024-05-10 12:45 UTC"]
        LoadSUVI --> SetHistoricalStatus["DataStatus = 'VERIFIED NASA HISTORICAL DATA'<br/>Source = 'NASA DONKI ARCHIVE'"]
    end

    subgraph ClientDelivery ["5. Client Presentation & Education"]
        CacheLive --> ReturnPayload["Return Normalized SpaceWeatherReport Table"]
        SetHistoricalStatus --> ReturnPayload
        ServeCache --> ReturnPayload
        
        ReturnPayload --> ClientSync["Synchronize to JuniorClientController via RemoteFunction"]
        ClientSync --> RenderUI["Render Mission 5 Science Fact Card & Journal Entry"]
        RenderUI --> DrillTrigger["Trigger Outpost Solar Storm Safety Drill"]
    end
```

---

[Back to NASA Data Integration Documentation](../NASA_DATA_INTEGRATION.md)
