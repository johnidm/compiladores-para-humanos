ou backus naur form (BNF)

é usada para especificar a gramatica de uma lingaugem 

exemplo de gramatica

if (expressao) statement else statemente

em uma gramatica liver de contexto essa regra e expressa como

smtp -> if (expr) smtp else smtp

aqui nos temoso uma producao e simbolos termais e nao terminais

### Gramatica Livre de Contexto - GLC

A Gramatica Livre de Contexto ajuda a especificar a sintaxe de uma linguagem de programação. A GLC é a base para a análise sintática das linguagens de programação e permitem descrever a maioria das linguagens de programação usadas atualmente. Uma gramatica descreve naturalmente como é possível fazer construções em linguagem de programação. Veja o exemplo de um comando if-else em Pascal que deve ter a seguinte forma.

`if (expressão) then declaração else declaracao ;`

Essa mesma forma em uma Gramatica Livre de Contexto pode ser expressada da seguinte maneira 

`declaracao → if ( expressao ) then declaracao else declaracao ;`

As linguagens regulares podem ser reconhecidas através de expressões regulares criando um analisador léxico (exemplo JFlex). uma linguagem livre de contexto pode ser reconhecida autômatos de pilha que a descrevem a forma como podemos criar analisadores sintáticos (exemplo JCUP).

A definição de uma gramatica livre de contexto pode ser representada da seguinte forma:

`G = (N, T, P, S)`

Onde: 

* N – Conjunto finito de símbolos não terminais.
* T – Conjunto finito de símbolos terminais.
* P – Conjunto de regras de produções.
* S – Símbolo inicial da gramatica


Terminologias:

* Símbolos terminais: símbolos básicos que formas as cadeias, são os tokens da linguagem de programação.

* Símbolos não terminais: variáveis sintáticas utilizadas para auxiliar a definição da linguagem, são compostas de símbolos terminas e pelos próprios símbolos não terminais.

* Regras de produções: regras sintáticas que indicam como símbolos terminais e não terminais podem ser combinados.

* Símbolo inicial: Inicio da validação da produção representado por um símbolo não terminal. 

Derivações: É a substituição de cadeias de símbolos terminais iniciando pelo símbolo inicial substituindo os símbolos não terminais pelos símbolos terminais.
Tipos de derivação:
Mais à esquerda: trocamos os símbolos não terminais mais à esquerda.
Mais à direita: trocamos os símbolos não terminais mais a direita.
Exemplos de definição de uma gramatica para validar uma linguagem de programação:

### Exemplos de gramaticas livres de contexto

#### Exemplo 01 – Linguagem ab

Definir a gramatica: 

```
G = ({LITERAL}, {a, b}, PALAVRA, LITERAL)

PALAVRA { 
	LITERAL → aLITERALb | Ø 
}
```

Identificação terminologias

|                         |         |
|-------------------------|---------|
| Símbolos terminais      | a,e b   |
| Símbolos não terminais: | LITERAL |
| Símbolo inicial:        | LITERAL |
| Regra de produção:      | PALAVRA |

A palavra aabb pode ser gerada a partir da seguinte derivação a direita:

```
LITERAL → aLITERALb 
LITERAL → aaLITERALbb 
LITERAL → aaØbb
```

Com a gramática acima é possível dizer que palavra aab da linguagem? 

#### Exemplo 02 – expressões matemáticas, soma e multiplicação

Definir da gramática: 

```
G = ({EXP}, {+, *, (, ), x}, OPR,  EXP)	

OPR { 
	EXP → EXP + EXP | EXP * EXP | (EXP) | x
}
```

Identificação terminologias

|                         |                |
|-------------------------|----------------|
| Símbolos terminais      | +,, *, (, ), x |
| Símbolos não terminais: | EXP            |
| Símbolo inicial:        | EXP            |
| Regra de produção:      | PALAVRA        |

A expressão (x + x) * x pode ser derivada a partir da seguinte derivação a esquerda:
```
EXP → EXP * EXP 
EXP → (EXP) * EXP 
EXP → (EXP + EXP) * EXP 
EXP → (x + EXP) * EXP 
EXP → (x + x) * EXP 
EXP → (x + x) * x
```

É possível derivar a expressão x - x?

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

|                         |                                  |
|-------------------------|----------------------------------|
| Símbolos terminais      | + , *, (, ), x, +, -, id, numero |
| Símbolos não terminais: | EXP, OP                          |
| Símbolo inicial:        | EXP                              |
| Regra de produção:      | OPR                              |


Derivação de expressões (Podemos validar qualquer expressão matemática).

Derivação a esquerda da expressão a + b

```
EXP → EXP OP EXP
EXP → EXP OP id
EXP → EXP + id
EXP → id + id

Derivação a direita da expressão -1
```
	EXP → OP EXP
	EXP → OP numero
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

|                         |                                        |
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

#### 3. Dicas para criar uma gramatica livre de contexto
a) Conhecer todos os tokens.
b) Criar a regra de produção.
c) Especificar a gramatica. 
Ex: G =  ( {A, B, C}, {int, id, numero, +, -}, P, A )
d) Fazer a derivação


f) O que é uma regra gramatical?
g) O que é gramatica livre de contexto?
b) Qual é a forma utilizada para representar uma gramatica livre de contexto.
c) O que é símbolo não terminal, símbolo terminal, regras de produções e símbolo inicial?
d) Explique o que é uma derivação?
e) Faça a derivação a esquerda das seguintes expressões matemáticas utilizando as gramatica definida no item 3.3.

a + 1

(a + b) – 8

c) Crie uma gramática, as regras de produção e a derivação a direta das seguintes instruções.
a. Sistema binário - 0 e 1.

b. Instrução while true ;
