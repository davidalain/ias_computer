# Fluxograma do funcionamento do Computador IAS

<!-- <Flowchart> -->

#### Projeto orientado pelo professor [David Alain do Nascimento](https://github.com/davidalain) na disciplina **_Arquitetura de Computadores_**
#### Curso Superior de Tecnologia em **_Análise e Desenvolvimento de Sistemas (ADS)_** 
#### IFPE Campus Garanhuns
#### Diagrama desenvolvido pelos estudantes:

- [José Otávio Gurgel Souto](https://github.com/OTGSJ)
- [Fagner Timoteo da Silva](https://github.com/Othergamer1)
- [João Pedro Bezerra Oliveira](https://github.com/yJPBO)


```mermaid
---
title:  Fluxograma do funcionamento do Computador IAS
---
flowchart TB

%% José Otávio Gurgel Souto
%% Fagner Timoteo da Silva
%% João Pedro Bezerra Oliveira

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
end

subgraph FC[ ]
    I --> DECODE
    G --> DECODE
    DECODE{{"Decode instruction in IR"}}:::orangeClass
    %%DECODE --- link[ ]
end

style FC fill:transparent,stroke:transparent

    subgraph execution_cycle [Execution Cycle]

        DECODE --->|"LOAD MQ
        OPCODE: 00001010"| loadmq_sp

        subgraph loadmq_sp ["LOAD MQ"];
        %%Transfere o valor de MQ para AC
            loadmq01["AC ← MQ"]
            direction TB
        end

        DECODE --->|"LOAD MQ,M(X)
        OPCODE: 00001001"| loadmqmx_sp

        subgraph loadmqmx_sp ["LOAD MQ, M(X)"];
        %%Transfere o valor de M(X) para MQ
             loadmqmx01["MBR ← M(MAR)"]
             loadmqmx02["MQ ← MBR"]

             loadmqmx01 --> loadmqmx02
             direction TB
        end

        DECODE --->|"STOR M(X)
        OPCODE: 00100001"| stormx_sp

        subgraph stormx_sp ["STOR M(X)"];
        %%Transfere o valor de AC para para M(X)
             stormx01["MBR ← AC"]
             stormx02["M(MAR) ← MBR"]

             stormx01 --> stormx02
             direction TB
        end

        DECODE --->|"LOAD M(X)
        OPCODE: 00000001"| loadmx_sp

        subgraph loadmx_sp["LOAD MX"];
        %%Transfere o valor de M(X) para AC
             loadmx01["MBR ← M(MAR)"]
             loadmx02["AC ← MBR"]

             loadmx01 --> loadmx02
             direction TB
        end

        DECODE --->|"LOAD - M(X)
        OPCODE: 00000010"| loadminusmx_sp

        subgraph loadminusmx_sp ["LOAD - M(X)"];
        %%Transfere o valor negativo de M(X) para AC 
             loadminusmx01["MBR ← - M(MAR)"]
             loadminusmx02["AC ← MBR"]

             loadminusmx01 --> loadminusmx02
             direction TB
        end

        DECODE --->|"LOAD |M(X)|
        OPCODE: 00000011"| loadabsolutemx_sp

        subgraph loadabsolutemx_sp["LOAD |M(X)|"];
        %%Transfere o valor absoluto de M(X) para AC
             loadabsolutemx01["MBR ← |M(MAR)|"]
             loadabsolutemx02["AC ← MBR"]

             loadabsolutemx01 --> loadabsolutemx02
             direction TB
        end

        DECODE --->|"LOAD - |M(X)|
        OPCODE: 00000100"| loadminusabsolutemx_sp

        subgraph loadminusabsolutemx_sp["LOAD - |M(X)|"];
        %%Transfere o valor absoluto negativo de M(X) para AC
             loadminusabsolutemx01["MBR ← - |M(MAR)|"]
             loadminusabsolutemx02["AC ← MBR"]

             loadminusabsolutemx01 --> loadminusabsolutemx02
             direction TB
        end
    end


    subgraph "End"
    END(("Go back
    to Start")):::greenClass

    style End fill:transparent,stroke:transparent

    loadmq_sp --- END
    loadmqmx_sp --- END
    stormx_sp --- END
    loadmx_sp--- END
    loadminusmx_sp --- END
    loadabsolutemx_sp --- END
    loadminusabsolutemx_sp --- END
end

classDef greenClass fill:#008000
classDef orangeClass fill:#FF6347


```

