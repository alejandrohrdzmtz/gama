flowchart LR
    subgraph Clients["MCP Clients"]
        A[Claude Desktop]
        B[Cursor / VSCode]
        C[Custom Agent]
    end

    subgraph Gateway["gama — Stateless Core"]
        G[Gateway Core<br/>JSON-RPC 2.0]
        O[OPA<br/>Authorization]
        T[OpenTelemetry<br/>Traces · Metrics · Logs]
    end

    subgraph Plugins["Plugin Registry"]
        P1[Plugin: DB]
        P2[Plugin: HTTP]
        P3[Plugin: Auth]
        P4[Plugin: Custom]
    end

    A --> G
    B --> G
    C --> G
    G --> O
    G --> T
    G --> P1
    G --> P2
    G --> P3
    G --> P4
