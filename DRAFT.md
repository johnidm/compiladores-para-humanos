 <!---

  * [Gramáticas Livres de Contexto](part1/context-free-grammars.md)
  -->s
  
    <!---
 
  -->

  ### Tabela de símbolos

A tabela de símbolos é uma estrutura de dados gerada pelo compilador com o objetivo de armazenar informações sobre os funções, variáveis e outros tokens usados no programa fonte.

Essa tabela normalmente armazenas informações como: tipo de dado - inteiro, string, etc. - escopo de visibilidade; limite de parâmetros, tamanho da variável. A tabela de símbolos é uma estrutura de dados do tipo tabelas hash, árvores binarias, listas lineares, etc.

O analisador léxico coleta informações sobre os tokens e seus atributos, para tokens que influenciam decisões de análise gramatical, como por exemplo identificadores, é criado uma entrada na tabela de símbolos, das quais as informações são mantidas para posterior uso. Podemos armazenar na tabela de símbolos também informações sobre a linha e coluna que o token foi examinado para em caso de erro o compilador passa informar a posição da falha.ss