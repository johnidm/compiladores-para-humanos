Otimizacoes de Cõdigo
======

O objetivo nessa etapa é gerar um codifo com mais qualidade e menor para isso o compilador analisa o coridgo e reescreve para um formato mais efeiciente. 

A otimizacao de codigo esta relacinada a dois apspecitso, reducao no consumo de memoria e velocidade na execucao, esse aspectos sao por natureza conflitantes pois ecomonia em espoaco de momoria pode por vezes tornar a execucao do programa mais lenta e vice vessa.

Outro apecto inportante e que essa tarefa consome tempo de compialcao muitas o que poor vezes pode tornar inviavel o custo beneficio de uma orimizacao.

Essa tarefa pode ser diviida em duas etapas:

* Otimizacao de codigo intermediario
* Otimizacao de código objeto

#### Otimizacao de codigo intermediario

Nesse momento ocorre a elimincaco de atribuicoes redundantes, eliminar temporario, eliminiar expressoes, trocar isntrucoes de lugar

Essa otimizacao é independete de maquina

#### Otimizacao de código objeto

Ao ideinticidar as instrucoes da maquina é possivel nessa etapa fazer troca de instrucies por isntuçoes mais rapidas e mais adquadas o uso.

Esse otimizxacao e dependente da arquitetura da maquina e pode apresnetar resultados diferentes para cada aquituteura

O nosso foco nesse capitulo a a otimicacao independente de maquina

Local Optimization 

Global Optimization
