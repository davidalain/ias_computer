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
    DECODE --> |"ADD M(X)
    (opcode: 00000101)"| ADD_MX
    subgraph ADD_MX ["ADD M(X)"]
        %% O conteúdo da memória no endereço indicado pelo MAR é lido e armazenado no MBR %%
        ADD_MX__A("MBR ← M(MAR)")
        %% O conteúdo do MBR é adicionado ao AC, com o resultado sendo armazenado novamente no AC %%
        ADD_MX__B("AC ← AC + MBR")

        ADD_MX__A ---> ADD_MX__B
    end

    DECODE ---> |"ADD M(X)
    (opcode: 00000111)"|ADD_MX2
    subgraph ADD_MX2 ["ADD |M(X)|"]
        %% O conteúdo da memória no endereço indicado pelo MAR é lido e armazenado no MBR %%
        ADD_MX2__A("MBR ← M(MAR)")
        %% O bit mais significativo do MBR é setado como 0
        ADD_MX2__B("MBR[0] ← 0")
        %% O conteúdo do MBR é adicionado ao AC, com o resultado sendo armazenado novamente no AC %%
        ADD_MX2__C("AC ← AC + MBR")

        ADD_MX2__A --> ADD_MX2__B
        ADD_MX2__B --> ADD_MX2__C
    end

    DECODE --> |"SUB M(X)
    (opcode: 00000110)"|SUB_MX
    subgraph SUB_MX ["SUB M(X)"]
	%% O conteúdo da memória no endereço indicado pelo MAR é lido e armazenado no MBR %%
        SUB_MX__A("MBR ← M(MAR)")
	%% O conteúdo do MBR é subtraido do AC, com o resultado sendo armazenado novamente no AC %%
        SUB_MX__B("AC ← AC - MBR")

        SUB_MX__A ---> SUB_MX__B
    end

    DECODE --> |"SUB |M(X)|
    (opcode: 00001000)"|SUB_MX2
    subgraph SUB_MX2 ["SUB |M(X)|"]
	%% O conteúdo da memória no endereço indicado pelo MAR é lido e armazenado no MBR %%
        SUB_MX2__A("MBR ← M(MAR)")
	%% O bit mais significativo do MBR é setado como 0
        SUB_MX2__B("MBR[0] ← 0")
	%% O conteúdo do MBR é subtraido do AC, com o resultado sendo armazenado novamente no AC %%
        SUB_MX2__C("AC ← AC - MBR")

        SUB_MX2__A --> SUB_MX2__B
        SUB_MX2__B --> SUB_MX2__C
    end


    DECODE --> |"MUL M(X)
    (opcode: 00001011)"|MUL_MX
    subgraph MUL_MX ["MUL M(X)"]
	%% O conteúdo da memória no endereço indicado pelo MAR é lido e armazenado no MBR %%
    	MUL_MX_A("MBR ← M(MAR)")
	%% O conteúdo do MQ é multiplicado com MBR, resultando em um número binario de 80 bits [0:79]%%
	%% A parte mais significativa [0:39] é armazenada em AC%%
	%% A parte menos significativa [40:79] é armazenada em MQ%%
    	MUL_MX_B("AC ← (MQ * MBR)[0:39]
    	MQ ← (MQ * MBR)[40:79]")
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

    DECODE --> |"LSH
    (opcode: 00010100)"|LSH_A
    subgraph LSH_A ["LSH"]
        LSH("AC ← AC << 1")
    end

    DECODE --> |"RSH
    (opcode: 00010101)"|RSH_A
    subgraph RSH_A ["RSH"]
        RSH("AC ← AC >> 1")
    end

end

subgraph "End"
    END(("Go back
    to Start")):::greenClass

    ADD_MX__B --> END
    ADD_MX2__C --> END
    SUB_MX__B --> END
    SUB_MX2__C --> END
    MUL_MX_B --> END
    DIV_MX_B --> END
    LSH_A ---> END
    RSH_A ---> END

end

classDef greenClass fill:#008000
classDef orangeClass fill:#FF6347



```
