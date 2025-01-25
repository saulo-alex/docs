
# PHP moderno - notas da linguagem

Escrito por saulod2 em Maio de 2024 😊

## Histórico

Lançada por Rasmus Lerdorf em meados de 1995 (mas desenvolvida desde o final de 93) como um puxadinho de programas CGI escritos em C para ajudar a manter o controle de sua homepage à época

O primeiro nome escolhido foi PHP/FI, que vêm de *Personal Home Page/Forms Interpreter*

Inicialmente não havia noção estrutura de uma linguagem, apenas com a versão 3.0 que foi introduzido um novo parser, o Zend Engine (por Zeev Suraki e Andi Gutmans), que daí a linguagem começou a tomar um formato mais sério e profissional além de se definir um nome oficial que vêm do acrônimo *PHP: Hipertext Preprocessor*

## Variáveis e Tipos

Possue uma tipagem fraca e dinâmica

```php
# tipagem fraca: embora $a seja string, a expressão de soma espera valores numéricos, então o PHP tenta converter implicitamente para um número, resultando em 2
$a = '1';
$a = $a + 1;
# tipagem dinâmica: $a era string agora é int
echo get_debug_type($a);
```

Os tipos base são: `int`, `float`, `bool`, `array`, `string`, `resource`, `object`, `null` e `callable`

Coerções explícitas são possíveis:

```php
$a = '1';
echo get_debug_type((int) $a);
echo get_debug_type((bool) $a);
```

> Dica: É possível utilizar os nomes não-canônicos como `integer`, `boolean`, `binary`, `double`, `real` mas não são recomendadas!

> Dica: Também é possível realizar casting explícito de uma expressão ou variável com `settype(exp, type)`

Além dos tipos bases, o PHP compõe uma ecossistema de tipos rica:

`never` é utilizado em retorno de funções que nunca terminam

`void` também é utilizado em retornos porém de funções que não retornam

`self`, `parent` e `static` são usados dentro de classes e referem a instâncias da classe, instâncias da superclasse e o último apenas em retorno de métodos estáticos

`false` e `true` são tipos-valor que representam que o próprio tipo em si

Interfaces são definidas com `interface`

Classes são definidas com `class`

Enumerações são definidas com `enum`

Funções e objetos-função que podem ser invocadas por `call_user_func()` são declaradas como `callable`

Tipos união podem ser definidos com o `|`, como em `null|string`

Tipos intersecção podem ser definidos com o `&`, como em `int&float`

> Dica: o tipo `mixed` é definido como `object|resource|array|string|float|int|bool|null` a partir do PHP 8.0

### Arrays

Pode ser tanto indexado como associativo, neste último suportando apenas chaves `string` ou `int`

Desestruturação de array usando `[]` é feita posição por posição, não sendo possível capturar múltiplas posições (como um novo array):

```php
# $ar tem 5 posições
$ar = ['a', 'b', 'c', 'd', 'e'];

# captura as 3 primeiras descartando o resto
[$a, $b, $c] = $ar;

# captura as duas primeiras e descarta a terceira e quarta
[$a, $b, , , $c] = $ar;

# $ar agora é associativo
$ar = ['apple' => 'macosx', 'microsoft' => 'windows', 'GNU' => 'Linux'];

# é possível desestruturar também e em qualquer ordem de seleção!
['windows' => $win, 'apple' => $apple, 'GNU' => $gnu] = $ar;

# swap se torna simples!
[$a, $b] = [$b, $a];
```

Para remover posições de um array faça como qualquer outra variável: `unset($array[index])`

É possível também desempacotar arrays (*array unpacking*) em outros arrays usando `...`:

```php
$ar1 = [1, 2, 3, 4, 5];
$ar2 = [10, 100];

function f() { return ['ko', 'ok']; }

# $ar3 recebe o unpacking de $ar1
$ar3 = [...$ar1, 6, 7];

# múltiplos unpackings (incluindo de funções)
$ar4 = [0, ...$ar3, ...$ar2, ...f()];
```

Como quase todos os tipos, arrays são atribuídos sempre por cópia, se quiser que seja por referência use `&`!

```php
$ar1 = [1, 2];

# cópia dos elementos
$ar2 = $ar1;

# referência, não há cópia; $ar3 se torna um apelido para $ar1
$ar3 = &$ar1;
$ar3[0] = 'One';

# ['One', 2];
var_dump($ar1);
```

### Strings

Além do básico com `'Hello World'` (sem interpolação) e `"Hello World"` (com interpolação), há também o `heredoc` e o `nowdoc`:

```php
# heredoc interpola e não utiliza aspas simples no delimitador mas opcionalmente pode usar aspas duplas!
$lang = 'PHP';
$body = <<<FIM
<body>
    <h1>Uma página inútil</h1>
    <p>Hello World from $lang</p>
</body>
FIM;

# nowdoc utiliza aspas no delimitador e não pode interpolar
$body = <<<'FIM'
<body>
    <h1>Uma página inútil</h1>
    <p>Hello World from HTML</p>
</body>
FIM;
```

Uma string em PHP é sempre tratada como sequência de bytes simples, isso significa que não há suporte nativo ao Unicode

Uma string na verdade é um array de bytes com um inteiro associado indicando seu tamanho

Uma função quando é *binary safe* indica que ela pode tratar strings como uma cadeia crua de bytes não fazendo presupostos do conteúdo
