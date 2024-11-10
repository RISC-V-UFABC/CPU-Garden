---
title: Simulação Simples - Hello World
tags:
  - simulador
  - simulação
  - verilog
draft: true
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



```verilog
module and_gate (
	    input a,
	    input b,
	    output y
	);
	    assign y = a & b;  // Operação AND
endmodule
```

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







