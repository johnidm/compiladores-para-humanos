Construindo um Compilador Front-End com JFlex e Java CUP
======

Para integrar com o analisador léxico criado através do JFlex vamos utilizar a ferramenta Java Cup que já faz parte do pacote de desenvolvimento de JFlex que pode ser obtido nesse link - http://jflex.de/download.html. 

Vocë pode obter mais detalhes do funcionamento do Java Cup em http://www2.cs.tum.edu/projects/cup/. 

Ambas as ferramentas utilizam a linguagem de programação Java, vocë pode criar um projeto Java utilizando alguma IDE de desenvolvimento de sua preferencia, nos vamos apresentar apenas as partes relevantes na construção do analisador léxico e sintático omitindo detalhes do projeto.

A construção de uma linguagem de programação deve iniciar pela definição léxica, abaixo é apresentado um fragmento de código escrito na linguagem que vamos construir que vai se chamar**SextaFase**.

```
program <{    
    @age INT:
    @name STR:
    
    &imprmir(^idade; ^nome) [
        if TT [
            $idade -> ^idade:
            $nome -> ^nome:
        ]        
        if FF [         
            $nome -> ^nome:
        ]        
    ]
    %imprimir(~sdfsd~; 12; 12):
}>
```

Salvar esse código em um arquivo do seu projeto chamado `Program.pg`.

As seguintes convenções serão utilizadas na linguagem **SextaFase**:

* A palavra reservada `program` deve identificar o inicio do programa;
* O bloco de inicio de fim do programa deve ser identificado através dos sinais `<{` e `}>`.
* O sinal de `:` determina o termino de uma declaração;
* Variáveis devem ser declaradas com o sinal `@`, seguido do nome formado por um conjunto de letras e seu respectivo tipo: `INT` ou `STR`;
* A declaração de funções deve ser precedida pelo sinal `&` seguido por um conjunto de letras que formam o nome;
* Os parâmetros das funções devem ser identificados pelo sinal `^` seguido por um conjunto de letras;
* As constantes verdadeiro e falso são identificados respectivamente por `TT` e `FF`;
* Os bloco de inicio e fim de uma instrução é identificado por `[` e ']';
* Operações de atribuição são identificadas pelo símbolos `->`;
* A strings devem estar entre o sinal de `~`;
* As variáveis deve ser usadas colocando o sinal `$` na frente do nome;
* As funções são chamadas colocando o sinal `%` na frente do nome;
* Nas funções os parâmetros são separados por `;` tando na declaração como na chamada;


Agora vamos criar o arquivo de especificação léxica. Dentro do projeto crie um arquivo chamado `Lexer.lex` com o seguinte conteúdo.

```
import java_cup.runtime.Symbol;

%%

%cup
%public
%class Lexer
%line
%column


DIGIT = [0-9]
LETTER = [a-zA-Z_]
COMMENT = \|\|.*\n
STRING = \~{LETTER}+\~
INTEGER = {DIGIT}+
VARIABLE = @{LETTER}+
ASSIGNMENT = \${LETTER}+
FUNCTION = &{LETTER}+
FUNCTION_PARAMS = \^{LETTER}+ 
CALL_FUNCTION = %{LETTER}+
IGNORE = [\n|\s|\t\r]

%%

<YYINITIAL> {

    "program"       {return new Symbol(Sym.PROGRAM); }
    "if"            {return new Symbol(Sym.IF); }
    
    "<{"            {return new Symbol(Sym.BEGIN); }
    "}>"            {return new Symbol(Sym.END); }
    
    "INT"           {return new Symbol(Sym.INTEGER_TYPE); }
    "STR"           {return new Symbol(Sym.INTEGER_TYPE); }

    ":"             {return new Symbol(Sym.COLON); }
    "("             {return new Symbol(Sym.LEFT_PARAMETER); }
    ")"             {return new Symbol(Sym.RIGHT_PARAMETER); }    
    "["             {return new Symbol(Sym.LEFT_BRACKETS); }
    "]"             {return new Symbol(Sym.RIGHT_BRACKETS); }    
    
    ";"             {return new Symbol(Sym.SEMICOLON); }
    
    "TT"            {return new Symbol(Sym.TT); }
    "FF"            {return new Symbol(Sym.FF); }
    
    "->"            {return new Symbol(Sym.SYMBOL_ASSIGNMENT); }
        
    {CALL_FUNCTION} {return new Symbol(Sym.CALL_FUNCTION); }
    
    {ASSIGNMENT}    {return new Symbol(Sym.ASSIGNMENT); }
    
    {STRING}        {return new Symbol(Sym.STRING); }
    {INTEGER}       {return new Symbol(Sym.INTEGER); }  
    
    
    {FUNCTION}      {return new Symbol(Sym.FUNCTION); }

    {FUNCTION_PARAMS}   {return new Symbol(Sym.FUNCTION_PARAMS); }
    
    {VARIABLE}      {return new Symbol(Sym.VARIABLE); }
    
    {IGNORE}        {}
    {COMMENT}       {}

}

<<EOF>> { return new Symbol( Sym.EOF ); }


[^] { throw new Error("Illegal character: "+yytext()+" at line "+(yyline+1)+", column "+(yycolumn+1) ); } 

```

Para que o analisador léxico desenvolvido com o JFlex funcione corretamente em conjunto com o analisador sintático Java Cup as seguintes instruções devem ser incluídas no arquivo lex.

* `%cup` – indica que o analisador léxico será integrado ao Java Cup;
* `%type java_cup.runtime.Symbol` – especifica o tipo de retorno dos tokens quando identificados.
* `return new Symbol(sym.INICIO)` - Cada token identificado deve retornar
uma instancia da classe Symbol que representa uma constante numérica. Essa constante é gerada automaticamente pelo Java Cup.

A próxima etapa é definir a gramática da linguagem de programação, nós vamos utilizar Gramática Livres de Contexto. 

Abaixo um exemplo de uma gramática que pode ser utilizada para montar uma frase.

```
G = ({SENTENÇA, SN, SV, ARTIGO, VERBO, SUBSTANTIVO, COMPLEMENTO}, {aluno, o, estudou, compiladores} , P, SENTENÇA)
    
P { 
    SENTENÇA → SN SV
    SN → ARTIGO SUBSTANTIVO
    SV → VERBO COMPLEMENTO
    COMPLEMENTO → ARTIGO SUBSTANTIVO | SUBSTANTIVO
    ARTIGO → o
    SUBSTANTIVO → aluno | compiladores
    VERBO → estudou
}
```

Com as regras de produção acima é possível validar a seguinte frase: `o aluno estudou compiladores`.

Vamos criar um arquivo com as especificações sintáticas, esse arquivo deve ser chamado de `Parser.cup` e deve ter o seguinte conteúdo.

```
<nome do pacote>

import java_cup.runtime.*;
import java.util.*;
import java.io.*;


parser code {:

    public void report_error(String message, Object info)  {
        System.out.println("Warning - " + message);
    }
    
    public void report_fatal_error(String message, Object info)  {
        System.out.println("Error - " + message);
        System.exit(-1);
    }

:};


terminal PROGRAM, BEGIN, END, VARIABLE, COLON, INTEGER_TYPE, STRING_TYPE, CALL_FUNCTION;
terminal RIGHT_PARAMETER, LEFT_PARAMETER, INTEGER, STRING, SEMICOLON, FUNCTION;
terminal FUNCTION_PARAMS, LEFT_BRACKETS, RIGHT_BRACKETS, TT, FF, IF, ASSIGNMENT;
terminal SYMBOL_ASSIGNMENT;

non terminal program, statements, statement, statement_function;
non terminal decl_variable, decl_call_function, decl_call_params, decl_params, decl_function, decl_if;
non terminal decl_boolean,decl_assignments, decl_assignment;
non terminal params_type, data_types;

start with program;

program ::= PROGRAM BEGIN statements END;

statements ::= statements statement | statement ;

statement ::= decl_variable | decl_call_function | decl_function;

decl_function ::= FUNCTION LEFT_PARAMETER decl_params RIGHT_PARAMETER LEFT_BRACKETS statement_function RIGHT_BRACKETS;

decl_params ::= decl_params SEMICOLON FUNCTION_PARAMS | FUNCTION_PARAMS | ;

statement_function ::= statement_function decl_if | decl_if;
decl_if ::= IF decl_boolean LEFT_BRACKETS decl_assignments RIGHT_BRACKETS;

decl_assignments ::= decl_assignments decl_assignment | decl_assignment ; 

decl_assignment ::= ASSIGNMENT SYMBOL_ASSIGNMENT FUNCTION_PARAMS COLON;

decl_boolean ::= TT | FF;

decl_call_function ::= CALL_FUNCTION LEFT_PARAMETER decl_call_params RIGHT_PARAMETER COLON;
decl_call_params ::= decl_call_params SEMICOLON params_type | params_type | ;
params_type ::= INTEGER | STRING;

decl_variable ::= VARIABLE data_types COLON;
data_types ::= INTEGER_TYPE | STRING_TYPE;
```

> Caso tenha sido criado um pacote no seu projeto Java, você deve substituir no arquivo `Parser.cup` a tag `<nome do pacote>` pelo nome do pacote ou remove-la caso não esteja usando um pacote.

Esse arquivo contém as especificações sintáticas, que nada mais é do que a gramática da linguagem de programação. Da mesma forma que o analisador léxico gerado pelo Jflex, o analisador sintático também irá gerar código Java para validar a sintaxe da linguagem de programação.

Para finalizar o projeto da linguagem de programação vamos criar uma classe que vai executar o compilador.


```
import java.io.FileReader;
import java.nio.file.Paths;

public class Main {

    public static void main(String[] args) {
        
        String rootPath = Paths.get("").toAbsolutePath().toString();
        String subPath = "/src/";

        String sourcecode = rootPath + subPath + "Program.pg";
        
        
        try {
            Parser p = new Parser(new Lexer(new FileReader(sourcecode)));
            Object result = p.parse().value;
            
            System.out.println("Compilacao concluida com sucesso...");
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}
```

> Você pode alterar o nome da classe Java para outro qualquer, nesse exemplo nos estamos utilizando o nome `Main`.

Essa classe vai executar o compilador da linguagem.

Todo o código para o projeto do compilador esta pronto, agora no precisamos gerar o analisador léxico e o analisador sintático. Isso deve ser feito através da linha de comando utilizando o JFlex e o Java Cup.

Acesse o programa de linha de comando e navegue até o diretório onde os arquivos `lex` e `cup`  estão.

Por exemplo:

```
cd /home/johni/Projects/compiler/sextafase-language/src/
```

Depois você deve localizar o diretório onde os arquivo `jar` do JFlex e Java Cup estão, e executar as seguintes linhas de código.

```
java -jar /home/johni/Projects/compiler/vendor/jflex-1.6.1.jar Lexer.lex

java -jar /home/johni/Projects/compiler/vendor/java-cup-11a.jar -parser Parser -symbols Sym Parser.cup
```

Foram gerados os arquivos de código Java `Lexer.java`, `Parser.java` e `Sym.java`. Esses arquivos vão fazer parte do compilador.