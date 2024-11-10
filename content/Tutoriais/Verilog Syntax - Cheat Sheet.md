---
title: Verilog Syntax - Cheat Sheet
tags:
  - verilog
draft: true
---
# Básico
## Estrutura de Módulo
```verilog
// Definição de um módulo básico
module NomeDoModulo(entrada1, entrada2, saida);
// Declaração das portas de entrada e saída
input entrada1, entrada2;
output saida;

// Código interno do módulo

endmodule
```
## Tipos de Dados Básicos
```verilog
// Declarações de tipos de dados
reg variavel_reg;     // Armazena valores, usado para flip-flops
wire variavel_wire;   // Conexões, usado para fios e lógica combinacional

// Definindo vetores (arrays) com bit width
reg [3:0] vetor4bits; // Vetor de 4 bits
wire [7:0] vetor8bits; // Vetor de 8 bits
```

## Operadores
- **Aritméticos**: `+`, `-`, `*`, `/`, `%`
- **Relacionais**: `==`, `!=`, `>`, `<`, `>=`, `<=`
- **Lógicos**: `&&`, `||`, `!`
- **Bitwise**: `&`, `|`, `~`, `^`, `^~` (XOR e XNOR)
- **Shift**: `<<`, `>>`

## Atribuições
- **Atribuição Contínua** (`assign`): Usado para circuitos combinacionais.
    ```verilog
    assign saida = entrada1 & entrada2; // Operação AND
    ```
- **Atribuição Procedural** (`always`): Usado em blocos `always` para registradores (flip-flops).
    ```verilog
    always @(posedge clk) begin
        variavel_reg <= entrada1; // Atribuição a cada borda de subida do clk
    end
    ```

## Estruturas de Controle
- **Bloco `always`**: Executado sempre que as variáveis dentro da sensibilidade mudam.
    ```verilog
    always @(condição_de_sensibilidade) begin
        // Código executado aqui
    end
    ```
  
- **`if` e `else`**: Condicional para controle de fluxo.
    ```verilog
    always @(posedge clk) begin
        if (condicao)
            saida <= valor1;
        else
            saida <= valor2;
    end
    ```

- **`case`**: Estrutura de seleção múltipla.
    ```verilog
    always @(variavel) begin
        case (variavel)
            2'b00: saida = valor1;
            2'b01: saida = valor2;
            2'b10: saida = valor3;
            default: saida = valorPadrao;
        endcase
    end
    ```

## Estruturas de Bloco
- **Bloco `initial`**: Executa uma única vez no início da simulação.
    ```verilog
    initial begin
        variavel_reg = 0;
        #10 variavel_reg = 1; // Após 10 unidades de tempo, altera o valor
    end
    ```

- **Bloco `always`**: Executa repetidamente ao longo da simulação.
    ```verilog
    always @(posedge clk) begin
        // Código executado a cada borda de subida do clk
    end
    ```

## Estrutura de Sensibilidade
- **Posedge** e **Negedge**: Bordas de subida e descida do sinal.
    ```verilog
    always @(posedge clk) begin
        // Executa a cada borda de subida
    end

    always @(negedge clk) begin
        // Executa a cada borda de descida
    end
    ```

# Exemplos Completos

## Exemplo 1: Flip-Flop D
```verilog
module DFlipFlop (
    input clk,
    input d,
    output reg q
);

    always @(posedge clk) begin
        q <= d; // Atualiza q com o valor de d na borda de subida do clk
    end

endmodule
```

## Exemplo 2: Multiplexador 2:1

```verilog
module Multiplexador2to1 (
    input a,
    input b,
    input sel,
    output y
);

    assign y = sel ? b : a; // Seleciona b se sel=1, caso contrário seleciona a

endmodule
```

## Exemplo 3: Contador de 4 bits

```verilog
module Contador4bits (
    input clk,
    input reset,
    output reg [3:0] q
);

    always @(posedge clk or posedge reset) begin
        if (reset)
            q <= 4'b0000; // Reseta o contador
        else
            q <= q + 1; // Incrementa o contador
    end

endmodule
```

# Simulação
- **Testbench**: Código para testar um módulo em simulação.
```verilog
    module Testbench();
        reg clk, d;
        wire q;

        DFlipFlop uut (.clk(clk), .d(d), .q(q)); // Instancia o módulo

        initial begin
            // Inicializa sinais
            clk = 0;
            d = 0;
            
            // Teste de estímulos
            #10 d = 1;
            #10 d = 0;
            #10 d = 1;
            #20;
            $stop; // Termina a simulação
        end

        always #5 clk = ~clk; // Gera clock de 10 unidades de tempo

    endmodule
```

Para criar um *Testbench* específico em Verilog, vou usar um exemplo de um módulo contador de 4 bits para ilustrar como fazer uma simulação de teste. Um *Testbench* permite verificar se o módulo está funcionando corretamente, fornecendo estímulos e observando as saídas.

### Exemplo Completo de um Testbench para um Contador de 4 bits

1. **Módulo Contador de 4 bits** - Vamos criar um contador básico que incrementa com cada pulso de clock e tem uma entrada de reset.
2. **Testbench** - Um módulo separado usado exclusivamente para simulação, onde controlamos os sinais e verificamos a resposta do contador.

### 1. Código do Módulo do Contador (Exemplo)
```verilog
module Contador4bits (
    input clk,
    input reset,
    output reg [3:0] q
);

    always @(posedge clk or posedge reset) begin
        if (reset)
            q <= 4'b0000;  // Reseta o contador
        else
            q <= q + 1;    // Incrementa o contador em cada borda de subida do clock
    end

endmodule
```

### 2. Código do Testbench para o Contador de 4 bits
O *Testbench* cria o sinal de clock, gera o sinal de reset e observa a saída do contador.

```verilog
module Testbench;
    // Declaração dos sinais que serão conectados ao módulo do contador
    reg clk;
    reg reset;
    wire [3:0] q;

    // Instancia o módulo do contador (Unidade de Teste, UUT)
    Contador4bits uut (
        .clk(clk),
        .reset(reset),
        .q(q)
    );

    // Inicializa o clock e os sinais de teste
    initial begin
        // Inicia com reset ativado e clock desligado
        clk = 0;
        reset = 1;
        
        // Espera 10 unidades de tempo com reset ativado
        #10 reset = 0;    // Desativa reset para iniciar o contador

        // Espera mais tempo para observar o contador em operação
        #100 reset = 1;   // Ativa reset para reiniciar o contador
        #10 reset = 0;    // Desativa reset novamente

        // Finaliza a simulação após algum tempo
        #100;
        $stop;
    end

    // Gera um pulso de clock a cada 5 unidades de tempo
    always #5 clk = ~clk;

endmodule
```

### Explicação do Testbench

- **Geração de Clock**: O `always #5 clk = ~clk;` cria um clock que alterna a cada 5 unidades de tempo, resultando em um período de 10 unidades de tempo.
- **Reset Inicial**: `reset` é ativado no início para garantir que o contador comece do zero. Após 10 unidades de tempo, o `reset` é desativado para permitir que o contador comece a contar.
- **Verificação do Comportamento**: O `#100 reset = 1;` aplica um novo pulso de reset para verificar se o contador retorna a zero conforme esperado, e então o reset é desativado novamente.
- **Finalização da Simulação**: `#100; $stop;` encerra a simulação após um período, permitindo observar o comportamento do contador.

### Como Executar o Testbench

Para simular o *Testbench*, basta utilizar uma ferramenta de simulação como o ModelSim, Icarus Verilog, ou Vivado. Isso fornecerá uma saída detalhada com as transições de `q` em função do clock e dos sinais de `reset`, facilitando a verificação de funcionalidade do contador.
