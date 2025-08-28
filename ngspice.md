# Dicas

## Condições iniciais

Basicamente define-se tensões ou correntes iniciais em certos nós ou componentes:

De forma global:

.ic V(2)=5

ou, de forma local no componente, normalmente capacitor ou indutor:

ex.: C1 3 2 10u ic=1, capacitor começa carregado em 1V
     L1 4 2 1m ic=50m, indutor começa energizado com 50mA

Para ignorar condições iniciais automáticas, quaisquer que seja na simulação use UIC, assim o ngspice usa apenas suas condições:

.tran 0.1ms 10ms uic

## Tipos de simulação

- Estática

    .op :: Calcula o ponto de operação DC com capacitores abertos e indutores em curto; útil para determinar tensões e correntes em circuitos simples sem transientes.

- Dinâmica

    .dc :: Calcula o comportamento do circuito para uma faixa de tensões e correntes DC

    .ac :: Calcula o comportamento do circuito para uma faixa de frequências AC; útil para filtros e amplificadores.

    .tran :: Calcula o comportamento do circuito ao longo do tempo a partir de fontes variáveis seja DC ou AC como senoidais, quadrada, triangular, pulsos.

    .step :: Calcula o comportamento do circuito para um conjunto de valores de componentes, como por exemplo valores de resistências.

Em simulações iterativas quando roda `ngspice arquivo.cir` use o bloco de comando:

.control
    run
    plot v(1) ; por exemplo...
.endc

## Números

Muliplicadores podem ser usados, se desejadar:

Atenção: Diferencia-se maiúscula de minúscula!

-9, nano: n
-6, micro: u
-3, mili: m

+3, quilo: K
+6, mega: Meg
+9: giga: G

Unidades após o número são desconsideradas mas podem ser usados por claridade

## Sintaxe

Primeira linha absoluta do arquivo deve ser o título do circuito

Todo arquivo deve ser finalizado com .end que corresponde à última linha absoluta

Comentário geral toma toda a linha e deve iniciar com * na primeira coluna

Comentário de fim de linha usa $ ou ;

Comandos podem se estender a linha subsequente desde que começe esta com +

Valores podem ser avaliados usando {}

Parâmetros (variáveis) devem ser declaradas antes do uso além de terem suas expressões avaliadas entre {} ou ''. ex.: .param meu_valor = '4 * 5' ou {4 * 5}. Não podem ser exibidas em print ou echo

Impressão: print V(1,0), V(1), I(V1)

Depuração: echo

## Dicas

Pesquisar sobre o comando `measure`

Exemplo de arquivo:

```spice
Divisor de tensão

.options noacct noinit nomod nopage
.param FonteGeral = 500mV
.param ResistorA = 1K
.param ResistorB = 1K

V1 1 0 dc {FonteGeral}
R1 1 2 {ResistorA}
R2 2 0 {ResistorB}
.control
    op
    echo Simulando tudo!
    print I(V1) V(1,2), V(2), @R1[i], @R2[temp], @R2[p]
    probe I(R2)
    echo Fim!
    quit
.endc
.end
```

