Construindo um Compilador Front-End com JFlex e Java CUP
======

Para integrar com um analisador léxico criado através do JFlex vamos utilizar a ferramenta Java Cup que já faz parte do pacote de desenvolvimento de JFlex. Mais informações nesse [link](http://www2.cs.tum.edu/projects/cup/).

Vamos começar definindo a sintaxe da linguagem de programação.

```
inicio 
	para 1 ate 10 faca 
		valor <- valor * 10 
fim
```

Salve esse código em um arquivo chamado `sextafase.pg`.

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

A próxima etapa é definir a gramática da linguagem de programação utilizando a notação da gramática livre de contexto. Observe os símbolos terminais e não terminais, símbolo inicial e a regra de produção

```
G = ({INICIO, LACO, FIM }, {inicio, para, numero, ate, faca, id, atribuicao, valor, multiplicacao, fim}, REGRA, INICIO)
REGRA {
	INICIO → inicio LACO fim;
	LACO → para numero ate numero faca id atribuicao id multiplicação numero
}
```

Nos vamos começar criando o  analisador léxico através do JFlex. Para isso crie um novo projeto chamado `SyntacticAnalyzer` então crie uma classe chamada `Generator` com o seguinte código.

```
package sintatico;

import java.io.File;

public class Generator {
  
	public static void main(String[] args) {
	
		String path = "/home/johni/Projects/collections-code-kata/demo-analisador-sintatico/src/sintatico/";		
		String arquivo = path + "linguagem.lex";
	    
	    File file = new File(arquivo );        
	    jflex.Main.generate(file);
	    
	}		
}
```

Agora vamos criar o arquivo de especificação léxica chamada `language.lex` com o seguinte conteúdo.

```
package sintatico;

import java_cup.runtime.*;

%%

%{

private void log(String descricao, String lexema) {
	 System.out.println(lexema + " - " + descricao);
	 
}

%}

%cup
%public
%class AnalisadorLexico
%type java_cup.runtime.Symbol

BRANCO = [\n|\s|\t\r]
CONSTANTE_NUMERICA = 0|[1-9][0-9]*
ID = [A-Za-z_][A-Za-z_0-9]*

%%

"inicio" 	{ log("Palavra reservada", yytext()); return new Symbol(sym.INICIO); }
"fim"		{ log("Palavra reservada", yytext()); return new Symbol(sym.FIM); }
"para"		{ log("Palavra reservada", yytext()); return new Symbol(sym.PARA); }
"ate"		{ log("Palavra reservada", yytext()); return new Symbol(sym.ATE); }
"faca"		{ log("Palavra reservada", yytext()); return new Symbol(sym.FACA); }

{CONSTANTE_NUMERICA} 	{ log("Constante numerica", yytext()); return new Symbol(sym.NUMERO); }
{ID}					{ log("Id", yytext()); return new Symbol(sym.ID); }
"*"						{ log("Operador de multiplicacao", yytext()); return new Symbol(sym.MULTIPLICACAO); }
"<-" 					{ log("Sinal de atribuicao", yytext()); return new Symbol(sym.ATRIBUICAO); }

{BRANCO} {  }

. {  }

```

Para que o analisador léxico desenvolvido com o JFlex funcione corretamente em conjunto com o analisador sintático Java Cup as seguintes oram feitas no arquivo lex.

* %cup – indica que o analisador léxico será integrado ao Java Cup
* %type java_cup.runtime.Symbol – especifica o tipo de retorno de tokens de
acordo com a interface Symbol.
* return new Symbol(sym.INICIO); - Cada token identificado deve retornar
uma instancia da classe Symbol identificada por uma constante numérica
(sym.INICIO). Essa constante é gerada automaticamente pelo Java Cup.

Nos precisamos criar um arquivo com as especificações sintáticas. Crie um novo arquivo e salve com o nome `language.cup` com o seguinte conteúdo.

```
```

Esse arquivo contém as especificações sintáticas, que nada mais é do que a gramática da linguagem de programação. Da mesma forma que o analisador léxico gerado pelo Jflex, o analisador sintático também irá gerar uma nova classe Java.

Através da linha de comando acesse a pasta onde o arquivo linguagem.cup esta salvo.

Execute a seguinte linha de comando:

`java –cp <caminho do jar do Java CUP> java_cup.Main -parser SyntacticAnalyzer linguagem.cup
`

Caso o comando seja executado com sucesso um arquivo chamado
`SyntacticAnalyzer .Java` foi criado com regras sintáticas da gramática definida.

Pronto agora cria uma classe chamada `Compiler` e inclua o seguinte código nela.

```
package sintatico;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.InputStreamReader;

public class Compilador {

	public static void main(String[] args) throws Exception {
		
		String path = "";
		
		String sourcecode = path + "programa.ptg"; 
			
		File in =  new File(sourcecode);
		
		InputStreamReader r = new InputStreamReader(new FileInputStream(in));
		
		/*
		AnalisadorLexico lexico = new AnalisadorLexico( r );	
		AnalisadorSintatico sintatico = new AnalisadorSintatico(lexico);
		sintatico.parse();
		*/	
	}
}
```

Essa classe vai executar o compilador da linguagem de programação. 


