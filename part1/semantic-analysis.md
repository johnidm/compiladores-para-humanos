Analise Semântica
======

Até o momento vimos as etapas de análise léxica, que quebra o programa fonte em tokens e a analise sintática, que valida as regras a sintaxe da linguagem de programação.

Não é possível representar com expressões regulares ou com uma gramática livre de contexto regras como: todo identificador deve ser declarado antes de ser usado. Muitas verificações devem ser realizadas com meta-informações e com elementos que estão presentes em vários pontos do código fonte, distantes uns dos outros. O analisador semântico utiliza a árvore sintática e a tabela de símbolos para fazer as analise semantica.

A analise semântica é responsavel por verificar aspectos relacionados ao significado das instruções, essa é a terceira etapa do processo de compilação e nesse momento ocorre a validação de uma serie regras que não podem ser verificadas nas etapas anteriores.

> É importante ressaltar que muitos dos erros semanticos tem origem de regras dependentes da linguagem de programacao.

As validações que não podem ser executadas pelas etapas anteriores devem ser executadas durante a análise semântica a fim de garantir que o programa fonte estaja coerente e o mesmo possa ser convertido para linguagem de máquina.

A análise semântica percorre a árvore sintática relaciona os identificadores com seus dependentes de acordo com a estrutura hierarquica.

Essa etapa também captura informações sobre o programa fonte para que as fases subsequentes gerar o código objeto, um importante componente da analise semântica é a verificação de tipos, nela o compilador verifica se cada operador recebe os operandos permitidos e especificados na linguagem fonte.

Um exemplo que ilustra muito bem essa etapa de validação de tipos é a atribuição de objetos de tipos ou classe diferentes. Em alguns casos, o compilador realiza a conversão automática de um tipo para outro que seja adequado à aplicação do operador. Por exemplo a expressão.

```
var s: String;
s := 2 + ‘2’;
```

Veja o exemplo de um código em Object Pascal:

```
function Soma(a, b : Integer) : Integer;
var 
  i : Integer;
begin
  i := a + b;
  Result := i;
end;
```

No exemplo acima o analisador semântico de ter uma série de preocupações para validar o significado de cada regra de produção.

Vamos utilizar como exemplo a regra de atribuição: 

```
i := a + b;
```

* O identificador `i` foi declarado?
* O identificador `i` é uma variável?
* Qual o escopo da variável `i`?
* Qual é o tipo da variável `i`?
* O tipo da variável `i` é compatível com os demais identificadores, operadores?

Os tipo de dados são muito importantes nessa etapa da compilação, eles são uma notações que as linguagens de programação utilizam para representar um conjunto de valores. Com base nos tipos o analisador semântico pode definir quais valores podem ser manipulados, isso é conhecido com *type checking*.

#### Inferência de tipos

O sistema de tipos de dados podem ser divididos em dois grupos: sistemas dinâmicos e sistemas estáticos. Muitas das linguagens utilizam o sistema estático, esse sistema é predominante em linguagens compiladas, pois essa informação é utilizada durante a compilação e simplifica o trabalho do compilador.

* Sistema de tipo estático: Linguagem como C, Java, Pascal obrigam o programador a definir os tipos das variáveis e retorno de funções, o compilador pode fazer varias checagens de tipo em tempo de compilação.
* Sistema de tipo dinâmico: Variáveis e retorno de funções não possuem declaração de tipos, como exemplo temos linguagens como Python , Perl e PHP.

Algumas linguagens utilizam um mecanismo muito interessantes chamado inferência de tipos, que permite a uma variavel assumir varios tipos durante o seu ciclo de vida, isso permite que a ela possa ter varios varios valores. Linguagens de programação como  **Haskel** tira proveito desse mecanismo. Nesses casos o compilador infere o tipo da variável em tempo de execução, esse tipo de mecanismos esta diretamente relacionado ao mecanismo de *Generics* do Java e Delphi Language. A validação de tipos passa a ser realizada em tempo de execucao.

#### Principais erros semânticos:

###### Escopo dos identificadores

O compilador deve garantir que variáveis e funções estejam declaradas em locais que podem ser acessados onde esses identificadores estão sendo utilizados.

Uma classe com funções declaradas como privadas e essas funções sendo utilizadas fora da classe.

```
class Foo {
   private $x;

   private function sum() {
        return $this->x = 1;
   }

}

Foo().sum()
```

Veja o exemplo da variável contador, ele deve ser declarada em cada escopo onde esta sendo utilizada.

```
def foo():
    cont = 0

    def bar():
        cont = 1

    def dop():
        cont = 3

    bar()
    dop()

    print(cont)
```

###### Compatibilidade de tipos: 

Verificar se os tipos de dados declarado nas variáveis e funções estão sendo utilizados e atribuidas corretamente, por exemplo: operações matemáticas devem ser realizadas com números, atribuições de valores para variávies.

Variável declaração como `int` está recebendo um valor `string`.

```
var i : int;
var s : string;
s = "john";
i := f;
```

No código acima qual é o comportamento em uma linguagem interpretada como Python? E no Pascal que é compilado?

> Em alguns casos pode ser efetuada a conversão de tipos, essa operacao é conhecida como *cast*, e pode ser feito de forma explicita no código ou pelo proprio compilador. 

###### Detectar unicidade de nomes de identificadores: 

Essa verificação é muito importante pois ele deve garantir que os identificadores sejam unicos não havendo na tabela de simbolos uma entrada para o mesmo identificador:

Isso implica em:

* diferentes identificadores podem ter o mesmo nome;
* o mesmo identificadores pode estar em um escopo diferente;
* sobrecarga de metodos deve gerar novas entradas na tabela de simbolos;

```
var i : int;
var i : int64;
```

```
var foo int

func foo() {
    fmt.Println(math.pi)
}
```

###### Detectar o uso correto de comandos de controle de fluxo:

Comandos como `continue` e `break` executam saltos na execucao de códigos, esse comandos deve ser utilizados em instruções que permitem os saltos.
