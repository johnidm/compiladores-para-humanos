Contruindo o primeiro analisador léxico com JFlex
======

JFlex é um gerador de analisador léxico escrito em Java. Ele recebe como entrada uma especificação baseada em um conjunto de expressões regulares e gera um algoritmo que lê os caracteres descritos no arquivo arquivo fonte combinando com os padrões definidos na especificação.

### Instalando o JFlex

Faça o download do JFlex nesse link: http://jflex.de/download.html

Nesse primeiro exemplo nós vamos usar o JFlex integrado com a IDE Eclipse.

Voce deve fazer a instalação da biblitoeca no seu ambiente de desenvolvimento, eu sugiro uma rápida leitura nas intruções de instalação nesse [link](http://jflex.de/installing.html).

Você pode adicioanar a biblitoeca **jflex-x.x.x.jar** na configuração de *Build Path* do seu projeto no Eclipse.

### Criado uma analisador léxico

Crie um novo projeto no Eclipse através do menu `File -> New -> Java Project` chamado `Analisador Lexico` e adicione a biblitoeca do JFlex - **jflex-x.x.x.jar** - nas confgiurações de *Build Path* do projeto.

Se preferir você pode crair um projeto usando o Mavem e adicionar a seguinte dependencia.

```
<dependency>
	<groupId>de.jflex</groupId>
	<artifactId>jflex</artifactId>
	<version>1.6.1</version>
</dependency>
```


Em seguida vamos criar uma classe que ira gerar o analisador léxico a cada nova alteração no arquivo de especificação. Isso evita o uso da linha de comando para gerar a classe Java responsavel por implementar o algoritmo de reconhecimento de tokens.

Vamos chamar essa classe de `Generator`.

```

```

O arquivo especificação possui as definições da nossa linguagem de programação. Para criar esse arquivo na IDE do Eclipse vá em `File -> New -> Other -> File` save esse arquivo com o nome de `linguagem.lex` na raiz do projeto.

No arquivo digite o seguinte conteúdo.

```
package br.com.johnidouglas.analisadorlexico;

%%

%{

private void imprimir(String descricao, String lexema) {
	System.out.println(lexema + " - " + descricao);
}

%}

%public
%class LexicalAnalyzer
%type void


BRANCO = [\n| |\t|\r]
ID = [_|a-z|A-Z][a-z|A-Z|0-9|_]*
INTEIRO = 0|[1-9][0-9]*
PONTOFLUTUANTE = [0-9][0-9]*"."[0-9]+
OPERADORES_MATEMATICOS = ("+" | "-" | "*" | "/")

%%

"if" 						{ imprimir("Intrucao if", yytext()); }
"then" 						{ imprimir("Intrucao then", yytext()); }
{BRANCO} 					{ imprimir("Branco", yytext()); }
{ID} 						{ imprimir("Identificador", yytext()); }
{INTEIRO} 					{ imprimir("Numero", yytext()); }
{PONTOFLUTUANTE} 			{ imprimir("Ponto plututante", yytext()); }
{OPERADORES_MATEMATICOS} 	{ imprimir("Operadores matematatico", yytext()); }
"==" 						{ imprimir("Operador igualdade", yytext()); }

. { throw new RuntimeException("Caractere invalido " + yytext() + " na linha " + yyline + ", coluna " + yycolumn); }
```

Pronto, nós já temos um analisador léxico onde é possivel reconhcer um conjunto de lexemas baseado em padroes definidos no arquivo de especificaçao - `lingaugem.lex`, agora nos precisamos testar o analizador léxico. 

Para isso crie uma nova classe chamada `LinguagemSextaFase` e inclua o seguinte código nela.


Para executar o analizador léxico siga os seguintes passos:

1. Execute a classe Gerador: Observe que no seu projeto foi criado a classe `AnalisadorLexico`. Essa classe possui a implementação das regras definidas no arquivo de especificacao `lingaugem.lex` é representa o algoritmo que do analisador léxico.

2. Crie a classe `Lingaugem` - `File -> New -> Class` - que irá implementar a chamada do analisador léxico.

A classe deve implementar o seguinte código

Execute essa classe o observe que a saída será a seguinte.

Basead nesse primeiro exercicio nos implementamos a primeira etapa do processo de compialcao utilziado um gerando de analsiaor lexico.



