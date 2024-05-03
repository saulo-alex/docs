
# PHP moderno - notas da linguagem

Escrito por saulod2 em Maio de 2024 😊

## Histórico

Lançada por Rasmus Lerdorf em meados de 1995 (mas desenvolvida desde o final de 93) como um puxadinho de programas CGI escritos em C para ajudar a manter o controle de sua página web à época

O primeiro nome escolhido foi PHP/FI, que vêm de *Personal Home Page/Forms Interpreter*

Inicialmente não havia noção estrutura de uma linguagem, apenas com a versão 3.0 que foi introduzido um novo parser, o Zend Engine (por Zeev Suraki e Andi Gutmans), que a linguagem começou a tomar um formato mais sério e profissional além de se definir um nome oficial que vêm do acrônimo *PHP: Hipertext Preprocessor*

## Variáveis e Tipos

Possue uma tipagem fraca e dinâmica

```php
# tipagem fraca: embora $a seja string, a expressão espera valores numéricos, então o PHP tenta converter implicitamente para um número, resultando em 2
$a = '1';
$a = $a + 1;
# tipagem dinâmica: $a era string agora é int
echo $a;
```

Os tipos base são: `int`, `float`, `bool`, `array`, `resource`, `object` e `null`

> Dica: o tipo `resource` é basicamente uma referência para algum recurso externo como uma conexão com banco de dados
