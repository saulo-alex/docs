## Amplificadores

Um componente eletrônico pode ser:

- Passivo: quando não consegue controlar sua corrente através de um sinal elétrico externo de controle: resistores, capacitores, indutores, transformadores, diodos.

- Ativo: quando consegue controlar sua corrente através de um sinal elétrico externo de controle, dessa forma permitindo amplificação: transistores, válvulas, tiristores.

Um equipamento para ser dito eletrônico precisa de ao menos um componente ativo.

Amplificação é o ato de remodelar um sinal de entrada afim de obter uma amplitude maior na saída.

Sabe-se que energia não se cria nem se destroi (pela lei da conservação de energia), dessa forma para o amplificador conseguir amplitudes maiores na saída é necessário possuir uma fonte externa de energia, no caso na malha da saída.

O ganho de potência (ou perda) de um amplificador é sempre não-unitário: Ap = Ps / Pe. (Onde: Ps e Pe são respectivamente, potências de saída e de entrada)

O bel é uma unidade logarítimica que descreve o ganho ou a perda (de potência, a priori) de maneira mais natural, já que sinais sonoros na audição humana são perceptíveis de forma logarítimica.

Por curiosidade, o bel foi inventado pelo escoçês Grahan Bell na década de 1910 para descrever a atenuação do sinal telefônico em redes de telefonia de longas distâncias.

O ganho ou perda de potência de um sinal de 1 bel é dado por: Ap_bel = log (Ps / Pe)

Se o sinal amplificou ou atenuou em potência em 10 vezes, então ele é amplificado em +1B ou atenuado em -1B.

De uma forma mais natural é comum usar o decibel, que equivale a décima parte do bel ou 1 db = B/10 (10dB = 1B).

Assim, se o sinal amplificar em potência 10 vezes, equivale a dizer que aumentou em +10dB.

Para cálculos de ganhos de tensão ou de corrente deve-se notar que:

P = V^2 / R

ou

P = R x I^2

Assim, no cálculo em valor absoluto, usa-se:

Av = ( Vs / Ve ) ^ 2

ou

Ai = ( Is / Ie ) ^ 2

Rescrevendo em bel, deve-se considerar esse quadrado:

Av_bel = log ( V_s / V_e ) ^ 2

ou

Ai_bel = log ( I_s / I_e ) ^ 2

Na aritmética dos logaritmos pode-se abaixar o quadrado multiplicando:

Av_bel = 2 * log ( V_s / V_e )

ou

Ai_bel = 2 * log ( I_s / I_e )

E, em decibéis:

Av_dB = 10 * 2 * log ( V_s / V_e ) = 20 * log ( V_s / V_e )

ou

Ai_dB = 10 * 2 * log ( I_s / I_e ) = 20 * log ( I_s / I_e )

Outra aplicação nesses cálculos é do valor em bel ou dB, encontrar o valor absoluto, em potência, tensão ou corrente:

Ap = 10 ^ ( Ap_bel / 10 )

e

Av = 10 ^ ( Av_bel / 20 )

e

Ai = 10 ^ ( Ai_bel / 20 )

Por curiosidade, um ganho unitário é representado em bel (também em dB) por 0 e não 1, e agora é óbvio isso.

Quando dispostos em cascata etapas amplificadoras ou atenuadoras, há algumas considerações:

Se os valores são absolutos o resultado é o produto de todos eles: A1 = 10, A2 = 5 então A = A1 * A2 = 10 * 5 = 50

Se os valores são logarítmicos o resultado é a soma deles. Se A1 = 20dB, A2 = 5dB então A = 20dB + 5dB = 25dB.
