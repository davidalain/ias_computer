# ias_computer

<!-- <Flowchart> -->

### Fluxograma do funcionamento do Computador IAS

```mermaid
---
title: Flowchart of IAS Operation
---
flowchart TD

subgraph "Fetch Cycle"
    START((Start)):::greenClass --> B{"Is next 
    instruction 
    in IBR?"}
    B --> |"Yes

    No memory access
    required"| F("IR ← IBR(0:7)
    MAR ← IBR(8:19)")
    F --> I
    B --> |No| C("MAR ← PC")
    C --> D("MBR ← M(MAR)")
    D --> E{"Left
    instruction
    required?"}
    E --> |No| H("IR ← MBR(20:27)
    MAR ← MBR(28:39)")
    E --> |Yes| G("IBR ← MBR(20:39)
    IR ← MBR(0:7)
    MAR ← MBR(8:19)")
    H --> I("PC ← PC + 1")
    I --> DECODE
    G --> DECODE
    DECODE{{"Decode instruction in IR"}}:::orangeClass
end

subgraph "Execution Cycle"
    subgraph "LOAD MQ"
        LOAD_MQ__A("MBR ← M(MAR)")
        LOAD_MQ__B("AC ← MBR")

        DECODE --> |"LOAD MQ
        (opcode: 00001010)"|LOAD_MQ__A
        LOAD_MQ__A --> LOAD_MQ__B
    end

    subgraph "LOAD MQ,M(X)"
        LOAD_MQ_MX__A("MBR ← M(MAR)")
        LOAD_MQ_MX__B("MQ ← MBR")

        DECODE --> |"LOAD MQ,M(X)
        (opcode: 00001001)"|LOAD_MQ_MX__A
        LOAD_MQ_MX__A --> LOAD_MQ_MX__B
    end
end

subgraph "End"
    END(("Go to
    Start")):::greenClass

    LOAD_MQ__B --> END
    LOAD_MQ_MX__B --> END
end

classDef greenClass fill:#008000
classDef orangeClass fill:#FF6347



```
