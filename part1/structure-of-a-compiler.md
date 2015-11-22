Estrutura de um Compilador
======

Os compiladores operem em uma sequencia de fases, cada uma transformando o programa fonte em uma representação para a etapa seguinte. Nesse capítulo vamos ver de uma forma resumida cada etapa do processo de compilação.

### Analise Léxica

É a primeira fase do processo de compilação, também é conhecida como leitura ou *scanning*. O objetivo nessa fase é identificar unidades léxicas ou lexemas que compõe o programa. O compilador lê todos os caracteres do programa fonte e verifica se eles pertencem ao alfabeto da linguagem, caso não pertença deve ser gerado um erro.

Os comentários e espaços em branco são ignorados. Esse processo resulta um conjunto de tokens que são formados por palavras reservadas, identificadores, delimitadores, etc.  Nesse momento é iniciado a construção da tabela de símbolos. De uma forma resumida a análise léxica deve quebrar o programa em lexiemas e verificar a categoria ao qual eles pertencem gerando assim o token. 

Suponha que tenhamos a seguinte linha de código:

`123 x1 ; Y2 true begin`

O analisador léxico deve identificar 6 símbolos e a categorias cada um deles.

| Lexema | Categoria                          |
|--------|------------------------------------|
| 123    | Constante inteira                  |
| X1     | Nome de variável ou procedimento   |
| ;      | Símbolo especial (ponto e vírgula) |
| Y2     | Nome de variável ou procedimento   |
| true   | Constante booleana                 |
| begin  | Palavra reservada                  |

Um token deve possuir o seguinte formato:

`<nome-token, valor-atributo>`

Onde o `nome-token` é um valor abstrato, normalmente o próprio lexema ou uma *flag* e o `valor-atributo` aponta para a tabela de símbolos .

Suponha que tenhamos a seguinte linha de código.

`position = initial + rate * 60`

Mapeamento de tokens.

| Lexema   | Símbolo | Significado   | Tabela de simbolos | Token         |
|----------|---------|---------------|--------------------|---------------|
| position | Idt     | Identificador | 1                  | <Idt, 1>      |
| =        |         |               |                    | <=, simbolo>  |
| initial  | Idt     | Identificador | 2                  | <Idt, 2>      |
| +        |         |               |                    | <+, operador> |
| rate     | Idt     | Identificador | 3                  | <Idt, 3>      |
| *        |         |               |                    | <*, operador> |
| 60       | Numero  | Inteiro       | 4                  | <Número, 4>   |


Sequência de tokens gerado. 

`<ID, 1> <=> <ID, 2> <+> <ID, 3> <*> <ID, 4>`

### Analise Sintática

Analise sintática é o coração do compilador e tem como objeto validar a gramática do programa, na análise léxica o objetivo era identificar os símbolos que pertenciam ao programa e verificar se eles pertencem a linguagem. 

A estrutura gramatical é verificada seguindo as regras da linguagem. É feito uma varredura na sequência de tokens produzidas pelo analisador léxico com o objetivo de construir a árvore sintática, ou árvore de derivação que exibe a estrutura sintática do código. Caso uma construção seja reconhecida com invalida um erro sintético deve ser gerado.

Veja o seguinte trecho de código:

`if (a – 10 > b * 2) a = b;`

O analisador sintático deve ser capaz de analisar essa linha e de reconhecê-la como valida ou invalida, para isso ele precisa conhecer a palavra reservada `if` a expressão `(` e `)` e o comando de atribuição `a = b`. Após essa validação deve ser montada a árvore sintática dessa linha de código.

A seguinte linha de código:

`position = initial + rate * 60

Produz esse árvore sintática.

![](images/sintax-tree.png)

Mesmo com uma boa técnica de detecção de erros o analisador sintático deve recuperá-los e continuar o processo de compilação identificando o maior número de erros possíveis.

### Analise Semântica  

O objetivo dessa etapa é verificar se a semântica do programa fonte tem consistência.  Para isso é utiliza a árvore sintática e as informações da tabela de símbolos. Outra atribuição importante dessa etapa e a verificação de tipo onde cada operando e verificado com cada operação. 

Por exemplo: verificar se um índice em um vetor é um número inteiro. Pode ser necessário nessa fase que alguns tipos de dados sejam convertidos para outros tipos, nesse caso ira ocorre a coerção. 

Veja o seguinte exemplo:

`position = initial + rate * 60`

Suponha que a variável `position` seja declarada como inteiro, nesse caso deverá deve ser identificado um erro de tipo interrompendo o processo de compilação. Caso `position`, `initial` e `rate` sejam declarados como ponto flutuante o inteiro `60` deve ser convertido para ponto flutuante. 

Algum tipos de erros semânticos:

* Tipos de operandos incompatíveis com operadores.
* Identificadores não declarados (variáveis e procedimento).
* Chamada de funções ou métodos com número incorreto de operadores
* Comando fora de contextos (um comando continue fora de um loop).
* Operações de conversão. 

A tabela de símbolos é muito importante nessa etapa pois é através dela que é possível recuperar informações sobre os identificadores.

### Geração de código intermediário

Nesse fase é gerado uma sequência de código denominada código intermediário, que posteriormente em outras fases irá gerar o código objeto. Por ventura essa fase pode não existir e a compilação pode ser feita diretamente para o código objeto. E importante destacar que que o código intermediário não especifica detalhes da construção do código objeto final.

A representação intermediaria deve ser facilmente produzida e facilmente traduzida para a máquina alvo.  Uma forma intermediaria muito conhecida e a forma de três endereços que consistem de três operandos por instrução.  Veja o exemplo:

```
T1 = inttofloat(60);
T2 = id3 * t1
T3 = id2 + t2
id1 = t3
```

Essa representação segue o seguinte formato `x = y op z` onde `op` é uma operador binário `y` e `z` são endereços para os operandos e `x` é o endereço para o resultado.

Uma das vantagens de gerar um código intermediário é a possibilidade de obter um código final mais eficiente.

### Otimização de Código

Nessa fase o objetivo é otimizar o código em termos de velocidade de execução e espaço na memória. Essa etapa não depende da arquitetura de máquina e tem como objetivo fazer transformações no código intermediário afim de um código objeto melhor. 

Veja um exemplo de código otimizado.

```
T1 = id3 * 60.0
id1 = id2 * t1
```

O número de otimizações irá depender de como o compilador foi implementado, quanto mais otimizações forem realizadas maior o tempo gasto na compilação, ou detalhes é que a otimização deve reduzir o tempo de execução do programa. Alguns compiladores ainda possuem uma etapa chamada de otimização de código dependente de máquina.

### Geração de código objeto

O gerador de código objeto recebe como entrada uma representação intermediaria e mapeia para uma linguagem objeto, caso a linguagem objeto for a linguagem de máquina de alguma arquitetura especifica deve ser feito a seleção de registradores reserva de memória para as constantes e variáveis. É uma etapa muito importante pois a produção de código objeto é eficiente e deve ter uma cuidadosa seleção de registradores.

No seguinte trecho de código:

`position = initial + rate * 60`

Uma possível geração de código objeto seria:

```
LDF		R2, 	id3
MULF	R2, 	R2 #60.0
LDF		R1, 	id2
ADDF	R1, 	R1, R2		
STF		id1	R1
```

O programa objeto reflete as instruções de baixo nível da plataforma que está sendo utilizada, normalmente é necessário um gerador de código para cada plataforma.

Após essa fase concluída o programa objeto é linkeditado com outros programas objetos ou recursos para ser posteriormente carreado na memória e executado pelo sistema operacional.

### Resultado do processo de compilação.

A imagem abaixo exemplada o processo de compilação divido em etapas. Essas etapas correspondem a estrutura lógica de um compilador e o resultado de cada fase.

![](images/detail-process-of-the-compilation.png)

### Tabela de símbolos 

Durante toda a fase de compilação algumas informações são armazenadas em uma tabela chamada de tabela de símbolos. As informações coletadas nessa tabela dependem do tipo de tradutor desenvolvido e do programa objeto a ser gerado. 

Os seguintes atributos costumam ser utilizados:

* Variáveis: classe(var), tipo, endereço no texto, posição e tamanho.
* Parâmetros formais: classe(par), tipo mecanismo de passagem.
* Procedimentos/sub-rotinas: classe(proc), número de parâmetros.

A tabela de símbolos é estrutura de dados utilizado durante todo o processo de compilação a fim de inserir e extrair informações de forma rápida. A tabela de símbolos é utilizada em todas as fases do processo de compilação.

### Árvore Sintática

A arvore sintática é uma estrutura de dados em forma de arvore que representa a gramatica, ou estrutura sintática da linguagem de programação.


[ˆ1] Sintático: tem o sentido de verificação de forma, estrutura sem referência a significado.

[ˆ2] Semântica: tem sentido de significado.

[ˆ3] Linkeditor ou Linker: é um programa que reúne módulos compilados e arquivos para criar o programa executável.

[ˆ4] Registradores: são áreas no processador que possuem uma grande velocidade e são utilizados para armazenar valores. Existem vários tipos de registradores com finalidade especificas.

