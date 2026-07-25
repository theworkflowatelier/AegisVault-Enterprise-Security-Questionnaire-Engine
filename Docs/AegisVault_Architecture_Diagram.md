# 📐 AegisVault — Interactive System Architecture Diagram

```mermaid
graph TB
    %% ZONE 1: POLICY GROUND TRUTH INGESTION
    subgraph Zone1 ["🔵 ZONE 1: POLICY GROUND TRUTH INGESTION"]
        direction TB
        N1(["🕒 Node 1: [TRIGGER]<br>30-Minute Cron Schedule"]) --> N2["📂 Node 2: [GROUND TRUTH]<br>Search Policy Docs Folder"]
        N2 --> N3{"🔀 Node 3: [ROUTER]<br>Evaluate Document Type"}
        
        N3 -- Docs / PDF / TXT --> N4A["📄 Node 4A: [DOWNLOAD]<br>Convert to text/plain"]
        N4A --> N4B["📝 Node 4B: [EXTRACT]<br>Stream Text Contents"]
        
        N3 -- Sheets / Excel / CSV --> N5A["📊 Node 5A: [DOWNLOAD]<br>Fetch Spreadsheet Binary"]
        N5A --> N5B["📝 Node 5B: [EXTRACT]<br>Read CSV Text Stream"]
        N5B --> N5C["⚙️ Node 5C: [FORMAT]<br>Stringify Table Rows"]
        
        N4B --> N7(["🧠 Node 7: [MEMORY]<br>Aggregate Master Ground Truth"])
        N5C --> N7
    end

    %% ZONE 2: MULTI-FORMAT PARSING
    subgraph Zone2 ["🟣 ZONE 2: MULTI-FORMAT QUESTIONNAIRE PARSING"]
        direction TB
        N7 --> N8["📂 Node 8: [INGEST]<br>Scan Inbound Questionnaires Folder"]
        N8 --> N9["🔁 Node 9: [LOOP]<br>Iterate Inbound Files"]
        N9 --> N10{"🔀 Node 10: [ROUTER]<br>Evaluate File MIME Type"}
        
        N10 -- Native Google Sheet --> N11A["📈 Node 11A: [READ]<br>Fetch Google Sheet Rows"]
        N10 -- Excel Spreadsheet --> N11B["📥 Node 11B: [DOWNLOAD]<br>Fetch Excel Binary"] --> N11C["📑 Node 11C: [PARSE]<br>Extract Excel Rows"]
        N10 -- CSV Binary --> N11D["📥 Node 11D: [DOWNLOAD]<br>Fetch CSV Binary"] --> N11E["📑 Node 11E: [PARSE]<br>Extract CSV Rows"]
        
        N11A --> N12["🔗 Node 12: [MERGE]<br>Combine All Data Streams"]
        N11C --> N12
        N11E --> N12
        N12 --> N13["🛠️ Node 13: [TRANSFORM]<br>Normalize Headers & Map IDs"]
    end

    %% ZONE 3: AI EVALUATION CORE
    subgraph Zone3 ["🟡 ZONE 3: GEMINI 1.5 PRO / 2.0 FLASH EVALUATION CORE"]
        direction TB
        N13 --> N14["⏳ Node 14: [THROTTLE]<br>Group into 15-Row Batches"]
        N14 --> N15(["🤖 Node 15: [AI ARCHITECT]<br>Evaluate Against Ground Truth"])
        
        SubA[["⚡ Sub-Node 15A:<br>models/gemini-2.0-flash<br>Temp: 0.1"]] -.-> N15
        SubB[["📐 Sub-Node 15B:<br>Structured Output Parser<br>Strict Schema"]] -.-> N15
    end

    %% ZONE 4: GATING & STORAGE
    subgraph Zone4 ["🟢 ZONE 4: CONFIDENCE GATING & DASHBOARD STORAGE"]
        direction TB
        N15 --> N16{"⚖️ Node 16: [GATE]<br>Knowledge Match = Present?"}
        
        N16 -- YES Verified Match --> N17A["✅ Node 17A: [STORAGE]<br>Append to Master Output Tab"]
        N16 -- NO Missing / Unknown --> N17B["⚠️ Node 17B: [STORAGE]<br>Route to Review_Required Tab"]
        
        N17A --> N19["⏱️ Node 19: [THROTTLE]<br>15-Second Quota Cooldown"]
        N17B --> N19
        
        N19 -- Loop Next 15-Row Batch --> N14
        N14 -- Queue Completed --> N20(["📦 Node 20: [ARCHIVE]<br>Move File to Completed Folder"])
    end

    %% ZONE 5: SYSTEM AUDIT LOGGING
    subgraph Zone5 ["🔴 ZONE 5: IMMUTABLE ERROR AUDIT LOGGING"]
        N18["🛡️ Node 18: [STORAGE]<br>Append Ground Truth / AI Errors"]
        N18b["🛡️ Node 18b: [STORAGE]<br>Append Ingestion / Parse Errors"]
    end

    %% Error Wire Routing
    N4A -. HTTP 4XX/5XX Error .-> N18
    N5A -. HTTP 4XX/5XX Error .-> N18
    N15 -. Rate Limit / Schema Error .-> N18
    
    N11A -. API / Sheet Error .-> N18b
    N11B -. Download Error .-> N18b
    N11D -. Download Error .-> N18b

    %% STYLING
    classDef blue fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,color:#0d47a1,rx:8px,ry:8px,font-weight:bold;
    classDef purple fill:#f3e5f5,stroke:#6a1b9a,stroke-width:2px,color:#4a148c,rx:8px,ry:8px,font-weight:bold;
    classDef yellow fill:#fffde7,stroke:#f57f17,stroke-width:2px,color:#e65100,rx:8px,ry:8px,font-weight:bold;
    classDef green fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px,color:#1b5e20,rx:8px,ry:8px,font-weight:bold;
    classDef red fill:#ffebee,stroke:#c62828,stroke-width:2px,color:#b71c1c,rx:8px,ry:8px,font-weight:bold;
    classDef router fill:#fff3e0,stroke:#e65100,stroke-width:2.5px,color:#bf360c,rx:12px,ry:12px,font-weight:bold;
    classDef subnode fill:#f5f5f5,stroke:#616161,stroke-width:1.5px,stroke-dasharray: 5 5,color:#212121,rx:4px,ry:4px;

    class N1,N2,N4A,N4B,N5A,N5B,N5C,N7 blue;
    class N8,N9,N11A,N11B,N11C,N11D,N11E,N12,N13 purple;
    class N14,N15 yellow;
    class N17A,N17B,N19,N20 green;
    class N18,N18b red;
    class N3,N10,N16 router;
    class SubA,SubB subnode;
