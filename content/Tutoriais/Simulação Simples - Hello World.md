---
title: Simulação Simples - Hello World
tags:
  - simulador
  - simulação
  - verilog
draft: false
---
Para este tutorial, vamos usar os programas:
- [[Icarus Verilog]]
- [[VS Code -  Extensões]] (apesar de não fazer uso de nenhuma extensão)
- [[GTK Wave]]

O nível de conhecimento exigido é nenhum. Este tutorial é um #BabySteps  .

# Objetivo

Se trata de um projeto bem `hello_world` chinelão mesmo. Nele, vamos implementar e testar um *AND*. O objetivo principal, é exemplificar como algumas das ferramentas são utilizadas e se estão corretamente configuradas. A partir deste tutorial e um pouco de conhecimento em sistemas digitais e [[Verilog Syntax - Cheat Sheet|Verilog]], o céu é o limite.
# Projeto
## Estrutura Básica

Para esse tutorial, serão criados dois arquivos em duas pastas. Da maneira que é mostrado abaixo.
```
hello_verilog/
├── src/
│   └── hello.v
└── test/
    └── hello_tb.v
```

É uma estrutura de arquivos bem simples e intuitiva. Em `src/` é onde vamos guardar os arquivos com os nossos módulos, já tem `test/` , vamos armazenar os *Testbench*, que são instruções para testar o nosso módulo.
## Verilog

No arquivo `src/hello.v` simplesmente definimos o nosso modulo *AND*.
```verilog
module and_gate (
	    input a,
	    input b,
	    output y
	);
	    assign y = a & b;  // Operação AND
endmodule
```

As linhas 2, 3 e 4, definem as entradas e saídas do nosso módulo. Já na linha 6 a palavra reservada `assign` , é utilizada para fazer a atribuição.

O arquivo `test/hello_tb.v`  define como o nosso módulo será testado.
```verilog
`timescale 1ns/1ps
module and_gate_tb;
    reg a;
    reg b;
    wire y;

    // Instancia o módulo and_gate
    and_gate uut (
        .a(a),
        .b(b),
        .y(y)
    );

    initial begin
        // Testa várias combinações e exibe os resultados no terminal
        $monitor("a=%b b=%b -> y=%b", a, b, y);
        
        a = 0; b = 0; #10;
        a = 0; b = 1; #10;
        a = 1; b = 0; #10;
        a = 1; b = 1; #10;
        
        $finish;
    end
endmodule

```
O arquivo de teste, é um pouco mais complicado, porém nada absurdo. Na primeira linha, é especificado qual é a unidade de tempo que será adotada, neste caso 1ns. Em seguida criamos um novo módulo, geralmente com o mesmo nome do módulo que será testado, acrescido de `_tb` ao final, de *testebench*, criamos o que seria as nossas "variáveis", porém aqui não vou entrar em detalhes de como funcionam.

Em seguida, chamamos o módulo `and_gate` e usamos a palavra reservada `uut` (*Unit Under Test*) e passamos para cada entrada no nosso módulo `and_gate`, as respectivas "variáveis".

Por fim, o `initial begin [...] end` especificá o que será executado em nossa simulação. Na linha 16 o `$monitor("a=%b b=%b -> y=%b", a, b, y);` apenas informa para imprimir as "variáveis" no terminal durante a simulação, o `%b` indica que elas são do tipo binário. Em seguida, nas linhas 18, 19, 20 e 21, os 4 estados possíveis de `a` e `b` são testados, o `#10` indica que entre cada transição, irá ocorrer 10 ciclos da base de tempo que foi definida no início do documento. E o `$finish` finaliza a nossa simulação e o `endmodule` encerra o módulo de *testbench*.

## "Compilação"

Para compilar o projeto, basta executar no terminal: 
```sh
iverilog -o simulacao test/hello_tb.v src/hello.v
```

A sintaxe é muito semelhante a de outros compiladores, o `-o` indica o nome do arquivo de saída, e em seguida, passamos uma lista dos arquivos a serem "compilados".

Para executar a simulação, podemos usar:
```sh
vvp simulacao
```
E vamos obter um saída abaixo
```
a=0 b=0 -> y=0
a=0 b=1 -> y=0
a=1 b=0 -> y=0
a=1 b=1 -> y=1
test/hello_tb.v:27: $finish called at 40000 (1ps)
```

# Visualização com [[GTK Wave]]

Para utilizar o GTK Wave, primeiro precisamos gerar o arquivo `.vcd` que o programa utiliza. Para isso, adicionamos a linha abaixo dentro `initial begin` do módulo `and_gate_tb`:
```verilog
$dumpfile("and_gate_wave.vcd");
$dumpvars(0, and_gate_tb);
```

Agora, novamente compilamos e executamos a simulação.
```sh
$ iverilog -o simulacao test/hello_test.v src/hello.v 
$ vvp simulacao 
VCD info: dumpfile and_gate_wave.vcd opened for output.
a=0 b=0 -> y=0
a=0 b=1 -> y=0
a=1 b=0 -> y=0
a=1 b=1 -> y=1
test/hello_test.v:25: $finish called at 40000 (1ps)
```

E na 3ª linha, nos é informado que um arquivo `and_gate_wave.vcd` foi criado. Para abrir esse arquivo no GTK Wave, podemos usar a própria interface do programa ou simplesmente aproveitar que estamos no terminal.

```sh
gtkwave and_gate_wave.vcd
```

Na interface do programa, basta selecionar o nosso *testbanch* em **SST** e em seguida arrastar as nossas "variáveis" para `Signals->Time`. Como mostrado na imagem abaixo. Na barra superior, ao lado do botão de **Zoom In**, tem o botão de **Zoom Fit**, ao clicar nele, o zoom vai se ajustar para mostrar toda a nossa simulação.

![[janela GTK Wave.png]]
> [!abstract]- Código completo
> `src/hello.v`
> ```verilog
>	module and_gate (
>			input a,
>			input b,
>			output y
>		);
>			assign y = a & b; // Operação AND
>	endmodule
> ```
> `test/hello_test.v`
> ```verilog
>	`timescale 1ns/1ps
>		module and_gate_tb;
>		initial begin
>			$dumpfile("and_gate_wave.vcd");
>			$dumpvars(0, and_gate_tb);
>		end
>
>		reg a;
>		reg b;
>		wire y;
>		
>		// Instancia o módulo and_gate
>		and_gate uut (
>			.a(a),
>			.b(b),
>			.y(y)
>		);
>		
>		initial begin
>			// Testa várias combinações e exibe os resultados no terminal
>			$monitor("a=%b b=%b -> y=%b", a, b, y);
>			
>			a = 0; b = 0; #10;
>			a = 0; b = 1; #10;
>			a = 1; b = 0; #10;
>			a = 1; b = 1; #10;
>			        
>			$finish;
>		end
>	endmodule
> ```







