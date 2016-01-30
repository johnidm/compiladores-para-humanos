Implementando um interpretador de operações matemáticas básicas
======

Nesse etapa nós vamos construir um interpretador de operações matemáticas básicas, o objetivo é demonstrar como podemos implementar um tradutor dirigido por sintaxe.

Vamos criar um analisador léxico com o JFlex e um analisador sintático com o Java Cup, ambas a bibliotecas pode ser baixadas nesse link - http://jflex.de/download.html. 

A tradução dirigida por sintaxe tem como objetivo associar a execução de cada regra da gramatical uma ação semântica. Nesse projeto a ação semântica será avaliar a operação matemática e executar ela, interrompendo a operação e exibindo o seu resultado quando encontrar o simbolo de final de instrução.

Nós vamos utilizar um projeto Java, você pode utilizar qualquer IDE de desenvolvimento de sua preferencia. Vamos omitir os detalhes do projeto e focar somente nas partes relevantes para mostrar como um tradutor dirigido por sintaxe pode ser implementado.

Vamos começar definido as regras léxicas do nosso projeto, basicamente vamos avaliar as operações matemáticas básicas lidas de um arquivo, caso encontro um sinal de `;` deve ser impresso o resultado da expressão.

Exemplo de sentenças validas:

```
(2 * 1) * 9; 1+ 3;
(1 + 3 + 5) * 8;
```

Crie um arquivo chamado `calc.l` e salve a sentenças nesse arquivo.

Crie um arquivo chamado `Lexer.lex` e coloque o seguinte conteúdo.

```
import java_cup.runtime.*;

%%

%class Lexer
%unicode
%cup
%line
%column

%{
    private Symbol symbol(int type) {
        return new Symbol(type, yyline, yycolumn);
    }
    private Symbol symbol(int type, Object value) {
        return new Symbol(type, yyline, yycolumn, value);
    }
%}

LineTerminator = \r|\n|\r\n
InputCharacter = [^\r\n]
WhiteSpace    = {LineTerminator} | [ \t\f]

Digit          = [0-9]
Number         = {Digit} {Digit}*
Letter         = [a-zA-Z] 

%%

<YYINITIAL> { 
    {Number}        { return symbol(Sym.NUMBER, new Integer(Integer.parseInt(yytext()))); }
   
    "+"             { return symbol(Sym.PLUS); }    
    "-"             { return symbol(Sym.MINUS); }
    "*"             { return symbol(Sym.TIMES); }
    "/"             { return symbol(Sym.DIVIDED); }
    
    "("             { return symbol(Sym.LPAREN); }
    ")"             { return symbol(Sym.RPAREN); }
        
    ";"             { return symbol(Sym.SEMI); }

    {WhiteSpace} {}
}

<<EOF>>             { return symbol( Sym.EOF ); }

[^]                 { throw new Error("Illegal character <" + yytext() + ">");}
```

Essas são a regras léxicas das expressões matemáticas.

Vamos utilizar Gramática Livres de Contexto para definir as regras sintáticas da nossa linguagem de programação.

Observe que cada regra sintática tem uma ação vinculada. Por exemplo `expr:e SEMI {: System.out.println(">> " + e); :}`, O código `System.out.println(">> " + e);` referente a expressão sintática `expr:e SEMI` será executa quando esse expressão for avaliada pelo compilador. 

Veja em detalhes as outras ações semânticas vinculadas a avaliação das expressões sintáticas.

```

import java_cup.runtime.*;

parser code {:
    
    public void report_error(String message, Object info)  {
        System.out.println("Warning - " + message);
    }
    
    public void report_fatal_error(String message, Object info)  {
        System.out.println("Error - " + message);
        System.exit(-1);
    }

:}

terminal            SEMI, PLUS, MINUS, TIMES, DIVIDED, LPAREN, RPAREN;
terminal Integer    NUMBER;        

non terminal            expr_list;
non terminal Integer    expr;

precedence left PLUS, MINUS;
precedence left TIMES, DIVIDED;

expr_list ::= expr_list expr:e SEMI         {: System.out.println(">> " + e); :}
            | expr:e SEMI                   {: System.out.println(">> " + e); :}
;
expr      ::= expr:e1 PLUS  expr:e2         {: RESULT = e1+e2;       :}
            | expr:e1 MINUS expr:e2         {: RESULT = e1-e2;       :}
            | expr:e1 TIMES expr:e2         {: RESULT = e1*e2;       :}
            | expr:e1 DIVIDED expr:e2       {: RESULT = e1/e2;       :}
                     
            | LPAREN expr:e RPAREN          {: RESULT = e;           :}
            | NUMBER:n                      {: RESULT = n;           :}
;
```

Crie um arquivo chamado `Parser.cup` e coloque conteúdo do analisador sintatico acima.

O código utilizada para executar o interpenetrador pode ser visto abaixo. Observe que ele abre o arquivo `calc.l`.

``` 
import java.io.FileNotFoundException;
import java.io.FileReader;

public class Main {

    public static void main(String[] args) throws FileNotFoundException {
        
        String fileName = "/home/johni/Projects/compiler-jflex-javacup-examples/OtherCalc/src/calc.l";  

        Parser parser = new Parser(new Lexer(new FileReader(fileName)));
        try {
            parser.parse();
        }       
        catch (Exception e) {
            System.out.println("Falha geral.");
        }
    }
}
```

O projeto do do interpretador esta pronto, agora no precisamos gerar o analisador léxico e o analisador sintático para concluir.

Acesse o programa de linha de comando e navegue até o diretório onde os arquivos `lex` e `cup`  estão.

Por exemplo:

```
cd /home/johni/Projects/interpreter/calc/src/
```

Depois você deve localizar o diretório onde os arquivos `jar` do JFlex e Java Cup estão, e executar as seguintes linhas de código.

```
java -jar /home/johni/Projects/interpreter/vendor/jflex-1.6.1.jar Lexer.lex

java -jar /home/johni/Projects/interpreter/vendor/java-cup-11a.jar -parser Parser -symbols Sym Parser.cup
```

Você pode executar o projeto e ver que cada expressão matemática esta sendo avaliada no momento que ele é encontrada. 