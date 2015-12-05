Construindo o primeiro analisador léxico com JFlex
======

JFlex é um gerador de analisador léxico escrito em Java. Ele recebe como entrada uma especificação baseada em um conjunto de expressões regulares e gera um algoritmo que lê os caracteres descritos no arquivo arquivo fonte combinando com os padrões definidos na especificação.

### Instalando o JFlex

Faça o download do JFlex nesse link: http://jflex.de/download.html

Nesse primeiro exemplo nós vamos usar o JFlex integrado com a IDE Eclipse.

Você deve fazer a instalação da biblioteca no seu ambiente de desenvolvimento, eu sugiro uma rápida leitura nas instruções de instalação nesse [link](http://jflex.de/installing.html).

Você pode adicionar a biblioteca **jflex-1.6.1.jar** na configuração de *Build Path* do seu projeto no Eclipse.

### Criado uma analisador léxico

Crie um novo projeto no Eclipse através do menu `File -> New -> Java Project` chamado `lexicalanalyzer` e adicione a biblioteca do JFlex - **jflex-1.6.1.jar** - nas configurações de *Build Path* do projeto. Você também deve crair uma pacote chamado `br.com.johnidouglas`.

Se preferir você pode criar um projeto usando o Maven e adicionar a seguinte dependência.

No campo `Group Id` informe `br.com.johnidouglas` e no campo `Artifact Id` informe `lexicalanalyzer`

```
<dependency>
	<groupId>de.jflex</groupId>
	<artifactId>jflex</artifactId>
	<version>1.6.1</version>
</dependency>
```

Essa imagem mostra a estrutura do projeto no Eclipse.

![](../imagem/project-estrucutre-jflex.png)

Em seguida vamos criar a classe que ira gerar o analisador léxico a cada nova alteração no arquivo de especificação. Isso evita o uso da linha de comando para gerar a classe Java responsável por implementar o algoritmo de reconhecimento de tokens.

Vamos chamar essa classe de `Generator`.

```
package br.com.johnidouglas.lexicalanalyzer;

import java.io.File;
import java.nio.file.Paths;

public class Generator {

	public static void main(String[] args) {
		
		String rootPath = Paths.get("").toAbsolutePath(). toString();
		String subPath = "/src/main/java/br/com/johnidouglas/lexicalanalyzer/";
		
		String file = rootPath + subPath + "language.lex";
		
		File sourceCode = new File(file);
		
		jflex.Main.generate(sourceCode);
		
	}
}
```

O arquivo especificação possui as definições da nossa linguagem de programação. Para criar esse arquivo na IDE do Eclipse vá em `File -> New -> Other -> File` salve esse arquivo com o nome de `language.lex` na raiz do projeto.

No arquivo digite o seguinte conteúdo.

```

package br.com.johnidouglas.lexicalanalyzer;

%%

%{

private void imprimir(String descricao, String lexema) {
    System.out.println(lexema + " - " + descricao);
}

%}


%class LexicalAnalyzer
%type void


BRANCO = [\n| |\t|\r]
ID = [_|a-z|A-Z][a-z|A-Z|0-9|_]*
SOMA = "+"
INTEIRO = 0|[1-9][0-9]*

%%

"if"                         { imprimir("Palavra reservada if", yytext()); }
"then"                       { imprimir("Palavra reservada then", yytext()); }
{BRANCO}                     { imprimir("Espaço em branco", yytext()); }
{ID}                         { imprimir("Identificador", yytext()); }
{SOMA}						 { imprimir("Operador de soma", yytext()); }
{INTEIRO}					 { imprimir("Número Inteiro", yytext()); }

. { throw new RuntimeException("Caractere inválido " + yytext()); }
```

Pronto, nós já temos um analisador léxico onde é possível reconhecer um conjunto de lexemas baseado em padrões definidos no arquivo de especificação - `language.lex`, agora nos precisamos testar o analisador léxico. 

Para isso crie uma nova classe chamada `LanguageSextaFase` e inclua o seguinte código nela.

```
package br.com.johnidouglas.lexicalanalyzer;

import java.io.IOException;
import java.io.StringReader;

public class LanguageSextaFase {

	public static void main(String[] args) throws IOException {
		
		String expr = "if 2 + 3+a then";
		
		LexicalAnalyzer lexical = new LexicalAnalyzer(new StringReader(expr));
		lexical.yylex();
		
	}
}

```

Para executar o analisador léxico siga os seguintes passos:

1. Execute a classe Generator: Observe que no projeto foi criado a classe `LexicalAnalyzer`. Essa classe possui a implementação das regras definidas no arquivo de especificação `language.lex` é representa o algoritmo do analisador léxico.

2. Execute a classe `LanguageSextaFase`: Essa classe vai executar o analisador léxico passando como parametro uma expressão de uma lingaugem de programação qualquer.

Observe que a saída será do analisador léxico será a seguinte.

```
if - Palavra reservada if
  - Espaço em branco
2 - Número Inteiro
  - Espaço em branco
+ - Operador de soma
  - Espaço em branco
3 - Número Inteiro
+ - Operador de soma
a - Identificador
  - Espaço em branco
then - Palavra reservada then

```

Baseado nesse primeiro exercício nos implementamos a primeira etapa do processo de compilação utilizado um gerando de analisador léxico.

### Construindo uma analisador léxico para a linguagem Pascal

Vamos aprimorar o nosso analisador léxico para que ele reconheça os tokens pertencentes a linguagem de programação Pascal.

Essa imagem mostra a estrutura do projeto no Eclipse.

![](../imagem/project-estrucutre-jflex-pascal.png)

Crie uma classe chamada `GeneratorPascal`, para isso você deve criar um novo pacote Java dentro do projeto `lexicalanalyzer`, vamos chamar esse pacote de `pascal` - `br.com.johnidouglas.lexicalanalyzer.pascal` .

A classe `GeneratorPascal` deve ter a seguinte implementação.

```
package br.com.johnidouglas.lexicalanalyzer.pascal;

import java.io.File;
import java.nio.file.Paths;

public class GeneratorPascal {

	public static void main(String[] args) {
		
        String rootPath = Paths.get("").toAbsolutePath(). toString();
        String subPath = "/src/main/java/br/com/johnidouglas/lexicalanalyzer/pascal/";

        String file = rootPath + subPath + "pascal.lex";

        File sourceCode = new File(file);
        
        jflex.Main.generate(sourceCode);
   
	}
}
```

Agora vamos criar um arquivo de código chamado `program.pas` esse arquivo vai conter o código fonte do programa escrito em Pascal.

```
Program CalcularSalario
	Var Idade : Integer; 
	Var ValorSalario: Real; 
	Begin 
		If (Idade > 10) Then 
			ValorSalario := 100; 
		Else 
			ValorSalario := ValorSalario * 2; 
End.
```

Vamos criar uma classe chamada `PascalToken` essa classe vai representar um token reconhecido na linguagem e deve ter duas propriedades, `name`, `value`, `line` e `column`.

```
package br.com.johnidouglas.lexicalanalyzer.pascal;

public class PascalToken {
	
	public String name;
	public String value;
	public Integer line;
	public Integer column;
	
	public PascalToken(String name, String value, Integer line, Integer column) {
		this.name = name;
		this.value = value;
		this.line = line;
		this.column = column;
	}
}


```

Crie uma classe chamada `PascaLexicalAnalyzer` com o seguinte código.

```
package br.com.johnidouglas.lexicalanalyzer.pascal;

import java.io.FileReader;
import java.io.IOException;
import java.nio.file.Paths;


public class PascaLexicalAnalyzer {

	public static void main(String[] args) throws IOException {
		
		String rootPath = Paths.get("").toAbsolutePath(). toString();
        String subPath = "/src/main/java/br/com/johnidouglas/lexicalanalyzer/pascal";

        String sourceCode = rootPath + subPath + "/program.pas";
		
		LexicalAnalyzer lexical = new LexicalAnalyzer(new FileReader(sourceCode));
		
		PascalToken token;
		
		while ((token = lexical.yylex()) != null) {
			System.out.println("<" + token.name + ", " + token.value + "> (" + token.line + " - " + token.column + ")");
		}
	}
}
```

Essa classe é responsável por executar o analisador léxico.

Nos precisamos criar o arquivo que vai conter as especificações da linguagem de programação Pascal para que os tokens sejam reconhecidos.

Crie o arquivo `pascal.lex` esse arquivo deve conter a seguinte especificação.

```
package br.com.johnidouglas.lexicalanalyzer.pascal;

import java_cup.runtime.*;

%%

%{


private PascalToken createToken(String name, String value) {
	return new PascalToken( name, value, yyline, yycolumn);
}

%}

%public
%class LexicalAnalyzer
%type PascalToken
%line
%column

inteiro = 0|[1-9][0-9]*
brancos = [\n| |\t]

program = "program"

%%

{inteiro} { return createToken("inteiro", yytext()); }
{program} { return createToken(yytext(), "");} 
{brancos} { /**/ }

. { throw new RuntimeException("Caractere inválido " + yytext() + " na linha " + yyline + ", coluna " +yycolumn); }

```

O processo para executar o analisador léxico e o mesmo já utilizado no primeiro exemplo.

* Execute a classe `GeneratorPascal` essa classe é responsável por gerar o algoritmo do analisador léxico.

* Execute a classe `PascaLexicalAnalyzer` essa classe é responsável por executar o analisador léxico.

Você deve ter percebido que alguns erros ocorreram, isso por que nosso arquivo de especificação esta incompleto, ou seja, não possui todos os tokens da linguagem Pascal.

Você deve implementar o restante da especificação do Pascal através de expressões regulares.