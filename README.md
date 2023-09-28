# Fluxograma do funcionamento do Computador IAS

<!-- <Flowchart> -->

#### Projeto orientado pelo professor [David Alain do Nascimento](https://github.com/davidalain) na disciplina **_Arquitetura de Computadores_**
#### Curso Superior de Tecnologia em **_Análise e Desenvolvimento de Sistemas (ADS)_** 
#### IFPE Campus Garanhuns
#### Diagrama desenvolvido pelos estudantes:

##### Instruções de transferência de dados
- [José Otávio Gurgel Souto](https://github.com/OTGSJ)
- [Fagner Timoteo da Silva](https://github.com/Othergamer1)
- [João Pedro Bezerra Oliveira](https://github.com/yJPBO)

##### Instruções de desvio condicional, desvio incondicional e modificação de endereço
- [Eliane de Melo Cordeiro](https://github.com/ElianeCordeiro)
- [Flávio Filipe França Farias](https://github.com/trewq010)
- [Maria Eduarda Veloso Cânha](https://github.com/dudacanha)
- [Laura Maria Farias Silva](https://github.com/laura-farias-dev)
- [Wanessa Santana Ferreira](https://github.com/Wanessaa)

```mermaid
---
title:  Fluxograma do funcionamento do Computador IAS
---
flowchart TB

subgraph _fetch_cycle_ [Fetch Cycle]
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
    H --> I("PC ← PC + 1
    Reset IBR")
end

subgraph _decode_ [ ]
    I --> DECODE
    G --> DECODE
    DECODE{{"Decode instruction in IR"}}:::orangeClass

    style _decode_ fill:transparent,stroke:transparent

    direction TB
end


subgraph _execution_cycle_ [Execution Cycle]

    %%%%  Instruções de transferência de dados
    %% [José Otávio Gurgel Souto](https://github.com/OTGSJ)
    %% [Fagner Timoteo da Silva](https://github.com/Othergamer1)
    %% [João Pedro Bezerra Oliveira](https://github.com/yJPBO)

    DECODE --->|"LOAD MQ
    Opcode: 00001010"| loadmq_sp

    subgraph loadmq_sp ["LOAD MQ"];
    %%Transfere o valor de MQ para AC
        loadmq01["AC ← MQ"]
        direction TB
    end

    DECODE --->|"LOAD MQ,M(X)
    Opcode: 00001001"| loadmqmx_sp

    subgraph loadmqmx_sp ["LOAD MQ,M(X)"];
    %%Transfere o valor de M(X) para MQ
            loadmqmx01["MBR ← M(MAR)"]
            loadmqmx02["MQ ← MBR"]

            loadmqmx01 --> loadmqmx02
            direction TB
    end

    DECODE --->|"STOR M(X)
    Opcode: 00100001"| stormx_sp

    subgraph stormx_sp ["STOR M(X)"];
    %%Transfere o valor de AC para para M(X)
            stormx01["MBR ← AC"]
            stormx02["M(MAR) ← MBR"]

            stormx01 --> stormx02
            direction TB
    end

    DECODE --->|"LOAD M(X)
    Opcode: 00000001"| loadmx_sp

    subgraph loadmx_sp["LOAD MX"];
    %%Transfere o valor de M(X) para AC
            loadmx01["MBR ← M(MAR)"]
            loadmx02["AC ← MBR"]

            loadmx01 --> loadmx02
            direction TB
    end

    DECODE --->|"LOAD -M(X)
    Opcode: 00000010"| loadminusmx_sp

    subgraph loadminusmx_sp ["LOAD -M(X)"];
    %%Transfere o valor negativo de M(X) para AC 
            loadminusmx01["MBR ← -M(MAR)"]
            loadminusmx02["AC ← MBR"]

            loadminusmx01 --> loadminusmx02
            direction TB
    end

    DECODE --->|"LOAD |M(X)|
    Opcode: 00000011"| loadabsolutemx_sp

    subgraph loadabsolutemx_sp["LOAD |M(X)|"];
    %%Transfere o valor absoluto de M(X) para AC
            loadabsolutemx01["MBR ← |M(MAR)|"]
            loadabsolutemx02["AC ← MBR"]

            loadabsolutemx01 --> loadabsolutemx02
            direction TB
    end

    DECODE --->|"LOAD -|M(X)|
    Opcode: 00000100"| loadminusabsolutemx_sp

    subgraph loadminusabsolutemx_sp["LOAD -|M(X)|"];
    %%Transfere o valor absoluto negativo de M(X) para AC
            loadminusabsolutemx01["MBR ← -|M(MAR)|"]
            loadminusabsolutemx02["AC ← MBR"]

            loadminusabsolutemx01 --> loadminusabsolutemx02
            direction TB
    end

    %%%% Instruções de desvio condicional, desvio incondicional e modificação de endereço
    %% [Eliane de Melo Cordeiro](https://github.com/ElianeCordeiro)
    %% [Flávio Filipe França Farias](https://github.com/trewq010)
    %% [Maria Eduarda Veloso Cânha](https://github.com/dudacanha)
    %% [Laura Maria Farias Silva](https://github.com/laura-farias-dev)
    %% [Wanessa Santana Ferreira](https://github.com/Wanessaa)

    DECODE --> |"STOR M(X, 8:19)
    Opcode: 00010010" | STOR_MXL

    %% A instrução STOR M(X, 8:19) substitui os bits de endereço da instrução esquerda, do bit 28 ao 39, de um local de memória chamado M(X), pelos bits do 28 ao 39 do acumulador (AC). Para isso, o processo passa pelo Registrador de Memória do Barramento (MBR), pois o AC não tem acesso direto à memória. Dessa forma, o conteúdo da memória tem que ser transferido para o MBR, que pode acessar tanto a memória quanto o AC. Então, o MBR substitui os bits do 8 ao 19 pelo conteúdo dos bits do 28 ao 39 do AC. Após a modificação, o conteúdo atualizado é transferido de volta para a memória M(X), finalizando o processo de modificação do campo de endereço da instrução esquerda em M(X).

    subgraph STOR_MXL ["STOR M(X, 8:19)"]
        STOR_MXL1("MBR ← M(MAR)")
        STOR_MXL2("MBR(8:19) ← AC(28:39)")
        STOR_MXL3("M(MAR) ← MBR")

        STOR_MXL1 --> STOR_MXL2
        STOR_MXL2 --> STOR_MXL3
        direction TB
    end


    DECODE --> |"STOR M(X, 28:39)
    Opcode: 00010011"|STOR_MXR 

    %% A instrução STOR M(X, 28:39) substitui os bits de endereço na instrução direita, do bit 28º ao 39º, de um local de memória chamado M(X), pelos respectivos bits do 28º ao 39º do acumulador (AC). Para isso, o processo passa pelo Registrador de Memória do Barramento (MBR), pois o AC não tem acesso direto à memória. Dessa forma, o conteúdo da memória tem que ser transferido para o MBR, que pode acessar tanto a memória quanto o AC.Então, o MBR substitui os bits do 28º ao 39º pelo conteúdo correspondente do AC. Após a modificação, o conteúdo atualizado é transferido de volta para a memória M(X), finalizando o processo de modificação do campo de endereço da instrução direita em M(X).

    subgraph STOR_MXR ["STOR M(X, 28:39)"]
        STOR_MXR1("MBR ← M(MAR)")
        STOR_MXR2("MBR(28:39) ← AC(28:39)")
        STOR_MXR3("M(MAR) ← MBR")

        STOR_MXR1 --> STOR_MXR2
        STOR_MXR2 --> STOR_MXR3
        direction TB
    end


    DECODE --> |"JUMP M(X, 0:19)
    Opcode: 00001101"|JUMP_ML

   %% A instrução JUMP M(X, 0:19), ao receber o endereço de memória (MAR) do ciclo de busca, acessa a palavra de memória que contém duas intruções e armazena esse conteúdo no MBR. O endereço de memória presente na instrução localizada à esquerda desse conteúdo é lido e armazenado no contador de programa (PC). O que indica que o conteúdo apontado por esse endereço será executado no próximo ciclo de busca, independente do conteúdo do registrador IBR ou o que estava armazenado anteriormente em PC.

    subgraph JUMP_ML ["JUMP M(X, 0:19)"]
	    JUMP_MXL1("MBR ← M(MAR)")
        JUMP_MXL2("MAR ← MBR(8:19)")
        JUMP_MXL3("PC ← MAR")
        JUMP_MXL4("Reset IBR")

        JUMP_MXL1 --> JUMP_MXL2
        JUMP_MXL2 --> JUMP_MXL3
        JUMP_MXL3 --> JUMP_MXL4
        direction TB
    end


    DECODE --> |"JUMP M(X, 20:39)
    Opcode: 00001110"|JUMP_MR

    %% A instrução JUMP M(X, 20:39),  ao receber o endereço de memória (MAR) do ciclo de busca, acessa a palavra de memória que contém duas intruções e armazena esse conteúdo no MBR. O endereço de memória presente na instrução localizada à direita desse conteúdo é lido e armazenado no contador de programa (PC). O que indica que o conteúdo apontado por esse endereço será executado no próximo ciclo de busca, independente do conteúdo do registrador IBR ou o que estava armazenado anteriormente em PC.

    subgraph JUMP_MR ["JUMP M(X, 20:39)"]
        JUMP_MXR1("MBR ← M(MAR)")
        JUMP_MXR2("MAR ← MBR(28:39)")
        JUMP_MXR3("PC ← MAR")
        JUMP_MXR4("Reset IBR")

        JUMP_MXR1 --> JUMP_MXR2
        JUMP_MXR2 --> JUMP_MXR3
        JUMP_MXR3 --> JUMP_MXR4
        direction TB
    end


    DECODE ----> |"JUMP+ M(X, 0:19)
    Opcode: 00001111"|JUMP+_ML

    %% A instrução JUMP+ M(X, 0:19) tem o efeito de saltar para a instrução esquerda da memória apenas se o valor contido no registrador AC for maior ou igual a zero, indicando que AC não é um número negativo. Caso contrário, se o valor em AC for negativo, o fluxo de execução continua normalmente, mantendo os mesmos valores nos registradores.

    subgraph JUMP+_ML ["JUMP+ M(X, 0:19)"]
        JUMP1_MXL1{"AC >= 0"}
        JUMP1_MXL2("MBR ← M(MAR)")
        JUMP1_MXL3("MAR ← MBR(8:19)")
        JUMP1_MXL4("PC ← MAR")
        JUMP1_MXL5("Reset IBR")
        DO_NOTHING_L("Do nothing")
       
        JUMP1_MXL1 --> |Yes| JUMP1_MXL2
        JUMP1_MXL1 --> |No| DO_NOTHING_L
        JUMP1_MXL2 -->  JUMP1_MXL3
        JUMP1_MXL3 -->  JUMP1_MXL4
        JUMP1_MXL4 -->  JUMP1_MXL5
        direction TB
    end


    DECODE ----> |"JUMP+ M(X, 20:39)
    Opcode: 00010000"|JUMP+_MR

    %% A instrução JUMP+ M(X, 20:39) tem o efeito de saltar para a instrução direita da memória apenas se o valor contido no registrador AC for maior ou igual a zero, indicando que AC não é um número negativo. Caso contrário, se o valor em AC for negativo, o fluxo de execução continua normalmente, mantendo os mesmos valores nos registradores.  
      
    subgraph JUMP+_MR ["JUMP+ M(X, 20:39)"]
        JUMP1_MXR1{"AC >= 0"}
        JUMP1_MXR2("MBR ← M(MAR)")
        JUMP1_MXR3("MAR ← MBR(28:39)")
        JUMP1_MXR4("PC ← MAR")
        JUMP1_MXR5("Reset IBR")
        DO_NOTHING_R("Do nothing")

        JUMP1_MXR1 --> |Yes| JUMP1_MXR2
        JUMP1_MXR1 --> |No| DO_NOTHING_R
        JUMP1_MXR2 -->  JUMP1_MXR3
        JUMP1_MXR3 -->  JUMP1_MXR4
        JUMP1_MXR4 -->  JUMP1_MXR5

        direction TB
    end


end

subgraph _end_ [End]
    style _end_ fill:transparent,stroke:transparent

    END(("Go back
    to Start")):::greenClass

    loadmq_sp --- END
    loadmqmx_sp --- END
    stormx_sp --- END
    loadmx_sp--- END
    loadminusmx_sp --- END
    loadabsolutemx_sp --- END
    loadminusabsolutemx_sp --- END

    STOR_MXL --- END
    STOR_MXR --- END
    JUMP_ML --- END
    JUMP_MR --- END
    JUMP+_ML --- END
    JUMP+_MR --- END

    direction TB
end

classDef greenClass fill:#008000
classDef orangeClass fill:#FF6347

```


