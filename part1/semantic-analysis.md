Analise Semantica
======

### Introdução


Até o momento vimos a etapa de análise léxica - que quebra o programa fonte em tokens - e a analise sintática - que validas as regras de formação da linguagem de programação.

A semântica de um programa está diretamente ligada ao seu significado. A análise semântica é a terceira etapa do processo de compilação e é nesse memento que ocorre a validação de verificação de erros semânticos que até então não podem ser verificados pelas etapas anteriores.


Frase: Não é possível representar em uma gramática livre de contexto uma regra como "Todo identificador deve ser declarado antes de ser usado", isso resume muito bem o que deve ser feito na análise semântica.O analisador semântico utiliza a árvore sintática produzida na etapa de analise sintática e a tabela de símbolos produzida e incrementada desde o início do processo de compilação. Toda essa estrutura é utilizada gerar uma arvore de sintática e uma tabela de símbolos mais elaboradas e adaptada para as próximas etapas que compreendem as tarefas do back-end do processo de compilação e que tratam da geração do código em si.


Principais erros semânticos:


	Escopo dos identificadores: Deve garantir que variáveis e funções estejam declaradas em locais que podem ser acessados onde esses identificadores estão sendo utilizados.


Ex: Em uma classe funções declaradas como privadas estão sendo utilizadas fora da classe. 


	Compatibilidade de tipos: Verificar se os tipos de dados declarados a variáveis e funções estão sendo utilizados e atabuados corretamente, operações com expressões matemáticas estão o gerando saídas corretas.


Ex: variável declaração como Numérica está recebendo um valor Literal.


	O mecanismo de passagem por parâmetro: Também é verificado através dessas ações semânticas a verificação de tipos dos paramentos e quantidade de parâmetros.  


	Detectar unicidade de nomes de identificadores: Por exemplo variáveis com o mesmo nome no mesmo escopo não deve ser permitido.


	Detectar o uso correto de comandos de controle de fluxo: como exemplo temos os comandos continue e break.





Lembrete: A análise semântica verifica erros semânticos no programa fonte e captura informações para as fases subsequentes que tratam das gerações do código objeto. Utiliza a estrutura hierárquica criada na fase de análise sintática a fim de identificar operadores e operações e enunciados de expressões. Um importante componente da analise semântica é a verificação de tipos, nela o compilador verifica se cada operador recebe os operandos permitidos e especificados na linguagem fonte.


Um exemplo que ilustra muito bem essa etapa de validação de tipos é a atribuição de objetos de tipos ou classe diferentes.

Curiosidade: Em alguns casos, o compilador realiza a conversão automática de um tipo para outro que seja adequado à aplicação do operador. Por exemplo a expressão 

C := 2 + ‘2’;


Veja o exemplo de um código em Object Pascal:


function TForm1.Soma (A, B : Integer) : Integer;
var 
  C : Integer;
begin
  C := A + B;
  Result := C;
end;


No exemplo acima o analisador semântico de ter uma series e preocupações para validar o significado de cada regra de produção.


Vamos utilizar como exemplo a regra de produção de atribuição 

C := A + B;

	O identificador C foi declarado?


	O identificador C é uma variável?


	Qual o escopo da variável C?


	Qual é o tipo da variável C?


	O tipo da variável C é compatível com os demais identificadores, operadores?



Tradução dirigida por sintaxe

Esse processo é implementado associando-se a cada execução de uma regra da gramatical (execução do analisador sintático) uma ação semântica.  Essas ações semânticas são frequentemente implementadas a chamadas de rotinas semânticas, e podem ser responsáveis por efetuar a análise semântica, geração de código, armazenamento de informações na tabela de símbolos.


