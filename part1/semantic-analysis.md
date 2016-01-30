Analise Semantica
======

Até o momento vimos as etapas de análise léxica, que quebra o programa fonte em tokens e a analise sintática, que valida as regras a sintaxe da linguagem de programação.

Não é possível representar com expressoes regulares ou com uma gramática livre de contexto regras como: todo identificador deve ser declarado antes de ser usado. Muitas verificacoes devem ser realizadas com metainformacoes e com elementos que estao presentes em varios pontos do código fonte, distantes uns dos outros e, por isso  analisador semântico utiliza a árvore sintática e a tabela de símbolos para fazer para resolver esse problema.

A analise semântica é responsavel por verificar aspectos relacionados ao significado das intrucoes, essa é a terceira etapa do processo de compilação e nesse memento ocorre a verificação de uma serie regras que não podem ser veorificadas nas etapas anteriores.

> É impotante ressaltar que muitos dos erros semanticos tem origem de regras dependentes da linguagem de programacao.

As valida;óes que nao poodem ser executadas pelas etapas anteriores devem ser executadas durante pelo a analise semantico a fim de garantir que o progrma estaja coerente para que o mesmo possa ser convertido para liguagem de maquina.

A analise sementica percorre a arvore sintatica relaionado os identificadores com seus dependencias, de acordo com a estrutura hierarqui e possivel validar os atributos semanticos.

#### Inferencia de tipos

Os sistemas de tipos de dados podem ser dividisos em dois tipos, sistemas dinamicos e sistemas estaticos, muitas das lingaugem utilizam o sistem estatico, isso é predominando em lingaugem compiladas, pois essa ifnormacao é muito utilizada duram a compialcao e simplifica o trabalho do compiladao.

Alguasm lingaugem utilizam um mecaniusmo muito interessantes cahamdo inferencia de tipos que permieu que uma variavel possa assumir varios tipos durante o seu ciclo de vida, isso permite que uma variavel possa assumir varios valores. Linguagem de proramacao como o **Haskel** tira prroveito desse mecanismo. Nesse casos o compilador infere o tipo de dados. Esse tipo de mecanismos esta doretamente relacionado ao mecanismo de *Generics* do Java e Delphi Language.  A validar de tipos passa a ser realizada em tempo de execucao. 

#### Principais erros semânticos:

###### Escopo dos identificadores

O compilador deve garantir que variáveis e funções estejam declaradas em locais que podem ser acessados onde esses identificadores estão sendo utilizados.

Uma classe funções declaradas como privadas estão sendo utilizadas fora da classe.

```
class Foo {
   private $x;

   provate function sum() {
        return $this->x = 1;
   }

   public function sum_sum() {
      return $this->sum();
   }
}
```

Veja o exemplo da vaiavel contador, ele deve ser declarada em cada escopo onde esta sendo utilizada.

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

Verificar se os tipos de dados declarados a variáveis e funções estão sendo utilizados e atribuidas corretamente, operações com expressões matemáticas estão o gerando saídas corretas.

Variável declaração como `int` está recebendo um valor `string`.

```
var i : int;
var s : string;
s = "john";
i := f;
```

No código acima qual é o comportamten em uma lingaugem interpretada como Python? E no Pascal que é compilado?

> Em alguns casos pode ser efetuado a conversão de tipos, essa operacao é conhecida como **cast**, isso pode ser feito de forma explicita no código ou pelo proprio compilador. 

###### Detectar unicidade de nomes de identificadores: 

Essa verificao é muito impostante pois ele deve garantir que os identificadores sejam unicos não havendo na tabela de simbolos de uma entrada para o mesmo identificador:

Isso implica em:

* diferntes identificadores podem ter o mesmo nome;
* o mesmo identifficadores pode estar em um escopo diferntes;
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

Comandos como `continue` e `break` executam saltos na execucao de códigos, esse comandos deve ser utilizados em instrucoes que permitem os saltos

Essa etapa também captura informações sobre o programa fonte para que as fases subsequentes gerar o código objeto, um importante componente da analise semântica é a verificação de tipos, nela o compilador verifica se cada operador recebe os operandos permitidos e especificados na linguagem fonte.

Um exemplo que ilustra muito bem essa etapa de validação de tipos é a atribuição de objetos de tipos ou classe diferentes. Em alguns casos, o compilador realiza a conversão automática de um tipo para outro que seja adequado à aplicação do operador. Por exemplo a expressão 

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

No exemplo acima o analisador semântico de ter uma series e preocupações para validar o significado de cada regra de produção.

Vamos utilizar como exemplo a regra de atribuição: 

```
i := a + b;
```

* O identificador `i` foi declarado?
* O identificador `i` é uma variável?
* Qual o escopo da variável `i`?
* Qual é o tipo da variável `i`?
* O tipo da variável `i` é compatível com os demais identificadores, operadores?

Os tipo de dados são muito importantes nessa etapa, eles sao notacoes que as linguagem de programacao utilizam para represetnar um conjunto de valores. As classes uma notacao de tipo, junto com Interior, strings e reais. Com base nos tipos o analisador semetnaodo pode definir quais valores sao possiveis, isso é conhecido com *type checking*.

As linguagens de progrmacao podem tem duas formas de definir os tipos

* Tipagem estatica: Linguagem como C, Java, Pascal obrigam o programador a definir os tipos das variaveis e retorno de funcoes, o compialdor pode fazer varias checagens em tempo de compilação.
* Tipagem dinamica: Varaiveis e retrno de funcioes nao possuem a declaracao de tipos, linguagem como Python , Perl, PHP são exemplos.





