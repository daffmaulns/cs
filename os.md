mermaid
```
sequenceDiagram
    participant A as Thread A
    participant B as Thread B
    participant C as Thread C
    participant Milk as Milk Count
    
    Note over A,B,C: All threads start at same time
    
    A->>A: noteA = 1
    B->>B: noteB = 1
    C->>C: noteC = 1
    
    A->>A: while(noteB||noteC) → WAIT
    B->>B: while(noteC) → WAIT
    
    C->>C: if(noteA==0 && noteB==0) → FALSE
    C->>C: noteC = 0 (no milk)
    
    B->>B: while(noteC) ends
    B->>B: if(noteA==0) → FALSE
    B->>B: noteB = 0 (no milk)
    
    A->>A: while(noteB||noteC) ends
    A->>Milk: milk = 1 (BUYS)
    A->>A: noteA = 0
    
    Note over Milk: Final: milk = 1
```
