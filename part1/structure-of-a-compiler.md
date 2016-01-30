Estrutura de um Compilador
======

Os compiladores operem em uma sequencia de fases, cada uma transformando o programa fonte em uma representação para a etapa seguinte. Nesse capítulo vamos ver de uma forma resumida cada etapa do processo de compilação.

### Analise Léxica

É a primeira fase do processo de compilação, também é conhecida como leitura ou *scanning*. O objetivo nessa fase é identificar unidades léxicas ou lexemas que compõem o programa. O analisador léxico lê todos os caracteres do programa fonte e verifica se eles pertencem ao alfabeto da linguagem. Caso um caractere não pertença ao alfabeto da linguagem deve ser gerado um erro léxico.

Os comentários e espaços em branco devem ser ignorados e removidos. Esse processo resulta um conjunto de tokens que são formados por palavras reservadas, identificadores, delimitadores, etc. Nessa etapa é criada a tabela de símbolos. 

De uma forma resumida a análise léxica deve quebrar o texto do programa fonte em lexemas, verificar a categoria ao qual eles pertencem e produzir uma sequencia de símbolos léxicos chamados como tokens. 

Suponha que tenhamos a seguinte linha de código:

`123 x1 ; Y2 true begin`

O analisador léxico deve identificar 6 lexemas e a categoria de cada um deles.

| Lexema | Categoria                          |
|--------|------------------------------------|
| 123    | Número inteiro                     |
| X1     | Nome de variável                   |
| ;      | Símbolo especial (ponto e vírgula) |
| Y2     | Nome de procedimento               |
| true   | Constante booleana                 |
| begin  | Palavra reservada                  |


O analisador léxico deve permitir identificar na linguagem repetições de subconjuntos permitindo que seja possível identificar e classificar esses subconjuntos, por exemplo o subconjunto de palavras reservadas. 

Nessa fase o processamento de uma linguagem pode ser feito por gramáticas regulares podendo ser formalmente descrito por expressões regulares. As rotinas que processam essa linguagem modelam algoritmos construídos a partir de autômatos finitos.

### Analise Sintática

A analise sintática tem como objeto validar a gramática do programa, nessa etapa o objetivo é reconhecer se a estrutura gramatical do código fonte esta de acordo com as regras sintáticas da linguagem. 

Nessa etapa é feita uma varredura na sequência de tokens recebidas do analisador léxico e produzida uma estrutura de dados em formato de árvore conhecida como árvore sintática. A árvore sintática representa a hierarquia do programa fonte. Caso uma construção seja reconhecida com inválida um erro sintético deve ser gerado.

Veja o seguinte trecho de código:

`if (a – 10 > b * 2) a = b;`

O analisador sintático deve ser capaz de analisar essa linha e de reconhecê-la como válida ou inválida. Para isso ele precisa conhecer a estrutura formada palavra reservada `if`, a expressão que esta entre `(` e `)` e o comando de atribuição `a = b`. Após essa validação deve ser montada a árvore sintática.

A seguinte linha de código:

`position = initial + rate * 60`

Produz esse árvore sintática.

![](../images/syntax-tree.png)

Mesmo com uma boa técnica de detecção de erros o analisador sintático deve ser capaz de recuperá-los e continuar o processo de compilação identificando o maior número possível.

A sintaxe da maioria das linguagens de programação é especificada usando gramáticas livres de contexto que permitem realizar substituições impostas por regras produção e assim validar a estrutura do programa.

### Analise Semântica  

O objetivo dessa etapa é verificar se a semântica do programa fonte tem consistência. Para isso é utiliza a árvore sintática e as informações contidas na tabela de símbolos. 

Por exemplo, a verificação de tipo em uma operação de soma onde cada operando é verificado com cada operador. 

Veja:

```
var a, b: int;
a = b + '2';
```

Outro exemplo é verificar se um índice em um vetor é um número inteiro. 

Pode ser necessário que nessa fase alguns tipos de dados sejam convertidos para outros tipos, essa operação é conhecida como coerção. 

Veja o seguinte exemplo:

`position = initial + rate * 60`

Suponha que a variável `position` seja declarada como inteiro, nesse caso deverá deve ser identificado um erro de tipo interrompendo o processo de compilação. Caso `position`, `initial` e `rate` sejam declarados como ponto flutuante o inteiro `60` deve ser convertido para ponto flutuante a fim de selecionar corretamente os registradores do processador.

Algum tipos de erros semânticos:

* Tipos de operandos incompatíveis com operadores;
* Identificadores,variáveis e procedimento, não declarados;
* Chamada de funções ou métodos com número incorreto de operadores;
* Comandos fora de contextos, um comando `continue` fora de um laço;
* Operações de conversão de tipos. 

A tabela de símbolos é muito importante nessa etapa pois é através dela que é possível recuperar informações sobre os identificadores que são utilizadas para avaliar as regras semânticas. 

### Geração de código intermediário

Nesse fase é gerado uma sequência de código denominada código intermediário, que posteriormente em outras fases irá gerar o código objeto. Por ventura essa fase pode não existir e a compilação pode ser feita diretamente para o código objeto, isso é comum em compiladores auto residentes.

E importante destacar que o código intermediário não especifica detalhes da construção do código objeto final.

A representação intermediaria deve ser facilmente produzida e facilmente traduzida para a máquina alvo. Uma forma de representação intermediaria muito conhecida e a forma de três endereços que consistem de três operandos por instrução. 

Veja o exemplo:

```
T1 = inttofloat(60);
T2 = id3 * t1
T3 = id2 + t2
id1 = t3
```

Essa representação segue o seguinte formato `x = y op z` onde `op` é uma operador binário, `y` e `z` são endereços para os operandos e `x` é o endereço para o resultado.

Uma das vantagens de gerar um código intermediário é a possibilidade de obter um código final mais eficiente através da otimização. A representação intermediária deve representar todas as operações do programa.

### Otimização de Código

Nessa fase o objetivo é otimizar o código em termos de velocidade de execução e consumo de memória. Essa etapa não depende da arquitetura de máquina e tem como objetivo fazer transformações no código intermediário afim obter um código objeto mais otimizado. 

Veja um exemplo de código otimizado.

```
T1 = id3 * 60.0
id1 = id2 * t1
```

O número de otimizações irá depender de como o compilador foi implementado, quanto mais otimizações forem realizadas maior o tempo gasto na compilação. Outro detalhe é que a otimização deve reduzir o tempo de execução do programa. 

Algumas compiladores permitem que seja parametrizado o nível de otimização, como por exemplo o `gcc` através das flags de otimização `-O2`, `-O3` por exemplo.

> Nos compiladores modernos existe uma etapa chamada de otimização de código dependente de máquina, que permite que as instruções sejam otimizadas para determinada arquitetura de computador.

### Geração de código objeto

A geração de código objeto é a última etapa do processo de compilação e recebe como entrada uma representação intermediaria que mapeia a linguagem objeto. 

Desse momento deve ser feito a seleção de registradores e reserva de memória para contantes e variáveis. Essa é uma etapa muito importante pois a produção de código objeto eficiente deve ter uma cuidadosa seleção de registradores.

No seguinte trecho de código:

`position = initial + rate * 60`

Temos como código objeto:

```
LDF     R2,     id3
MULF    R2,     R2 #60.0
LDF     R1,     id2
ADDF    R1,     R1, R2      
STF     id1     R1
```

O programa objeto reflete as instruções de baixo nível da plataforma que está sendo utilizada, pode-se fazer necessário um gerador de código para cada plataforma.

Após essa fase concluída, para que o programa possa ser executado o código objeto é linkeditado com outros programas objetos ou recursos para posteriormente ser carreado na memória e executado pelo sistema operacional.

### Resultado do processo de compilação.

A imagem abaixo exemplifica o processo de compilação divido em etapas. Essas etapas correspondem a estrutura lógica de um compilador e o resultado de cada fase.

![](../images/detail-process-of-the-compilation.png)

Essa é uma estrutura didática do funcionamento de um compilador, na pratica a distinções em fases não é muito clara, alguns módulos podem ser implementados dentro de outros ou até mesmo nem existirem.

### Tabela de símbolos 

Durante toda a fase de compilação algumas informações são armazenadas em uma tabela chamada de tabela de símbolos, que nada mais é do que uma estrutura de dodos em forma de lista ou dicionário. As informações coletadas nessa tabela dependem do tipo de tradutor desenvolvido e do programa objeto a ser gerado. 

O analisador léxico coleta informações sobre os tokens e seus atributos, para tokens que influenciam decisões de análise gramatical, como por exemplo identificadores, é criado uma entrada na tabela de símbolos, das quais as informações são mantidas para posterior uso.

Os seguintes atributos costumam ser gravados na tabela de símbolos:

* Classe dos identificadores: variável, função, etc.
* Tipo de dado: Inteiro, String, etc.
* Tipos de retornos: no caso de métodos
* Variáveis: tipo, endereço no texto, posição e tamanho.
* Parâmetros formais: tipo do mecanismo de passagem, por valor ou referência.
* Procedimentos/sub-rotinas: número de parâmetros.

A tabela de símbolos é utilizado durante todo o processo de compilação a fim de inserir e extrair informações de forma rápida e eficiente.

> Dependendo do linguagem de programacao outras informacoes podem ser gravadas da tabela de simbolos.

Podemos armazenar na tabela de símbolos também informações sobre a linha e coluna que o token foi examinado para em caso de erro o compilador passa informar a posição da falha.


### Árvore Sintática

A árvore sintática é uma estrutura de dados em forma de árvore ou grafo que representa sequencia hierarquica da linguagem de programação. Essa esturura permite represetnar cada elemento do programa, os demais passos do compilador consitem em visatar os nos dessa estrutura em uma determinada ordem

Veja uma exemplo de uma sentença `if` `else`

![](../images/syntax-tree-if-else-ex.gif)
![](../images/syntax-tree-if-else.gif)

Essa representação gráfica é resultante da derivação de uma sentença, cada nó representa uma unidade sintática, formada por um simbolo terminal ou simbolo nao terminal.

### Termos

[ˆ1] Sintático: tem o sentido de verificação de forma, estrutura sem referência a significado.

[ˆ2] Semântica: tem o sentido de significado.

[ˆ3] Linkeditor ou Linker: é um programa que reúne módulos compilados e arquivos para criar o programa executável.

[ˆ4] Registradores: são áreas no processador que possuem uma grande velocidade e são utilizados para armazenar valores. Existem vários tipos de registradores com finalidade especificas.
