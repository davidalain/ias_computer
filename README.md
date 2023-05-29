# ias_computer

<!-- <Flowchart> -->

### Fluxograma do funcionamento do Computador IAS

```mermaid
---
title: Flowchart of IAS Operation
---
flowchart TD

subgraph "Fetch Cycle"
START([Start]) --> B{"Is next 
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
I --> AA
G --> AA
AA["Decode instruction in IR"]
%% LOAD_MQ("LOAD MQ")
%% AA --> LOAD_MQ
%% AA --> LOAD_MQ_MX
end

subgraph "LOAD MQ"
LOAD_MQ__A("MBR ← M(MAR)")
LOAD_MQ__B("AC ← MBR")

AA --> |"LOAD MQ
(opcode: 00001010)"|LOAD_MQ__A
LOAD_MQ__A --> LOAD_MQ__B
end

subgraph "LOAD MQ,M(X)"
%% LOAD_MQ_MX("LOAD MQ,M(X)")
LOAD_MQ_MX__A("MBR ← M(MAR)")
LOAD_MQ_MX__B("MQ ← MBR")

AA --> |"LOAD MQ,M(X)
(opcode: 00001001)"|LOAD_MQ_MX__A
LOAD_MQ_MX__A --> LOAD_MQ_MX__B
end

subgraph "End"
END(["Go back to 
Start"])

LOAD_MQ__B --> END
LOAD_MQ_MX__B --> END
end


```
