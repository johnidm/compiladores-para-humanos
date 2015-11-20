Introdução e visão geral da estrutura de um compilador
======

O estudo e escrita de compiladores abrange diversrs sidciplina de cientea da computacao como conceitos de linguagens de programação, arquitetura de máquina, algoritmos e engenharia de software. Nesse e-book será aprestado uma introdução ao processo de compilação mostrando uma visão de alto nível da estrutura de um compilador. As linguagens de programação e a arquitetura de computadores evoluem e estão cada vez mais sofisticados, o desafio dos projetistas de compiladores é criar algoritmos mais eficientes que visam obter um melhor desempenho no uso de memória e processamento.

Nessa primeira parte do ebook, vamos apresentar as etapas envolvidas no processo de compilação e como elas se relacionam com uma linguagem de programação. O estudo de compiladores é essencial para entender a ligação entre engenharia de software linguagem de programação, sistemas operacionais e arquitetura de computadores.

### O Compilador

O compialdor é um software complexo que converte uma liguagem fonte em uma lingaigem destino, ou seja, comverte um programa originado de uma lingaugem de prpgoacao para uma lingagem que possa eser entendida e executada por um computador, esse eapa deve preservar a significdo do fonte original. Na compiação é excutado um de tarefas que gerar a descricao de uma lingaugem em outra

Os processo de compilação sao complexxos e exigiam um esforo muia significativo, os compiladores i=tinha que ser escritos em binario e salvo na memoria ROM, hoje nos temos um conjunto de ferramentas que faciliam a criacao e manutencoa de compialdores, muitas delas escritas em linguagem como Java, C e C++ já automatizamo boa parte da escrica de compiladores.

Essas ferrametna geram códigos que podem ser incluidos no nosso compilador. Um exme´lo sao os geradores de analizadore lexicos que com base em comjunto de expressoes reguladores costreo elemento lexicos que sao passados para a proxima etapa de compilacao 

Quando nos geramano um compilaador nosjá temos uma lingaugem de programcao que pode ser utilziada e manutenida, e nos geramos essa nova lingagem com base em aguma já existe. è importante rsaltar que a nossa nova lingaugem já esta suficiente completa é com ela nos podemos gerar no seu proprio compilador, isso nos obirgao a desenvovler um novo compilador utilizando agora a nova lingaugem criado e assim nos gerar um ciclo completo da nossa nova linguagem.

Um exmeplo muito comum de um processo de compiação é o uso de PostScript, que é uma linguagem de programação especializada para visualização de informações, nos podemos ter um programa de composião tipografica, que produz o PostScript, ele tem como entrado um documento é ira produzir com saida um aquivo PostScript que vai descrevr uma imagem, isso é cosiderado um processo de compiacao. O código que transfora o PostScript em pixel é cahammdo de interpreador.

### Linguagens de programação

Podemos definir uma linguagem comumente chamada de língua ou idioma como um meio de comunicação entre pessoas. Em programação definimos a linguagem como o meio de comunicação entre o pensamento humano e as ações de um computador (PRICE, 2001). De acordo com AHO(2008) todo o software executado em todos os computadores foi escrito em uma linguagem de programação e antes que ele possa rodar um programa ele deve ser traduzido para um formato que possa ser executado pelo computador. 

A lingaugem de programcao é projetada para permitir que sere umanos expressem processos computacionaos em uma sequencia de operacoes, por exemplo ler um arquivo e imprimir o seu conteúdo.


![]()

O programa fonte é conhecido como Java, C, Python, etc e a lignagem alvo é geramletne um  conjunto de intruções de um processadores.







Os programas executados no computador são sequencias de zeros e uns que representam valores inteiros, ponto flutuante, strings, etc., ou ainda instrução que indicam o que o computador deve fazer. A memória do computador é dividida em partes que possuem endereços únicos, todo o programa armazenado na memória é executado pela CPU que possui registradores que guardam dados temporários utilizados em operações. Pense na seguinte situação: para somar o número armazenado no endereço de memória `$000011` com o número armazenado no endereço de memória `$000012` é necessário: 

* copiar o conteúdo da memória `$000011` para um registrador **A**; 
* copiar o conteúdo da memória `$000012` para um registrado **B**; 
* somar o conteúdo de **A** e **B** e copiar o resultado para o endereço de memória `$000013`. 

Esse processo certamente é muito mais complexo do que simplesmente executar o comando `c = a + a` em uma determinada linguagem de programação (DELAMARO, 2004).


Uma linguagem de programação é considera de alto nível quando sua representação está próxima do domínio da aplicação e do problema a ser resolvido. Os computadores por sua vez possuem sua própria linguagem denominada de baixo nível e possuem uma gramática composta por zeros e uns. 

O processo de tradução de linguagem de alto nível para linguagem de baixo nível é feito através de softwares conhecidos como compiladores e tem como entrada uma linguagem fonte (alto nível) e como saída uma linguagem objeto (baixo nível) (PRICE, 2001).

![](../images/compilation-process.png)

A imagem acima demonstrada por DELAMARO(2004) podemos ver um diagrama que representa o processo de compilação onde a entrada é um programa fonte e a saída é um programa objeto.

De acordo com PRICE (2001) as linguagens de programação podem ser classificadas em 5 gerações:

* 1ª linguagens de máquina – baixo nível 
* 2ª linguagens simbólicas ou montagem (Assembly) – baixo nível
* 3ª linguagens orientadas a usuário – alto nível
* 4ª linguagens orientadas a aplicação – alto nível 
* 5ª linguagens de conhecimento – alto nível

Nós primórdios da computação os computadores eram programados em linguagem de máquina utilizando notação binaria, dessa forma os algoritmos eram complexos e de difícil implementação. A 2ª geração denominada de linguagens simbólicas ou de montagem foi utilizada para minimizar a complexidade da 1ª geração é utilizavam mnêmicos que substituíam as instruções binárias. As instruções eram convertidas em código de máquina antes de serem executados, esse processo é conhecido como montagem (PRICE, 2001).

As linguagens de 3ª geração são voltadas para a solução de problemas específicos, por exemplo o uso em aplicações comerciais e cientificas, nesse momento surgem linguagens como COBOL, Pascal, Ada, FORTRAN, etc. Essas linguagens são classificas como procedimentais, declarativas, imperativas, logicas, etc. e os programas descrevem como os problemas serão resolvidos através de instruções denominados:

* Entrada/saída.
* Cálculos aritméticos ou lógicos.
* Controle de fluxo.

As linguagens de 3ª geração foram projetadas para serem utilizadas por profissionais específicos conhecidos como engenheiros de software ou simplesmente programadores. Já as linguagens de 4ª geração formam projetadas para serem utilizadas por usuários finais sendo de fácil programação permitindo que os próprios usuários possam resolver os seus problemas, exemplos são: Excel; Access; SQL, etc. As linguagens de 5ª são utilizadas em programas de Inteligência Artificial, simulando comportamentos inteligentes, como exemplo temos o PROLOG (PRICE, 2001).

### Tradutores 

Os tradutores são sistemas que aceitam como entrada um programa escrito em uma linguagem e produzem como resultado um programa equivalente na mesma lingaugem ou em outra linguagem. De acordo com PRICE (2001) os tradutores podem ser classificados em:

* Montadores: também chamados de assemblers, eles mapeiam instruções em linguagem simbólica para instruções em linguagem de máquina. Faz a tradução de valores resultantes para valres binarios. DIsasemlber ou desmontadores fazer o processo inverso.
* Macro-assemblers: funcionam da mesma forma que os montadores, porém podem ser criados “macros” que são uma sequência de comandos simbólicos.
* Compiladores: mapeiam programas escritos em linguagem de alto nível para linguagem simbólica ou de máquina. 
* Pré-compiladores: também chamados de pré-processadores ou filtros são programas que convertem uma linguagem de alto nível estendida de uma linguagem de programação original, para outra linguagem de alto nível.
* Interpretadores: Possuem como entrada uma linguagem intermediária ou a própria linguagem fonte e um programa compilado produz o efeito de execução.

#### Compiladores x Interpret

UM traduor pode ser entido como um processo que em vez de visar um conjunto de instruções de um processaor visar outra luingaugem 

Diferente do compilador o interperatdo rece como estar uma espeficicao eecutavel e produx com saida a execucao dessa especificacoa, ligaggem com PHP, Scheme, Python ~sao interpretadas, os compialdores e inte´retadores possuiem muit oem comum, pois executam as mesmas tarefas, como por exemplo analsia rum programa de entrada e determiar se ele é valido ou nao.

Um caso muito interessante é o da linaugem Java qe combina os dposi modelos, nos temo um processo de cimpialdcao que pode uma um formato de codigo cahamdo de bytecode, e um proe=cesso de interpertacao feitp pla JVM, outro detalhe impreotnta é que a JVM possui um processo chamado de JIT, que permite que algumas intruções sejam compialdora para para codio de mauiqna a fim de orimizar essa isntruções.


### Estrutura de um compilador

O processo de compilação ou interpretação é muito complexo, existe uma estrutura básica que divide esse processo em fases, essas fases estão representadas por tarefas divididas em análise e síntese (PRICE, 2001).

Todo o processo de compilação é dividido em fases e tem como objetivo dar uma visão explicita e detalhada do processo de compilação. A parte de análise também chamada de *front-end* divide o programa fonte em partes e impõe uma estrutura gramatical sobre elas, uma das principais responsabilidades da parte de análise é garantir que a sintaxe e semântica do programa fonte estejam corretos. A parte da síntese constrói o programa objeto a partir da representação criada na parte de análise. A síntese e conhecida também é back-end (AHO, 2008).

![](../images/compilation-steps.png)

Caso o compilador tenha sido cuidadosamente projeto podemos produzir compiladores de diferentes linguagens fonte para diferentes máquinas alvo, combinando diferentes estrutura de *front-end* para um únido *back-end* e um único *front-end* para diferentes *back-ends*. 
De acordo com TUCKER(2008) processo de compilação inicia com a analisador léxico que varre todo o programa fonte e o transforma em um fluxo de tokens, logo em seguida vem a análise sintática que lê o fluxo de tokens e e valida a estrutura do programa criando a árvore sintática e a tabela de simbolos, a terceira fase e a análise semântica responsável por garantir as regras semânticas. 

A próxima fase e a geração de código intermediário que cria uma abstração do código, logo após vem a fase de otimização do código e pôr fim a geração do código objeto que tem como objetivo gerar o código de baixo nível baseado na arquitetura da máquina alvo.

Em contraste temos os interpretadores que de acordo com TUCKER(2008) é uma forma de tradutor no qual as duas últimas fases do compilador são substituídas por um programa que executa o código intermediário.

O interpretador pode ser divido em dois tipos:

* Interpretador puro: Cada instrução e “quebrada “em tokens, analisada, verificada semanticamente e interpretada a cada vez que é executada. Como exemplo temos integradores de comandos shell tradutores Basic.
* Interpretadores mistos: Traduzem todo o script em código intermediário e depois interpretam esse código.


A etapa de analise ou *front-end* é fetuada atreaves de algoritmos de complexidade lonear e aboradmos nessa etapa automatico finitmos, ligaugens regulares e automatos de pilha, nosp odemos escreve o nosso proprios programa que executam essas etapa ou utilziar geradores de analizadores lexicos, como o flex, jFlex, Jlex,  e sintaticos , byacc, bison, JavaCup. 

[^1] Desmontador também chamado de Desassemblador faz o processo inverso ao montador, ou seja, pega o código de máquina e transforma em código Assembly. 

[^2] Mnemônicos são símbolos que substituem o padrão de bits. Como exemplo o ADD equivale a soma e em bits significa 00100011.

[^3] Tempo de compilação é o intervalo de tempo de conversão de um programa fonte para um programa objeto. Já o programa objeto é executado no intervalo de tempo chamado tempo de execução.

[^4] Tradutores auto residentes ou self-resident-translator geram códigos de máquinas hospedeiras nas quais eles mesmo executam. 


[^5] Compiladores cruzados ou cross-compilers são compiladores que geram código objeto para outras máquinas que não são hospedeiras

[^6] Cross-compiling é o process de compilacao que permite a um compilador cimplar um programa para diversos processadores ou arquiteuras.










Exercicíos
------

1. Explique as diferenças entre um compilador e um interpretador?

2. Quais as cincos classificações das linguagens de programação?
3. Cite os 5 tipos de tradutores e o significado de cada um?
4. Quais as fazes de um processo de compilação?
5. Cite as fazes que pertencem a parte de análise ou front-end do processo de compilação?
6. Cite as fazes que pertence a parte de análise ou back-end do processo de compilação?
7. O que é um integrador puro e um interpretador misto?
8 .O que é a tabela de símbolos e quais os principais atributos que são utilizados?
9. O que é o modelo de compilação Just-In-Time?
