# Mermaid Templates for architecture-map

## Mindmap (project overview)
```mermaid
mindmap
  root((Project Name))
    Core
      config.py
      main.py
    Agents
      watcher
      analyst
      debater
      judge
    Market
      client
      indicators
    Trading
      risk_manager
      strategies
      executor
    Knowledge
      graph
      memory
    Infrastructure
      telemetry
      news
```

## Architecture graph
```mermaid
graph TD
    Main[main.py] --> Graph[LangGraph]
    Graph --> Agents[Agents Layer]
    Graph --> Trading[Trading Layer]
    Agents --> LLM[LLM Client]
    Agents --> KG[Knowledge Graph]
    Trading --> Risk[Risk Manager]
    Trading --> Market[Market Data]
    Market --> MOEX[MOEX ISS API]
    LLM --> OpenRouter[OpenRouter API]
    Risk -.->|DETERMINISTIC| Trading
    style Risk fill:#ff6b6b,color:#fff
    style LLM fill:#4ecdc4,color:#fff
```

## Sequence diagram (data flow)
```mermaid
sequenceDiagram
    participant S as Scheduler
    participant W as Watcher
    participant A as Analyst
    participant D as Debater
    participant R as Risk Manager
    participant M as MOEX API

    S->>W: trigger check
    W->>M: fetch market data
    M-->>W: candles + orderbook
    W->>A: significant event detected
    A->>D: analyze situation
    D->>D: Bull vs Bear debate
    D->>R: proposed action
    R-->>D: approved | rejected
    Note over R: DETERMINISTIC ONLY
```

## File tree (copy-paste template)
```
project/
├── src/
│   ├── config.py           — Description
│   ├── main.py             — Description
│   ├── module_a/           — Description
│   │   ├── file_1.py       — Description
│   │   └── file_2.py       — Description
│   └── module_b/           — Description
├── tests/                  — Description
├── .ai/                    — AI context (DenseCode)
└── .foryou/                — Human docs (Russian)
```

## State diagram (for LangGraph)
```mermaid
stateDiagram-v2
    [*] --> Watching
    Watching --> Analyzing: event_detected
    Analyzing --> Debating: analysis_complete
    Debating --> RiskCheck: decision_made
    RiskCheck --> Executing: approved
    RiskCheck --> Watching: rejected
    Executing --> Watching: order_placed
    Executing --> ErrorState: execution_failed
    ErrorState --> Watching: recovered
```
