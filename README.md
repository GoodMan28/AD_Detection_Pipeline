flowchart TD
    classDef baseline fill:#f9d0c4,stroke:#333,stroke-width:2px;
    classDef context fill:#fcedaa,stroke:#333,stroke-width:2px;
    classDef clinical fill:#cce5ff,stroke:#333,stroke-width:2px;
    classDef noninvasive fill:#d4edda,stroke:#333,stroke-width:2px;
    classDef omni fill:#c3e6cb,stroke:#28a745,stroke-width:3px;

    %% Phase 1
    P1["Phase 1: Baseline CSF Model"]:::baseline --> F1["Features: ABETA, TAU, PTAU<br>(Median Imputation)"]
    F1 --> M1["Algorithm: Random Forest"]
    M1 --> A1(("Accuracy: ~38%"))

    %% Phase 2
    A1 -->|Strategy: Drop NaNs, Add Context| P2["Phase 2: Biological Context"]:::context
    P2 --> F2["Features: Clean CSF, AGE, PTGENDER,<br>TAU/ABETA & PTAU/ABETA Ratios"]
    F2 --> M2["Algorithm: Random Forest"]
    M2 --> A2(("Accuracy: ~54%"))

    %% Phase 3
    A2 -->|Strategy: Anchor with Cognitive Tests| P3["Phase 3: Clinical Grounding"]:::clinical
    P3 --> F3["Features: Phase 2 + MMSE, CDRSB"]
    F3 --> M3["Algorithm: Random Forest"]
    M3 --> A3(("Accuracy: ~86%"))

    %% Phase 4
    A3 -->|Strategy: Drop Invasive CSF, Upgrade Algorithm| P4["Phase 4: High-Res Non-Invasive"]:::noninvasive
    P4 --> F4["Features: APOE4, Demographics, ADAS13,<br>FAQ, RAVLT, MMSE, CDRSB"]
    F4 --> M4["Algorithm: XGBoost"]
    M4 --> A4(("Accuracy: 88.2%"))

    %% Phase 5
    A4 -->|Strategy: Full Battery, Native NaN Handling| P5["Phase 5: Comprehensive Omni-Model"]:::omni
    P5 --> F5["Features: All Neuro & Functional Batteries<br>+ Full Demographics + APOE4"]
    F5 --> M5["Algorithm: XGBoost (No Scaling, NaN Inclusive)"]
    M5 --> A5(("Final Accuracy: 88.87%<br>MCI Recall: 90%"))