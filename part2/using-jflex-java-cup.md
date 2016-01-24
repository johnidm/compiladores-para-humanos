Construindo um Compilador Front-End com JFlex e Java CUP
======

Para integrar com o analisador léxico criado através do JFlex vamos utilizar a ferramenta Java Cup que já faz parte do pacote de desenvolvimento de JFlex. Mais informações nesse [link](http://www2.cs.tum.edu/projects/cup/).

Vamos começar definindo a sintaxe da linguagem de programação.

Lexical especification

```
$programa$ {

    @idade INT:
    @nome STR:
    
    &imprmir(^idade, ^nome) [
        se verdade [
            @idade -> ^idade:
            @nome -> ^nome;
        ]
    ]

    %imprimir(12, ~Papitoo~):
}
```

Crie um novo projeto chamado `SyntacticAnalyzer` e um pacote chamado `br.com.johnidouglas`, salve o código em um arquivo chamado `program.pg`.

As seguintes convenções serão utilizadas na nova linguagem.

| Lexema | Descrição                              |
|--------|----------------------------------------|
| inicio | Palavra reservada – início do programa |
| para   | Palavra reservada                      |
| 1      | Constante numérica                     |
| ate    | Palavra reservada                      |
| 10     | Constante numérica                     |
| faca   | Palavra reservada                      |
| valor  | ID – variável                          |
| <-     | Sinal de atribuição                    |
| valor  | ID – variável                          |
| *      | Constante numérica                     |
| fim    | Palavra reservada – fim do programa    |

A próxima etapa é definir a gramática da linguagem de programação utilizando uma gramática livre de contexto. Observe os símbolos terminais e não terminais, símbolo inicial e a regra de produção criados.

```
G = ({INICIO, LACO, FIM }, {inicio, para, numero, ate, faca, id, atribuicao, valor, multiplicacao, fim}, REGRA, INICIO)
REGRA {
	INICIO → inicio LACO fim;
	LACO → para numero ate numero faca id atribuicao id multiplicação numero
}
```

Vamos começar criando o analisador léxico através do JFlex. Crie uma classe chamada `Generator` com o seguinte código.

```
package br.com.johnidouglas;

import java.io.File;
import java.nio.file.Paths;

public class Generator {

	public static void main(String[] args) {

		String rootPath = Paths.get("").toAbsolutePath().toString();
		String subPath = "/src/br/com/johnidouglas/";

		
		String file = rootPath + subPath + "language.lex";

		File sourceCode = new File(file);

		jflex.Main.generate(sourceCode);

	}

}
```

Agora vamos criar o arquivo de especificação léxica chamada `Lexer.lex` com o seguinte conteúdo.

```
package br.com.johnidouglas;

import java_cup.runtime.*;

%%

%{

private void log(String descricao, String lexema) {
     System.out.println(lexema + " - " + descricao);

}

%}

%cup
%public
%class LexicalAnalyzer
%type java_cup.runtime.Symbol

BRANCO = [\n|\s|\t\r]
CONSTANTE_NUMERICA = 0|[1-9][0-9]*
ID = [A-Za-z_][A-Za-z_0-9]*

%%

"inicio"     	{ log("Palavra reservada", yytext()); return new Symbol(sym.INICIO); }
"fim"        	{ log("Palavra reservada", yytext()); return new Symbol(sym.FIM); }
"para"        	{ log("Palavra reservada", yytext()); return new Symbol(sym.PARA); }
"ate"        	{ log("Palavra reservada", yytext()); return new Symbol(sym.ATE); }
"faca"        	{ log("Palavra reservada", yytext()); return new Symbol(sym.FACA); }

{CONSTANTE_NUMERICA}     	{ log("Constante numerica", yytext()); return new Symbol(sym.NUMERO); }
{ID}                    	{ log("Id", yytext()); return new Symbol(sym.ID); }
"*"                        	{ log("Operador de multiplicacao", yytext()); return new Symbol(sym.MULTIPLICACAO); }
"<-"                     	{ log("Sinal de atribuicao", yytext()); return new Symbol(sym.ATRIBUICAO); }

{BRANCO} {  }

. { throw new RuntimeException("Caractere inválido " + yytext() + " na linha " + yyline + ", coluna " +yycolumn); }
```

Para que o analisador léxico desenvolvido com o JFlex funcione corretamente em conjunto com o analisador sintático Java Cup as seguintes foram feitas no arquivo lex.

* `%cup` – indica que o analisador léxico será integrado ao Java Cup
* `%type java_cup.runtime.Symbol` – especifica o tipo de retorno de tokens de
acordo com a interface Symbol.
* `return new Symbol(sym.INICIO)` - Cada token identificado deve retornar
uma instancia da classe Symbol identificada por uma constante numérica. Essa constante é gerada automaticamente pelo Java Cup.

Nos precisamos criar um arquivo com as especificações sintáticas. Crie um novo arquivo e salve com o nome `Parser.cup` com o seguinte conteúdo.

```
package br.com.johnidouglas;

import java_cup.runtime.*;
import java.io.*;
import java.util.*;

terminal INICIO, FIM, PARA, ATE, FACA, NUMERO, ID, MULTIPLICACAO, ATRIBUICAO;

non terminal _INICIO_, _LACO_, _BLOCO_;

start with _INICIO_;

_INICIO_ ::= INICIO _BLOCO_ FIM;

_BLOCO_ ::= _BLOCO_ _LACO_ | _LACO_;

_LACO_ ::= PARA NUMERO ATE NUMERO FACA ID ATRIBUICAO ID MULTIPLICACAO NUMERO;
```

Esse arquivo contém as especificações sintáticas, que nada mais é do que a gramática da linguagem de programação. Da mesma forma que o analisador léxico gerado pelo Jflex, o analisador sintático também irá gerar uma classe Java.

Através da linha de comando acesse a pasta onde o arquivo linguagem.cup esta salvo.

Execute a seguinte linha de comando:

`java –cp <caminho do jar do Java CUP> java_cup.Main -parser SyntacticAnalyzer linguagem.cup
`
Exemplo de comando:

```
cd /Users/johni/Documents/workspace/SyntacticAnalyzer/src/br/com/johnidouglas
java -cp "/Users/johni/.m2/repository/edu/princeton/cup/java-cup/10k/java-cup-10k.jar" java_cup.Main -parser SyntacticAnalyzer language.cup
```

Caso o comando seja executado com sucesso um arquivo chamado
`SyntacticAnalyzer.java` será criado com regras sintáticas da gramática definida.

Pronto, agora crie uma classe chamada `Compiler` e inclua o seguinte código nela.

```
package br.com.johnidouglas;


import java.io.File;
import java.io.FileInputStream;

import java.io.InputStreamReader;
import java.nio.file.Paths;

public class Compiler {

    public static void main(String[] args) throws Exception {

    	String rootPath = Paths.get("").toAbsolutePath().toString();
		String subPath = "/src/br/com/johnidouglas/";

		String sourcecode = rootPath + subPath + "sextafase.pg";

        File in =  new File(sourcecode);

        InputStreamReader sourceCode = new InputStreamReader(new FileInputStream(in));

        LexicalAnalyzer lexer = new LexicalAnalyzer( sourceCode );    
        SyntacticAnalyzer parser = new SyntacticAnalyzer(lexer);
        parser.parse();
       
    }
}
```

Essa classe vai executar o compilador da linguagem.

Para rodar esse exemplo siga os seguintes passos:

* Execute linha de comando `java –cp <caminho do jar do Java CUP> java_cup.Main -parser SyntacticAnalyzer linguagem.cup`.

* Execute a classe `Generator`.

* Execute a classe `Compilador`.

UAU, nós já concluímos mais uma etapa projeto do compilador. Parabéns.

