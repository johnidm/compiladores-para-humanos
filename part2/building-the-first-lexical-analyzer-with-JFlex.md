Contruindo o primeiro analisador léxico com JFlex
======

JFlex é um gerador de analisador léxico escrito em Java. Ele recebe como entrada uma especificação baseada em um conjunto de expressões regulares e gera um algoritmo que lê os caracteres descritos em uma linguagem de programação combinando com os padrões definidos no arquivo de especificação.

### Instalando o JFlex

Faça o download do JFlex nesse link: http://jflex.de/download.html

Nesse primeiro exemplo nós vamos usar o JFlex integrado com a IDE Eclipse.

Voce deve fazer a instalaçao da biblitoeca no seu ambiente de desenvolvimento, eu sugiro uma rápida leitura nas intruções de instalacao nesse [link](http://jflex.de/installing.html).

Você pode adicioanar a biblitoeca **jflex-x.x.x.jar** na configuracao de *Build Path* do seu projeto no Eclipse.

### Criado uma analisador léxico

Crie um novo projeto no Eclipse através do menu `File -> New -> Java Project` e adiciona a biblitoeca do JFlex - **jflex-x.x.x.jar** - nas confgiurações de *Build Path* do projeto.

Em seguida vamos criar uma classe que ira gerar o analisador léxico a cada nova alteração no arquivo de especificação. Isso evita o uso da linha de comando para gerar a classe Java responsavel por implementar o algoritmo de reconhecimento de tokens.

```
```

O arquivo especificação possui as definições da nossa linguagem de programação. Para criar esse arquivo na IDE do Eclipse vá em `File -> New -> Other -> File` save esse arquivo com o nome de `linguagem.lex`.

No arquivo digite o seguinte conteúdo.

```
```

Pronto, nós já temos um analisador léxico onde é possivel reconhcer um conjunto de lexemas baseado em padroes definidos no arquivo de especificaçao - `lingaugem.lex`, agora nos prcisamos teste nosso analizador léxico. Para isso crie uma nova classe chamada `` e inclua o seguinte código nela.


### Executando o analisador léxico

Nós vamos executar o analizador lexico com os seguinte passos:

1. Execute a classe Gerador: Observe que no seu projeto foi criado a classe `AnalisadorLexico`. Essa classe possui a implementação das regras definidas no arquivo de especificacao `lingaugem.lex` é representa o algoritmo que do analisador léxico.

2. Crie a classe `Lingaugem` - `File -> New -> Class` - que irá implementar a chamada do analisador léxico.

A classe deve implementar o seguinte código

Execute essa classe o observe que a saída será a seguinte.

Basead nesse primeiro exercicio nos implementamos a primeira etapa do processo de compialcao utilziado um gerando de analsiaor lexico.



