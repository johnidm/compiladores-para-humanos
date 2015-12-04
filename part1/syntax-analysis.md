Analise Sintática
======

### Introdução

O Analisador sintático também conhecido como *parser* tem como tarefa principal determinar se o programa de entrada representado pelo fluxo de tokens é uma sentança valida na linaugem de programação 

A analise sintática e a segunda etapa do processo de compilação e utiliza gramáticas livres de contexto para especificar a sintaxe de uma lingaugem de programacao.

### Visao geral

O que é sintaxe? Parte da gramática que estuda a disposição das palavras na frase e das frases no discurso, bem como a relação lógica das frases entre si.

Essa estapa do processo de compilação deve reconhecer a sintaxe do programa fonte e determinar se ela é valida ou não, esse modele de sintaxe pode ser definido utilizando gramaticas livres de contexto que refresental a gramática formal, o algoritmo tenta deriver todas as possíveis contruções da lingaugem. As derivações tem como objetivo determinar se um fluxo de palavras se encaixa na sintaxe da linguagem de programação.

Alguns termos são utilizados em linguagens de programação para facilitar o seu entendimento.

* Símbolo: são os elementos mínimos que compõe uma linguagem. Na linguagem humana são as letras. 
* Sentença: É um conjunto ordenado de símbolos que forma uma cadeia ou string.  Na linguagem humana são as palavras.
* Alfabeto: É um conjunto de símbolos. Na linguagem humana é o conjunto de letras {a, b, c, d, ...} 
* Linguagem: É o conjunto de sentenças, Na linguagem humana são os conjuntos de palavras {compiladores, linguagem, ...}
* Gramática: É uma forma de representar as regras para formação de uma linguagem.

Dada uma GLC “G” e uma sentença “s” o objetivo do analisador sintático é verificar se a sentença “s” pertence a linguagem “G”. O analisador sintático também é conhecido como parser e recebe do analisador léxico a sequência de tokens que constitui a sentença “s” e produz uma arvore de derivação se a sentença é válida ou emite um erro sintático. 

O analisador sintático deve ser projetado para que a análise seja feita até o fim do programa mesmo que encontre erros no texto do programa fonte.

O analisador léxico é desenvolvido para reconhecer os tokens fazendo uma leitura dos caracteres e obtendo a sequência de tokens, esse analisador vê o texto como uma sequência de palavras de uma linguagem regular e reconhece ele através de um autômato finito. 

Já o analisador sintático vê o mesmo texto como uma sequência de sentenças que deve satisfazer as regras gramaticais. É através da gramatica que podemos validar expressões criadas na linguagem de programação. O analisador sintático agrupa os tokens em frases gramaticais usadas pelo compilador com o objetivo de criar uma saída que é uma estrutura de dados que possui uma hierarquia da entrada a árvore de derivação.

![](../images/ast.png)

Observe estrutura sintática de uma linguagem de programação. Temos as divisões dos blocos, compostos por comandos, compostos por expressões, os tokens.

![](../images/source-code-java.png)

Entende-se por regras gramaticas as formas como podemos descrever a estrutura sintática do programa. 

No modelo de compilador que está sendo estudado o analisador sintático recebe do analisador léxico uma cadeia de tokens representado o programa fonte verifica se essas cadeias pertencem a linguagem definida pela gramatica. Veja um exemplo no diagrama abaixo demostrando esse processo. 

![](../images/scheme-parsing.png)

Descubra os erros sintáticos do código fonte abaixo escrito em linguagem Java.

![](../images/errors-java-source-code.png)

* Na linha 06 a falta do colchete.
* Na linha 03 o ponto e vírgula marcando o final do comando.
* Na linha 01 a virgula separando os parâmetros.

E importante destacar que a árvore de derivação representa toda a estrutura sintatica do programa.

### 4. Exercícios 

a) Defina quais são os termos utilizados em linguagens de programação e os seus significados?
b) Qual é o significado de sintaxe?
c) Qual a função da análise sintática?
e) Explique como o analisador léxico e analisador sintático trabalham juntos? 
