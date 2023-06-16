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

   subgraph "STOR M(X, 8:19)"

        STOR_MXL1("MBR ← M(MAR)")
        STOR_MXL2("MBR(8:19) ← AC(28:39)")
        STOR_MXL3("M(MAR) ← MBR")

        DECODE --> |"STOR M(X,8:19)
        (opcode: 00010010)"|STOR_MXL1
        STOR_MXL1 --> STOR_MXL2
        STOR_MXL2 --> STOR_MXL3
    end

    subgraph "STOR M(X, 28:39)"
                    <!-- Essa instrução serve para substituir os bits localizados 
            a direita de m(x), ou seja, do bit 28 a 39 por respectivamente os bits do 28 a 39 do AC.
            Mas para isso o processo passa pelo MBR, pois só ele tem acesso direto a memória.
            Sendo assim, todo o conteúdo de m(mar) é passado para o MBR, que substituirá o atual conteudo recebido do bit 28 ao 39 pelos os bit do 28 a 39 do AC.
            Feita essa alteração no MBR, é possível agora transferir esse conteudo atualizado para o m(mar). E assim é feita a modificação do lado direito de m(x). -->
        STOR_MXR1("MBR ← M(MAR)")
        STOR_MXR2("MBR(28:39) ← AC(28:39)")
        STOR_MXR3("M(MAR) ← MBR")

        DECODE --> |"STOR M(X,28:39)
        (opcode: 00010011)"|STOR_MXR1
        STOR_MXR1 --> STOR_MXR2
        STOR_MXR2 --> STOR_MXR3
    end

    subgraph "JUMP M(X, 0:19)"

        JUMP_MXL1("IBR ← M(MAR,0:19)")
        JUMP_MXL2("PC ← MAR")
        
        DECODE --> |"JUMP M(X, 0:19)
        (opcode: 00001101)"|JUMP_MXL1
        JUMP_MXL1 --> JUMP_MXL2
    end

    subgraph "JUMP M(X, 20:39)"

        JUMP_MXR1("IBR ← M(MAR,20:39)")
        JUMP_MXR2("PC ← MAR")
        
        DECODE --> |"JUMP M(X, 20:39)
        (opcode: 00001101)"|JUMP_MXR1
        JUMP_MXR1 --> JUMP_MXR2
    end

    subgraph "JUMP+ M(X, 0:19)"

        JUMP1_MXL1{"AC>= 0"}
        JUMP1_MXL1 --> |Yes| JUMP1_MXL2("PC ← MAR")
        JUMP1_MXL1--> |NO| JUMP1_MXL3("AC ← AC + M(X)")
        
        DECODE --> |"JUMP+ M(X, 0:19)
        (opcode: 00001111)"|JUMP1_MXL1
        JUMP1_MXL1 
    end

    subgraph "JUMP+ M(X, 20:39)"

        JUMP1_MXR1{"AC>= 0"}
        JUMP1_MXR1 --> |Yes| JUMP1_MXR2("PC ← MAR")
        JUMP1_MXR1 --> |NO| JUMP1_MXR3("AC ← AC + M(X)")
        DECODE --> |"JUMP+ M(X, 20:39)
        (opcode: 00010000)"|JUMP1_MXR1
        JUMP1_MXR1 
    end



end

subgraph "End"
    END(("Go back
    to Start")):::greenClass

    STOR_MXL3 --> END
    STOR_MXR3 --> END
    JUMP_MXL2 --> END
    JUMP_MXR2 --> END
    JUMP1_MXL2 --> END
    JUMP1_MXL3 --> END
    JUMP1_MXR2 --> END
    JUMP1_MXR3 --> END
    
end

classDef greenClass fill:#008000
classDef orangeClass fill:#FF6347



```