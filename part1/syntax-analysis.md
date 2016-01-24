Analise Sintática
======

O Analisador sintático também conhecido como *parser* tem como tarefa principal determinar se o programa de entrada representado pelo fluxo de tokens representa sentenças validas na linguagem de programação 

A analise sintática e a segunda etapa do processo de compilação e na maioria dos casos utiliza gramáticas livres de contexto para especificar a sintaxe de uma linguagem de programação.

### Visão geral

A sintaxe é a parte da gramática que estuda a disposição das palavras na frase e das frases em um discurso. Essa etapa do processo de compilação deve reconhecer a sintaxe do programa fonte e determinar se ele é valido ou não.

Esse modelo pode ser definido utilizando gramáticas livres de contexto que representam uma gramática formal e possuem um o algoritmo que tenta derivar todas as possíveis construções da linguagem.

As derivações tem como objetivo determinar se um fluxo de palavras se encaixa na sintaxe da linguagem de programação.

Alguns termos são utilizados na definição de linguagens de programação.

* **Símbolo**: são os elementos mínimos que compõe uma linguagem. Na linguagem humana são as letras. 
* **Sentença**: É um conjunto ordenado de símbolos que forma uma cadeia ou *string*.  Na linguagem humana são as palavras.
* **Alfabeto**: É um conjunto de símbolos. Na linguagem humana é o conjunto de letras {a, b, c, d, ...} 
* **Linguagem**: É o conjunto de sentenças, Na linguagem humana são os conjuntos de palavras {compiladores, linguagem, ...}
* **Gramática**: É uma forma de representar as regras para formação de uma linguagem.

Trazendo esse conceito para linguagem de programação temos:

* Alfabeto: {w, h, i, l, e, +, 1, 2, 3}
* Símbolos: 1, 5, +, w
* Sentença: while, 123, +1
* Linguagem: {while, 123, +1}

Dada uma gramática `G` e uma sentença `s` o objetivo do analisador sintático é verificar se a sentença `s` pertence a linguagem `G`. O analisador sintático recebe do analisador léxico a sequência de tokens que constitui a sentença `s` e produz uma arvore de derivação se a sentença é válida ou emite um erro se a sentença é inválida. 

O analisador léxico é desenvolvido para reconhecer cadeias de caracteres fazendo uma leitura dos caracteres e obtendo a sequência de tokens, esse analisador vê o texto como uma sequência de palavras de uma linguagem regular e reconhece ele através de autômatos finitos. 

Já o analisador sintático vê o mesmo texto como uma sequência de sentenças que deve satisfazer as regras gramaticais. É através da gramática que podemos validar expressões criadas na linguagem de programação. 

![](../images/ast.png)

O analisador sintático agrupa os tokens em frases gramaticais usadas pelo compilador com o objetivo de criar uma saída que representa a estrutura hierarquia do programa fonte.

Veja no quadro abaixo a especificação da entrada e saída das etapas vistas até o momento:

| Fase   | Entrada               | Saída              |
|--------|-----------------------|--------------------|
| Lexer  | Conjunto de caracteres | Conjunto de tokens |
| Parser | Conjunto de tokens    | Arvore sintática   |


Observe estrutura sintática de uma linguagem de programação. Temos as divisões dos blocos, compostos por comandos, compostos por expressões.

![](../images/source-code-java.png)

Entende-se por regras gramáticas as formas como podemos descrever a estrutura sintática do programa. 

No modelo de compilador que está sendo estudado o analisador sintático recebe do analisador léxico uma cadeia de tokens representado o programa fonte verifica se essas cadeias pertencem a linguagem definida pela gramática. Veja um exemplo no diagrama abaixo demostrando esse processo. 

![](../images/scheme-parsing.png)

Descubra os erros sintáticos do código fonte abaixo escrito em linguagem Java.

![](../images/errors-java-source-code.png)

* Na linha 06 a falta do colchete.
* Na linha 03 o ponto e vírgula marcando o final do comando.
* Na linha 01 a virgula separando os parâmetros.

E importante destacar que a árvore de derivação representa toda a estrutura sintática do programa.

### Gramática Livre de Contexto - GLC

As linguagens de programação em geral pertencem a uma categoria chamada de Linguagens Livres de Contexto. Umas das formas de representar essas linguagens é através de Gramáticas Livres de Contexto que são a base para a construção de analisadores sintáticos. Elas são usadas para especificar as regras sintáticas de uma linguagem de programação, uma linguagem regular pode ser reconhecida por uma automato finito, já uma Gramatica Livre de Contexto pode ser reconhecida por um automato de pilha.

Outra aplicação de GLC são os DTD - Definição de Tipos de Documentos -  utilizados por arquivos XML que descreve as tags de uma forma natural, as tags deve estar aninhas afim de lidar com o significado do texto.

Veja o exemplo:

```
<produto><codigo</codigo></produto>
```

Uma gramática descreve naturalmente como é possível fazer construções no programa. Veja o exemplo de um comando `if-else` em Pascal que deve ter a seguinte forma.

`if (expressão) then declaração else declaração ;`

Essa mesma forma em uma Gramática Livre de Contexto pode ser expressada da seguinte maneira: 

`declaração → if ( expressão ) then declaração else declaração ;`

A definição de uma gramática livre de contexto pode ser representada através dos seguintes componentes:

`G = (N, T, P, S)`

Onde: 

* N – Conjunto finito de símbolos não terminais.
* T – Conjunto finito de símbolos terminais.
* P – Conjunto de regras de produções.
* S – Símbolo inicial da gramática.

Terminologias:

* **Símbolos terminais**: Conjunto finito de símbolos básicos que formam as palavras da linguagens, são representadas pelo tokens reconhecidos pelo analisador lexico.

* **Símbolos não terminais**: Conjunto finito de variáveis utilizadas para representar os conjuntos da linguagem, são formadas pelos terminas e pelos próprios símbolos não terminais.

* **Símbolo inicial**: É a variável, simbolo não terminal, que representa o inicio da definição da linguagem. 

* **Regras de produções**: Representa um conjunto de regras sintáticas que representam a definição da linguagem, indicam como símbolos terminais e não terminais podem ser combinados.

As regras de producao são representadas da seguinte forma:

```
    {A} → {α}
```

Onde:

* **A** é uma variável - simbolo não terminal.
* **->** simbolo de producao .
* **α** é a combinação símbolos terminais e não terminais que representam a forma como uma string vai ser formada.

Veja o exemplo de uma Gramatica Livre de Contexto.

```
G = ({S}, {a, b}, P, S)

P = {   
        S → aSb
        S → λ  
    }
```

Essa gramatica é formada pelas terminais `a` e `b`, que são os tokens da linguagem, como regras de produção nos temos `aSb` que indica a presenta de um `a` e `b` nas extremidades da palavras é o simbolo `λ` que significa vazio.



#### Derivacaoes

A derivação é a substituição do conjunto de símbolos não terminais por simbolos terminais iniciando pelo símbolo inicial, ao final desse processo o resutlado é a forma como a linguagem deve assumir.

A derivacao sempre inicia pela raiz que é o simbolo inicial, durante a derivacao aplica as regras de producao para que possamos identificar que certas cadeias de palavras podem ser represetnadas na lingaugem, as regras espandem o *body* das producoes ate substituir todas palavras em simbolos terinais. É possivel respsetanr as derivacaoes em uma estrurua de arvore conhecida como arvore de derivacao.

Tipos de derivação:

* Mais à esquerda: a derivcao inicia pela troca do símbolo não terminais mais à esquerda.
* Mais à direita: a derivcao inicia pela troca do símbolo não terminais mais à direita.

Idenpendente da direcao da derivacao, esquerda ou direita, ela deve produzir o mesmo resultado, ou seja, a mesma arovorde de derivacao, caso o resutlado seja diferente temos uma ambiguidade.

#### Árvores de derivação

É uma represetnacao em formato de arvores que representa a derivacao de uma sentenca, essa estrutra ira gera a arvoers de analise sintatica que representa o programa fonte é o resutaldo da analise sitatica, essa setrututa faciltia a a conversao desse programa em codigo em codigo executavel

E importante ressaltar que a arovre de analise sintatica esta diretamente relacionada a existencia de derivacoes, e isso resulta es as folhas serem todas simbolos terminais.

Dada as seguinte GLC

```
G = ({S}, {a, b}, P, S)

P = {   
        S → aSb
        S → Ø  
    }
```

Como resultado temos a seguinte arvore de derivacao

![](../images/part1-derivation-tree.png)

A raiz da arvore de derivacao  sempre o simbolo inicial, os vertices interiores sao os simbolos nao termiais e os seimbolos termianis e a palavra vazia sao as folhas.



#### Ambiguidade:

Certas gramaticas permitem que uma mesma sentenca tenha mas de uma arvore de derivacao, isso torna a gramtica inadequada para a lingaugem de programacao , pois o compilador nao pode determinar a estrutura desse programa fonte e portant nao pode montar o código final, portanto se a derivca mais a esquerda ou a direita produzor maus de uma arvode essa gramatica e dita ambigua.

Uma ambiguidade por ser evitada de duas formas:

1.  Reescrevendo a gramatica afim de remover a ambigudiade, isso pode tornar a graamtica mais complexa
2. Definindao ordens de prioridade na derivacao


Veja um exemplo exemplos comum de gramatica ambiguidades.

Dada a seguinte gramatica utilziada para reconhcer as principais operaoes aritemeticas. 

```
G = ({E}, {+, *, (, ), x}, P,  E) 

p { 
    E → E + E
    E → E * E
    E → (E)
    E → x
    E → Ø
}
```


Suponha quye quermos valdiar a seguinten sentença `x + x * x`.

• Derivação mais à esquerda
    ∗ E ⇒ E+E ⇒ x+E ⇒ x+E∗E ⇒ x+x∗E ⇒ x+x∗x
    ∗ E ⇒ E∗E ⇒ E+E∗E ⇒ x+E∗E ⇒ x+x∗E ⇒ x+x∗x

• Derivação mais à direita
    ∗ E ⇒ E+E ⇒ E+E∗E ⇒ E+E∗x ⇒ E+x∗x ⇒ x+x∗x
    ∗ E ⇒ E∗E ⇒ E∗x ⇒ E+E∗x ⇒ E+x∗x ⇒ x+x∗x

Observe que duas arvores sintaticas forma geradas para essa sentença, logo temos uma ambiguidades.

![](../images/part1-ambiguity-tree.png)

Reescrevendo essa gramatica para evitar a ambiguidade nos temos o sequinte resultado 

```
G = ({E}, {+, *, (, ), x}, P,  E) 

p { 
    E → T + E | T
    T → x * T
    E → x
    E → (E) * T
    E → (T)
    E → Ø
}
```

Idenpendente da direcao da derivacao nos vamos obter a setuinte arovore sintatica 

![](../images/part1-solved-ambiguity-tree.png)

Nao existem nennhum algiritmo que seja capaz de eliminar a ambiguidade, e exsiste gramatica onde a ambiguidade é impossivel se ser eliminada, nesse caso fasse necessario aplicas a tecnica de emiminacao de ambiguidade e modificar a gramatica.

### Forma de Backus-Naur (BNF)

A Forma de Backus-Naur outra maneira de representar lingaugens livres de contexto, são muito semelhantes as GLC mas possuem duas importantes diferencas apararecem quando comparamos com GLC

1. O sinal `→` é substituído por `::=`. Ex: `S → α` ≡ `S ::= α`.
2. Os simbolos não terminais devem esta entre `<` e`>`.

Veja o exemplo

![](../images/form-backus-naur.gif)

Veja uma comparacao entre as duas formas gramaticais

###### Grmatica livre de contexto

``` 
G ({S, M, N}, {x,y}, P, S)
P {
    S → x | M
    M → MN | xy
    N → y
}
```

###### Forma de backus naur

```
G = ({<S>, <M>, <N>}, {x,y}, P, <S>)
<S> ::= x | <M>
<M> ::= <M> <N> | xy
<N> ::= y
```

> Os símbolos <, >, ::= não fazem parte da linguagem



### Forma Extendida de Backus-Naur (BNF)

E um complemento da Forma de Backus-Naur que permite ao lado direto da producao seja possivel ter operadores. Isso facilita a escrita das regras gramaticais pois deixa elas mais simples. 

Podemos ter o seguintes operadores:

* Selecao:
    `(α | β)` um dos elementos entre parenteses pode ser expressos

    ```
    <S> ::= a(b | c | d)e
    ```

    Pode assumir as seguites derivacoes 
    
    `abe`
    `ace`
    `ade`

* Opcional
    `[α]` o que estiver entre colchetes pode ter utilziado ou nao 

    ```
    <S> ::= a[bcd]e
    ```

    Pode assumir as seguites derivacoes 

    `ae`
    `abcde`   


* Repeticao 0 ou mais vezes

    `(α)*` o que estiver entre parentese pode repetir um numero qualquer de vezes e pode nao ser usado

    ```
    <S> ::= a(b)*c
    ```

    Pode assumir as seguites derivacoes 

    `ac`
    `abc`   
    `abbc`   
    `abbc`
    `abbb...c`   

* Repeticao 1 ou mais vezes

    `(α)+` o que estiver entre parentese pode repetir um numero qualquer de vezes 

    ```
    <S> ::= a(b)+c
    ```

    Pode assumir as seguites derivacoes 

    `abc`   
    `abbc`   
    `abbc`
    `abbb...c`   

![](../images/extended-BNF.gif)

Outro tipo de notação usual para gramáticas é a notação de grafos sintáticos. Esta notação tem o mesmo poder de expressão de BNF, porém define uma representação visual para as regras de uma gramática livre de contexto.

![](../images/graph-syntactic.jpg)

Na notação de grafos sintáticos, símbolos terminais são representados por círculos e símbolos não-terminais, por retângulos. Setas são utilizadas para indicar a seqüência de expansão de um símbolo não-terminal.

### Exemplos de gramáticas livres de contexto

Nos seção vamos focar no estudo de GLC e entender como ela funciona, 

Dicas para criar uma gramática livre de contexto

* Conhecer todos os tokens;
* Especificar a gramática. Por exemplo `G =  ( {A, B, C}, {int, id, numero, +, -}, P, A )`;
* Criar as regra de produção;
* Fazer a derivação;

> È comum representar os o simbolos temrinais em maiusculo e o nao terminais em minusculo

#### Exemplo 01 – Linguagem ab

Definir a gramática:

```
G = ({S}, {a, b}, S, S)

PALAVRA { 
	LITERAL → aLITERALb | Ø 
}
```

Identificação terminologias

| Descrição               |         |
|-------------------------|---------|
| Símbolos terminais      | a,e b   |
| Símbolos não terminais: | LITERAL |
| Símbolo inicial:        | LITERAL |
| Regra de produção:      | PALAVRA |

A palavra `aabb pode ser gerada a partir da seguinte derivação a direita:

```
LITERAL → aLITERALb 
LITERAL → aaLITERALbb 
LITERAL → aaØbb
```

Com a gramática acima é possível dizer que palavra `aab` pertence linguagem? 

#### Exemplo 02 – expressões matemáticas de soma e multiplicação

Definir da gramática: 

```
G = ({EXP}, {+, *, (, ), x}, OPR,  EXP)	

OPR { 
	EXP → EXP + EXP | EXP * EXP | (EXP) | x
}
```

Identificação terminologias

| Descrição               |                |
|-------------------------|----------------|
| Símbolos terminais      | +,, *, (, ), x |
| Símbolos não terminais: | EXP            |
| Símbolo inicial:        | EXP            |
| Regra de produção:      | OPR            |

A expressão `(x + x) * x` pode ser derivada a partir da seguinte regra de produção:

```
EXP → EXP * EXP 
EXP → (EXP) * EXP 
EXP → (EXP + EXP) * EXP 
EXP → (x + EXP) * EXP 
EXP → (x + x) * EXP 
EXP → (x + x) * x
```

É possível derivar a expressão `x - x?

#### Exemplo 03 – expressões matemáticas completas

Definir da gramática: 

```
G = ({EXP, OP}, {+, *, +, -, (, ), id, numero}, OPR,  EXP)

OPR { 
	EXP → EXP OP EXP | EXP → (EXP) | -EXP | id | numero
	OP → + | - | * | /
}
```

Identificação terminologias

| Descrição               |                                  |
|-------------------------|----------------------------------|
| Símbolos terminais      | + , *, (, ), x, +, -, id, numero |
| Símbolos não terminais: | EXP, OP                          |
| Símbolo inicial:        | EXP                              |
| Regra de produção:      | OPR                              |


Podemos validar qualquer expressão matemática com essa gramática.

Derivação a esquerda da expressão `a + b`

```
EXP → EXP OP EXP
EXP → EXP OP id
EXP → EXP + id
EXP → id + id
```

Derivação a direita da expressão -1
```
EXP → OP EXP
EXP → - EXP
EXP → - numero
```

#### Exemplo 04 – Chamada de funções

Definir a gramática: 

```
G = ({CHAMADA, CHAMADA_PARAMS, PARAMS, PARAM}, {(, ), , , id}, OPR,  CHAMADA)	
OPR { 
	CHAMADA → id(CHAMADA_PARAMS)
	CHAMADA_PARAMS → (PARAMS | Ø)
	PARAMS → PARAMS, PARAM | PARAM
	PARAM → id
}
```

Identificação terminologias

| Descrição               |                                        |
|-------------------------|----------------------------------------|
| Símbolos terminais      | (,,),,, ,,id                           |
| Símbolos não terminais: | CHAMADA, CHAMADA_PARAMS, PARAMS, PARAM |
| Símbolo inicial:        | CHAMADA                                |
| Regra de produção:      | OPR                                    |

Derivação a esquerda de exibir(valor, desconto)
```
CHAMADA → id(CHAMADA_PARAMS)
CHAMADA → id(PARAMS, PARAM)
CHAMADA → id(PARAM, PARAM)
CHAMADA → id(PARAM, PARAM)
CHAMADA → id(id , id)
CHAMADA → exibir(valor, desconto)
```


### Geradores de analisadores sintáticos

Da mesma forma que ocorre na construção de analisadores léxicos os analisadores
sintáticos podem ser construídos através de ferramentas que auxiliam esse trabalho.

O funcionamento é semelhante aos analisadores léxicos. Um arquivo com as
especificações sintáticas é criado e através de comandos o analisador sintático é gerado.

A saída é um arquivo de código com a implementação do analisador sintático.