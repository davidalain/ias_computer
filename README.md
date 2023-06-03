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
    subgraph "ADD M(X)"
        ADD_MX__A("MBR ← M(MAR)")
        ADD_MX__B("MBR ← M(MAR)")

        DECODE --> |"ADD M(X)
        (opcode: 00000101)"| ADD_MX__A --> ADD_MX__B
    end

        DECODE ---> |"ADD M(X)
        (opcode: 00000111)"|ADD_MX2
    subgraph ADD_MX2 ["ADD |M(X)|"]
        ADD_MX2__A("MBR ← M(MAR)")
        ADD_MX2__B("MBR[0] ← 0")
        ADD_MX2__C("AC ← AC + MBR")
        
        ADD_MX2__A --> ADD_MX2__B
        ADD_MX2__B --> ADD_MX2__C
    end

    subgraph "MUL M(X)"
    MUL_MX_A("MBR ← M(MAR)")
    MUL_MX_B("AC ← (MQ + MBR)[0:39]
    MQ ← (MQ + MBR)[40:79]")

    DECODE --> |"MUL M(X)
    (opcode: 00001011)"|MUL_MX_A
    MUL_MX_A --> MUL_MX_B
    end

    subgraph "DIV M(X)"
    DIV_MX_A("MBR ← M(MAR)")
    DIV_MX_B("MQ ← AC/MBR
    AC ← AC%MBR")

    DECODE --> |"DIV M(X)
    (opcode: 00001100)"|DIV_MX_A
    DIV_MX_A --> DIV_MX_B
    end
end

subgraph "End"
    END(("Go back
    to Start")):::greenClass

    ADD_MX__B --> END
    ADD_MX2__C --> END
    MUL_MX_B --> END
    DIV_MX_B --> END
end
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
    DECODE --> |"ADD M(X)
    (opcode: 00000101)"| ADD_MX
    subgraph ADD_MX ["ADD M(X)"]
        ADD_MX__A("MBR ← M(MAR)")
        ADD_MX__B("MBR ← M(MAR)")
        ADD_MX__A ---> ADD_MX__B
    end

    DECODE ---> |"ADD M(X)
    (opcode: 00000111)"|ADD_MX2
    subgraph ADD_MX2 ["ADD |M(X)|"]
        ADD_MX2__A("MBR ← M(MAR)")
        ADD_MX2__B("MBR[0] ← 0")
        ADD_MX2__C("AC ← AC + MBR")
        ADD_MX2__A --> ADD_MX2__B
        ADD_MX2__B --> ADD_MX2__C
    end

    DECODE --> |"MUL M(X)
    (opcode: 00001011)"|MUL_MX
    subgraph MUL_MX ["MUL M(X)"]
    MUL_MX_A("MBR ← M(MAR)")
    MUL_MX_B("AC ← (MQ + MBR)[0:39]
    MQ ← (MQ + MBR)[40:79]")
    MUL_MX_A ---> MUL_MX_B
    end

    DECODE ---> |"DIV M(X)
    (opcode: 00001100)"|DIV_MX
    subgraph DIV_MX ["DIV M(X)"]
    DIV_MX_A("MBR ← M(MAR)")
    DIV_MX_B("MQ ← AC/MBR
    AC ← AC%MBR")
    DIV_MX_A ---> DIV_MX_B
    end
end

subgraph "End"
    END(("Go back
    to Start")):::greenClass

    ADD_MX__B --> END
    ADD_MX2__C --> END
    MUL_MX_B --> END
    DIV_MX_B --> END
end

classDef greenClass fill:#008000
classDef orangeClass fill:#FF6347




```
