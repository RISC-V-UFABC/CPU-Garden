---
title: Usar Perfis no VS Code
tags:
  - code
draft: false
---
# Motivação
O VS Code é um excelente editor de texto, que com algumas extensões, pode substituir uma IDE sem problemas. Nele, é possível customizar de tudo e adequar ele sem problemas ao workflow do seu trabalho.

Porém a treta começa quando aquela extensão do NodeJS, começa a querer dar palpite no seu projeto de C++, ou ainda, a interface do PlatformIO fica ocupando espaço na sua tela, quando você só quer editar um script Python.

# Os Perfis
![[vscode perfis.png]]
A ideia por trás dos perfis (*Profile*)[^doc-profile], é justamente dar a possibilidade de se ter diversos ambientes de trabalhos, sem que um acabe interferindo no outro. Além de que facilita na hora de enviar uma configuração para uma outra pessoa, você pode só simplesmente copiara  pasta de configuração do perfil em específico e enviar.

[^doc-profile]: [Documentação oficial sobre *Profile* no site do VS Code](https://code.visualstudio.com/docs/editor/profiles)

Na tela de configurações dos perfis, ainda é possível escolher quais configurações a extensão vai compartilhar com a padrão, e quais pastas serão abertas com a configuração.

![[vscode perfis configuracao.png]]