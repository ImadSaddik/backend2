## 6. Docker Container Communication Architecture with Data Handles

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              Docker Network: app-network                    │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────┐                ┌─────────────────────────┐     │
│  │    fastapi-container    │                │   tool-bridge-container │     │
│  │                         │                │                         │     │
│  │  🐍 Your FastAPI App    │                │  🤖 PydanticAI          │     │
│  │  📊 data_fetcher.py     │                │  🔧 Tool Definitions    │     │
│  │  🗄️  Your Database      │                │  🌐 HTTP Client         │     │
│  │  🔌 Your Endpoints      │                │  📡 MCP Protocol        │     │
│  │                         │                │  💾 JSON Storage:       │     │
│  │  Port: 8000            │◄──────────────┤  /tmp/sessions/         │     │
│  │                         │ Standard HTTP  │                         │     │
│  │  Volumes:               │                │  Environment:           │     │
│  │  ./all_types:/app/     │                │  FASTAPI_BASE_URL=      │     │
│  all_types             │                │  http://fastapi-        │     │
│  │                         │                │  container:8000         │     │
│  │                         │                │                         │     │
│  │                         │                │  Port: 8001 (MCP)      │     │
│  │                         │                │                         │     │
│  │                         │                │  Volumes:               │     │
│  │                         │                │  ./all_types:/app/     │     │
│  │                         │                │  all_types             │     │
│  │                         │                │  ./tmp:/tmp            │     │
│  │                         │                │                         │     │
│  └─────────────────────────┘                └─────────────────────────┘     │
│                                                         │                   │
└─────────────────────────────────────────────────────────┼───────────────────┘
                                                          │
                                    ┌─────────────────────▼───────────────────┐
                                    │            AI Agent                     │
                                    │         (Your Computer)                 │
                                    │                                         │
                                    │  🤖 PydanticAI Agent                   │
                                    │  📡 MCP Protocol Client                │
                                    │  🔗 Data Handle Manager                │
                                    │                                         │
                                    │  Connection:                            │
                                    │  • HTTP+SSE: http://localhost:8001     │
                                    │  • JSON-RPC over persistent stream     │
                                    │  • Real-time bidirectional comms       │
                                    │  • Lightweight handle-based context    │
                                    │                                         │
                                    └─────────────────────────────────────────┘

```

## Mermaid Version

```mermaid
flowchart TD
    subgraph Docker_Network["Docker Network: App Network"]
        direction TB
        subgraph fastapi["fastapi-container"]
            direction TB
            F1["🐍 Your FastAPI App"]
            F2["📊 data_fetcher.py"]
            F3["🗄️ Your Database"]
            F4["🔌 Your Endpoints"]
            F5["Port: 8000"]
            F6["Volumes: ./all_types:/app/all_types"]
        end

        subgraph toolbridge["tool-bridge-container"]
            direction TB
            T1["🤖 PydanticAI"]
            T2["🔧 Tool Definitions"]
            T3["🌐 HTTP Client"]
            T4["📡 MCP Protocol"]
            T5["💾 JSON Storage: /tmp/sessions/"]
            T6["Environment: FASTAPI_BASE_URL=http://fastapi-container:8000"]
            T7["Port: 8001 (MCP)"]
            T8["Volumes: ./all_types:/app/all_types\n./tmp:/tmp"]
        end

        fastapi -- "Standard HTTP" --> toolbridge
    end

    subgraph ai_agent["AI Agent (Your Computer)"]
        direction TB
        A1["🤖 PydanticAI Agent"]
        A2["📡 MCP Protocol Client"]
        A3["🔗 Data Handle Manager"]
        A4["Connection:\n• HTTP+SSE: http://localhost:8001\n• JSON-RPC over persistent stream\n• Real-time bidirectional comms\n• Lightweight handle-based context"]
    end

    toolbridge --> ai_agent
```
