# Bash: linguagem, comandos e usos

Escrito por saulod2 em Abril de 2024 😊

## basename

Mostra a última parte (o arquivo) de um caminho completo

### Exemplos:

```bash
# qml.hpp
basename /usr/include/qt/qml.hpp
```

## dirname

Mostra o que sobra de um caminho completo retirado a última parte (o caminho para)

### Exemplos:

```bash
# /usr/include/qt
dirname /usr/include/qt/qml.hpp
```

## cut

Exibe campos de um texto separados por um delimitador

### Opções úteis:

- `-f` informa o número ou faixa do campo
- `-f2` campo 2
- `-f2-` todos os campos do 2 até o último
- `-f-7` todos os campos do primeiro até o 7
- `-f3-7` todos os campos entre 3 e 7
- `--output-delimiter=' '` informa que a faixa extraída deve ter o delimitador alterado para ' '

### Exemplos:

```bash
# c
echo 'a;b;c;d;e' | cut -d';' -f3
# a;b;c
echo 'a;b;c;d;e' | cut -d';' -f-3
# a b c
echo 'a;b;c;d;e' | cut -d';' -f-3 --output-delimiter=' '
# b;c
echo 'a;b;c;d;e' | cut -d';' -f2-4
```

## head

Exibe as primeiras linhas de um arquivo (por padrão 10)

Também é possível exibir a nível de bytes (usando `-c`)

### Opções úteis:

**Dica rápida:** Com o `-` em números descarta os últimos.

- `-n1` apenas a primeira linha
- `-n-1` todas exceto a última linha
- `-n20` as primeiras 20 linhas
- `-n-20` todas exceto as últimas 20 linhas
- `-c2` os dois primeiros bytes
- `-c-2` todos exceto os dois últimos bytes

## tail

Exibe as últimas linhas de um arquivo (por padrão 10)

Também é possível exibir a nível de bytes (usando a opção `-c`)

É muito comum para visualizar em tempo real o log de programas com `tail -f`

### Opções úteis:

**Dica rápida:** Com o `+` em números descarta as primeiras `N-1` linhas.

- `-n1` apenas a última linha
- `-n+2` todas exceto a primeira linha
- `-n20` as últimas 20 linhas
- `-n+21` todas exceto as primeiras 20 linhas
- `-c2` os dois últimos bytes
- `-c+3` todos exceto os dois primeiros bytes
- `-f` siga qualquer modificação

**Dica rápida:** Pense nos símbolos `+` e `-` como os *primeiros* e *últimos*, respectivamente

## seq

Gera uma sequência numérica arbitrária

**Dica rápida:** Usando expansão de chaves `{min..max[..step]}` é possível obter o mesmo resultado com diferencial de que este gera sequências de caracteres: `echo {1..10}`, `echo {a..z}`, `echo Z{A..Z..4}`.

### Opções úteis:

- `-s` define um separador entre os números, padrão é `\n`

### Exemplos:

```bash
# 1 até 10 separados por \n
seq 10
# 1 até 10 separados por espaços
seq -s' ' 10
# 100 até 150
seq 100 150
# 0 até 1 incrementado em 0.05
seq 0 .05 1
# 1 até -5
seq 1 -1 -5
# -2 até 2 incrementado em .1 separados por espaços
seq -s' ' -2 .1 2
```

## sort

Ordena um conjunto de linhas usando diversos critérios de comparação (numérico, alfabético, nome de meses, randômico). O padrão é alfabético e crescente.

### Opções úteis:

- `-f` desativa a diferenciação entre maiúsculas e minúsculas
- `-n` comparação numérica
- `-M` comparação baseada nos prefixos de meses (JAN, FEV, MAR, ...)
- `-g` comparação numérica geral (mais lenta e pode ponto flutuante)
- `-s` comparação estável
- `-k` define a chave (ou as chaves, separadas por vírgulas) para ordenação de linhas de múltiplos campos na ordem declarada
- `-t` define o separador para ordenação com base nos campos

**Dica rápida:** Comparação sempre utiliza o locale para nomes de meses e pontos decimais.

### Exemplos:

```bash
# a b c ... z
echo {z..a} | tr ' ' '\n' | sort
# 0 1 2 ... 10
seq 10 -1 0 | sort -n
# 10 9 8 ... 1
seq 10 | sort -nr
# ordena numericamente usando o primeiro campo (-k1) como chave
# 0 jorge\n2 daniel\n3 roberta etc.
cat <<FIM | sort -k1 -n
5 saulo
0 jorge
2 daniel
3 roberta
10 roberto
5 sílvio
FIM
# ordena alfabeticamente usando o segundo campo (-k2) como chave e define o separador de campos o ponto-e-vírgula
cat <<FIM | sort -k2 -t';'
5;saulo alex
0;jorge de la penha
9;jorge de sá
2;daniel matias
3;roberta santana
10;roberto santos
5;sílvio fabiano
FIM
# ordena pelo mês (usando primeiro campo como chave) de forma decrescente
cat <<FIM | sort -k1 -r -M
jan saulo alex
fev jorge de la penha
mar jorge de sá
abr daniel matias
abr roberta santana
mai roberto santos
dez sílvio fabiano
FIM
```

## uniq

Seleciona os valores únicos de um conjunto de linhas

**Dica rápida:** O `uniq` só consegue determinar linhas iguais quando são adjacentes, então é necessário ordenar antes!

### Opções úteis:

- `-c` conta ocorrências de cada valor único
- `-i` não diferencia maiúsculas de minúsculas

### Exemplos:

```bash
# aa\nab\naz\ncc\ncd\nce
echo ab cd cc ab ab de az aa ab de ab de | tr ' ' '\n' | sort | uniq
# conta as ocorrências unicas e não diferencia minúscula de maiúscula
echo ab cd Cc ab Ab de AZ aa ab DE ab de | tr ' ' '\n' | sort | uniq -ci
# 1\n5\n8\n10
echo {1,5,10} {1,5,10} {1,5,10} 8 | tr ' ' '\n' | sort -n | uniq
```

## shuf

Seleciona linhas (ou termos) de forma randômica da entrada

### Opções úteis:

- `-n` seleciona no máximo (linha ou palavras, se usado com `-e`)
- `-e` a entrada será uma lista de palavras separados por espaço

**Dica rápida:** o termo `palavra` não restringe apenas a palavras de fato, pode ser números, o que importa é que estejam separados por espaço

### Exemplos:

```bash
# cinco números aleatórios entre 1 e 10
shuf -e -n5 <<< echo {1..10}
# selecione duas letras do alfabeto
shuf -n2 -e <<< : {a..z}
# cinco números aleatórios entre 0 e 1
seq 0 .05 1 | shuf -n5
```

## date

Exibe a data e hora atuais do sistema em formato local e padrão ou personalizado

### Opções úteis:

- `+''` define um formato. Note as aspas simples que não pertencem ao comando mas evitam o shell interpretar termos do formato (que usam o prefixo `%`)
- `-d''` transforma do formato *unix timestap* em hora e data locais. É necessário prefixar o timestamp com `@` como em `@129034`.
- `-R` exibe no formato RFC 5322 (utilizado em e-mail)
- `-u` não utiliza o timezone local mas sim o UTC

### Exemplos:

```bash
# Exemplo: Hoje é quarta
date +'Hoje é %A!'
# Exemplo: Hoje é qua, 03 de abril
date +'Hoje é %a, %d de %B!'
# Exemplo: Que horas são? 16:18:55
date +'Que horas são? %X (e nosso fuso horário é %:z)'
# Exemplo: Que dia é hoje? 03/04/2024
date +'Que dia é hoje? %x'
# dd/mm/YYYY HH:MM:SS
date +'%d/%m/%Y %H:%M:%S'
# transformando timestamp para local
date -d'@3600'
```

## join

Junta linhas de dois arquivos nas quais existem campos em comum. Por padrão utiliza a primeira coluna como campo de junção e o separador espaço.

### Opções úteis:

- `-t` define um separador alternativo
- `-j` define a coluna de comparação, ou seja o campo de junção (iguais nos dois arquivos)
- `-o` define um formato de saída alternativo
- `-1` define a coluna de comparação no arquivo 1
- `-2` define a coluna de comparação no arquivo 2

### Exemplos:

Para ver usos do `join` suponha que haja dois arquivos, um contendo em cada linha um ID e comandos prediletos desse usuário e outro arquivo, contendo o mesmo ID, não necessariamente em ordem com o primeiro, e o sistema operacional que ele usa.

```
file1.txt
  10 cd cp who join
  20 ls make cc
  15 ping ifconfig iwconfig
  17 ping firefox

file2.txt
  10 Arch
  17 Ubuntu
  15 Ubuntu
  20 Debian

$ join file1.txt file2.txt
  10 cd cp who join Arch
  20 ls make cc Debian
  15 ping ifconfig iwconfig Ubuntu
  17 ping firefox Ubuntu
```

## paste

Mescla dois arquivos, linha por linha, em um único sem haver nenhuma comparação.

### Opções úteis:

- `-d` define o delimitador

### Exemplos:

```bash
# o comando abaixo terá a saída:
# 1   1
# 2   2
# 3   3
# 4   4
# 5   5
# 6
# 7
# 8
# 9
# 10
paste <(seq 10) <(seq 5)
# mesmo resultado, porém usando - como separador na saída
paste -d'-' <(seq 10) <(seq 5)
```

## expr

Avalia expressões numéricas ou de cadeia (string)

### Expressões:

- `A | B`, `A & B`
- `A < B`, `A <= B`, `A > B`, `A >= B`
- `A = B`, `A != B`
- `A + B`, `A - B`, `A * B`, `A / B`
- `A % B`
- `str : regxp` ou `match str regxp` verifica se `str` casa com `regxp`
- `substr str pos len` `pos` inicia em `1`
- `index str substr`
- `length str`
- `()`

**Dicas rápidas:**

- Valores numéricos são sempre inteiros
- Alguns operadores como o `*` precisam ser escapados como em `expr 2 \* 3`

## comm

É um comando arcaico que serve para realizar comparações entre dois arquivos ordenados linha a linha, informando em três colunas o resultado, sendo a 1ª os valores únicos do primeiro arquivo, sendo a 2ª os valores únicos do segundo arquivo e a terceira os valores comuns em ambos.

**Dica rápida:** Na prática, o comando serve para testar a igualdade entre dois arquivos, e apenas isso. Use assim: `comm file1 file2 &>/dev/null; echo $?`.

## test

Testa uma porção de coisas em arquivos e também compara cadeias (strings)

Há duas sintaxes: `test expr` ou `[ expr ]`.

### Expressões:

- `( expr )`
- `! expr` NOT
- `expr1 -a expr2` AND
- `expr1 -o expr2` OR
- `-n str` ou `str` `str` tem tamanho maior que zero
- `-z str` `str` tem tamanho zero
- `str1 = str2`
- `str1 != str2`
- `num1 -eq num2`
- `num1 -ge num2`
- `num1 -gt num2`
- `num1 -le num2`
- `num1 -lt num2`
- `num1 -ne num2`
- `file1 -ef file2` arquivos estão no mesmo dispositivo e possuem o mesmo *inode*
- `file1 -nt file2` `file1` é mais novo que file2
- `file1 -ot file2` `file1` é mais velho que file2
- `-b file` `file` existe e é de bloco
- `-c file` `file` existe e é de caractere
- `-d file` `file` existe e é diretório
- `-e file` `file` existe (lembre-se: diretórios também são arquivos)
- `-f file` `file` existe e é arquivo comum
- `-g file` `file` existe e tem o bit *set-group-ID* setado
- `-G file` `file` existe e é mantido pelo GID efetivo
- `-h file` ou `-L file` `file` existe e é um arquivo simbólico
- `-k file` `file` existe e está setado o *sticky bit*
- `-N file` `file` existe e foi modificado desde a sua última leitura
- `-O file` `file` existe e é mantido pelo UID efetivo
- `-p file` `file` existe e é um *named pipe*
- `-r file` `file` existe e o usuário tem permissão de leitura
- `-s file` `file` existe e tem tamanho maior que zero
- `-S file` `file` existe e é um *socket*
- `-t FD` FD (file descriptor) está aberto no terminal
- `-u file` `file` existe e tem o bit *set-user-ID* setado
- `-w file` `file` existe e o usuário tem permissão de escrita
- `-x file` `file` existe e o usuário tem permissão de execução

**Dica rápida:** Lembre-se que o shell considera `0` (`true`) e `1` (ou qualquer outra coisa, `false`) e também não precisa prefixar variáveis com `$`.

### Exemplos:

```bash
# 0
test saulo = saulo; echo $?
# 1
test 2 -ne 2; echo $?
# Existe e você pode escrever nele! 
[ -w bash_commands.md ] && echo Existe e você pode escrever nele! 
```

## cat

Concatena um ou mais arquivos e mostra na saída

### Opções úteis:

- `-n` mostra o número da linha
- `-E` mostra $ no fim de cada linha
- `-T` mostra tabulações como ^I

### Exemplos:

```bash
# mostra arq1.txt e depois arq2.txt
cat arq1.txt arq2.txt
# lista de 1 a 10 e em seguida e 1 a 5
cat <(seq 10) <(seq 5)
# cat se não informado arquivo nos argumentos pega a stdin
cat <<FIM
Oi eu sou
uma string
FIM
```

## bc

Uma calculadora de números de ponto flutuante arbitrária

**Dicas rápidas:**

- É também uma linguagem de programação
- Funciona também no modo iterativo
- Comandos são separados por ponto-e-vírgula `;`
- Existem variáveis, que sempre são minúsculas
- Literais são sempre maiúsculos
- Há cinco variáveis importantes:
    - `scale` define a quantidade de digitos após a vírgula decimal
    - `last` define o último resultado
    - `length` define o tamanho máximo suportado sem contar a vírgula
    - `ibase` define a base numérica de entrada
    - `obase` define a base numérica de saída
- Nas bases numéricas `ibase` e `obase` os valores literais são definidos usando apenas um dígito de `{0-9,A-Z}`, assim: `ibase=G` significa base 16 (ou hexadecimal), `obase=A` significa base 10 enquanto `ibase=2` significa 2 mesmo, ou binário.
- A maior base numérica permitida é 36
- A menor é 2
- Os operadores seguem os da linguagem C, com exceção do `A ^ B` que é o de potência.
- **Cuidado:** `a = 3 < 5` no bc é interpretado como (1) `a = 3`; (2) `a = (3 < 5)`, ou seja primeiramente ele atribui!
- Há arrays `var[index]` porém não se pode inicializar múltiplas posições na declaração: `a[0] = 10; a[1] = 20; a[2] = 30;`
- Há diversas estruturas como: `{ }`, `if (expr) stmt1 [else stmt2]`, `while (expr) stmt`, `for ([expr1]; [expr2]; [expr3]) stmt`, `break`, `continue`, `return`, `define funcname (params) { NEWLINE AUTO_LIST stmt }` para funções.

### Funções *built-ins*:

- `sqrt(X)`
- `read()` lê (um número) da stdin

### Funções da biblioteca de matemática (`bc -l`):

- `s(X)` seno
- `c(X)` cosseno
- `a(X)` arctg
- `l(X)` log
- `e(X)` e^X

### Exemplos:

```bash
# binário para hexa
echo 'ibase=2;obase=G;1111+10' | bc
# decimal para octal
echo 'ibase=A;obase=8;1024*3' | bc
# raíz quadrada com precisão de 5 casas
echo 'scale=5;sqrt(2)' | bc
# raíz quadrada de 1 até 10 incrementado em 0.05 com precisão de 5 casas
# LC_NUMERIC foi necessário para mudar o formato do número (pt_BR usa vírgula)
for n in $(LC_NUMERIC=en_US seq 1 .05 10); do echo "scale=5;sqrt($n)" | bc; done
# todos os números binários de 0 a 65535
bc <<< 'a=0;ibase=G;obase=2;while (a <= FFFF) {a;a+=1}'
```

## tr

Transforma a entrada substituindo caracteres de um alfabeto por um outro

### Opções úteis:

- `-c` utiliza ao invés, o complementar do primeiro alfabeto
- `-d` deleta caracteres do primeiro alfabeto
- `-s` reduz caracteres repetidos (em sequência) do alfabeto para um único
- `-t` trunca o primeiro alfabeto para o tamanho do segundo

**Dicas rápidas:**

- O alfabeto pode ser informado como `aeiou`, por exemplo.
- Suporta intervalos como: `A-Z`, `a-z`, `0-9`, `a-f`
- Suporta listas: `[12345abc]`, `[^A-Z]`
- Suporta classes de caracteres: `[[:alnum:]]`, `[[:digit:]]`, `[[:alpha:]]`, `[[:blank:]]` e outras mais
- Suporta repetição de caractere como no exemplo: `[A*5]`, 5 caracteres A ou `[A*]`, repetição de A tantas vezes for necessário para casar com o tamanho do outro alfabeto

### Exemplos:

```bash
# deleta todas as linhas vazias
tr -s '\n' < main.c
# troca da stdin as letras maiúsculas por minúsculas
tr A-Z a-z
# troca os caracteres do alfabeto aeiou por 12345, nessa ordem
echo era uma vez três porquinhos | tr aeiou 12345
```

## wc

Informa o número de bytes, caracteres, palavras, linhas ou a largura máxima do arquivo

### Opções úteis:

- `-c` conta bytes
- `-m` conta caracteres
- `-l` conta linhas
- `-w` conta palavras
- `-L` diz a largura máxima

## echo

Ecoa na tela conteúdo de cadeias e gera uma nova linha

### Opções úteis:

- `-e` permite usar caracteres de controle como \n ou \x
- `-n` não gera uma nova linha

## cd

Muda para algum diretório

## pwd

Mostra o diretório atual

## od

Realiza um dump dos bytes do arquivo em várias bases, que por padrão é octal

### Opções úteis:

- `-A` define a base numérica dos endereços: `d` (decimal), `x` (hexadecimal), `o` (octal) ou `n` (não exibe ela!)
- `-t` define a representação de cada byte: `a` (caracteres como apresentados no ASCII), `b` (octal), `c` (como `a`, `\n`, `\t`, `ã`), `f` (números reais), `x` (hexadecimal)

> É possível definir a largura do byte manualmente usando um sufixo no valor de `-t`, como `-tx1` (hexa de 1 byte)

> Qualquer tipo com sufixo, último de todos, `z` permite que seja exibido na coluna mais a direita a representação ASCII da linha, como em `-tx1z`

- `-j` define o offset inicial, ou seja, quantos bytes serão deslocados antes de exibir o *dump*
- `-N` define a quantidade máxima de bytes a serem lidos

**Dica rápida:** É possível utilizar sufixos nos números do *offset* ou do tamanho como `b` (bloco), `KB` (1000), `K` (1024), `MB` (1000\*1000), `M` (1024\*1024) etc.

### Exemplos:

```bash
# pega 10 bytes e exibe com endereços hexadecimais e um byte hexadecimal por coluna mostrando o caractere (na coluna mais a direita)
head -c10 /dev/random | od -Ax -tx1z
# gerador aleatório de letra minúscula
printf "\x$(bc <<< "obase=G;97 + $(head -c1 /dev/random | od -An -td | tr -d ' ') % 27")\n"
```

## tee

Lê a stdin e ecoa na stdout e em arquivos ao mesmo tempo

### Opções úteis:

- `-a` anexa conteúdo no final

### Exemplos:

```bash
# leia a entrada padrão e cuspa na saída padrão assim como nos arquivos, zerando primeiro
tee file1.txt file2.txt
# leia a entrada padrão e cuspa na saída padrão assim como nos arquivos, anexando ao final
tee -a file1.txt
```

## tac

Faz o que cat faz, porém concatena os arquivos a partir da última linha até a primeira (reversamente)

### Exemplos:

```bash
tac | sort
```

## ls

Lista o conteúdo de diretórios

Por padrão a listagem é em ordem alfabética

### Opções úteis:

- `-l` formato de lista (detalhado)
- `-1` formato de lista (simples, só o nome)
- `-m` formato de vírgulas (útil em script)
- `-C` formato de colunas (padrão)
- `-a` mostra arquivos ocultos (incluíndo os diretórios `.` e `..`)
- `-R` lista o diretório corrente e todos seus subdiretórios
- `-I {pattern}` esconde arquivos que casam com pattern (usando curingas do shell)
- `-B` não mostra os arquivos de backups (sufixo ~)
- `-g` formata em lista e não mostra o dono
- `-o` formata em lista e não mostra grupo
- `-s` mostra o tamanho do arquivo
- `-i` mostra o inode do arquivo
- `--group-directories-first` mostra diretórios primeiro
- `-d` lista o diretório em si, não o conteúdo
- `-H` segue o link simbólico quando o é informado especificamente (por padrão não segue)
- `-r` ordem reversa
- `-c` ordena pelo *ctime*
- `-lc` ordena pelo nome e mostra *ctime*
- `-lct` ordena e mostra pelo *ctime*
- `-t` ordena pelo tempo (mais novos primeiro) que pode ser definido por `--time`, `-c` ou `-u`
- `-u` ordena pelo *atime*
- `-lu` ordena pelo nome e mostra o *atime*
- `-lut` ordena e mostra pelo *atime*
- `-S` ordena pelo tamanho (maiores primeiro)
- `-X` ordena pela extensão
- `--time={type}` especifica qual o momento de tempo utilizado no formato de lista detalhada: `atime` (ou `access`, `use`), `ctime` (ou `status`), `mtime` (ou `modification`), `birth` (ou `creation`)
- `--sort={type}` ordena por tamanho `size`, horário `time`, versão `version`, extensão `extension` ou não ordena `none`
- `--color=never|auto|always` habilita ou desabilita cores

> Em script é interessante ver o retorno `0` - sucesso, `1` - falha simples, `2` - falha séria.

### Coluna de permissões:

Na primeira coluna do formato detalhado, há informações de tipo de arquivo e suas permissões. Da esquerda para direita os próximos 10 caracteres tem o seguinte significado:

- *tipo do arquivo*: `d` diretório, `-` arquivo comum, `c` arquivo de caractere, `b` arquivo de bloco, `l` link simbólico, `p` pipe, `s` socket
- *permissões do **dono*** (os três próximos caracteres): `r` leitura, `w` escrita e `x` execução
- *permissões do **grupo*** (os três próximos caracteres): `r` leitura, `w` escrita e `x` execução
- *permissões dos **outros*** (os três próximos caracteres): `r` leitura, `w` escrita e `x` execução

> Se houver um `-` em algum caractere no segmento da permissão, então ela não é concebida!

> No terceiro caractere dos segmento das permissões, pode haver além de `x` (execução) ou `-` (sem persmissão de execução) os seguintes caracteres: `s` indica que o *set-user-id* ou *set-group-id* estão setados e há permissão de execução, `S` indica que o *set-user-id* ou *set-group-id* estão setados mas não há permissão de execução, `t` o *sticky-bit* (também conhecido como *deletion flag*) está setado e há permissão de execução, `T` o *sticky-bit* está setado mas não há permissão de execução.

> `atime`, `ctime` e `mtime` representam os três registros de tempo presentes em qualquer metadado de arquivo no Linux. Sendo `atime` o *access time* ou seja, representa o momento em que o arquivo foi acessado pela última vez. Já o `ctime` é o *change time* que representa o último momento em que algum metadado do arquivo foi modificado. Por último o `mtime` que é o *modified time* a qual indica o momento a qual o conteúdo do arquivo foi modificado pela última vez. Por padrão o `ls` utiliza o `mtime`. Para visualizar todos os momentos citados também é possível usar o comando `stat`.

**Dica rápida:** O *set-user-id* e o *set-group-id* permitem que usuários comuns possam ter as mesmas permissões de dono ou de grupo, ao acessar o arquivo; O *sticky-bit* se aplica apenas em diretórios, e impede que qualquer usuário que não o dono (do arquivo ou do diretório) e o root possam excluir ou renomear arquivos no diretório mesmo que tenham permissões para isso.

### Exemplos:

```bash
# lista o conteúdo do diretório . (atual) de forma detalhada e formata os números de uma forma mais legível (usando múltiplos K, M, G, T)
ls -lh
# lista o conteúdo de . de forma simples para ser usados em script
for f in $(ls -1); do
    echo "$f"
done
# lista o conteúdo do diretório . em formato de colunas
ls 
# lista de forma detalhada . ocultando o dono e o grupo
ls -log
# lista de forma detalhada . e ordenada pelo tamanho (maior primeiro)
ls -lS
# lista de forma detalhada . e em ordem crescente de tamanho
ls -lSr
# lista de forma detalhada . e ordenada pela extensão
ls -lX
# lista de forma detalhada . e ordenada pelo último acesso
ls -ltu
# lista de forma detalhada . e ordem alfabética reversa
ls -lr
# lista de forma detalhada . e ordenada pelo último acesso
ls -lt --time=ctime
# supondo que o caminho seja um link simbólico, informa para onde ele está apontando
ls -l Vídeos
# supondo o mesmo, informa o conteúdo apontado pelo link (ou seja, segue ele)
ls -lH Vídeos
# lista detalhadamente . e esconde os de extensão c, cpp, h e seus backups (sufixo ~)
ls -lGI'*.c' -I'*.cpp' -I'*.h'
# lista informação detalhada do diretório . e não mais do seu conteúdo
ls -a
# lista os últimos 10 arquivos (incluindo ocultos) de . recentemente modificados
ls -at | head -n10
# as permissões são normalmente: -rwsr-xr-x, indicando que o set-user-bit está setado, o que permite que usuários comuns tenha permissões de root, o dono desse arquivo. Outro exemplo é /sbin/sudo
ls -l /sbin/passwd
# as permissões são normalmente: drwxrwxrwt, indicando que o sticky-bit está setado, o que impede que usuários comuns mesmo com permissões elevadas possam renomear ou deletar arquivos de outros usuários, a menos que ele seja o dono do arquivo, ou o dono do diretório ou o root.
ls -ld /tmp
```

## cp

Copia arquivos (e também diretórios)

### Opções úteis:

- `-a` preserva atributos do arquivos (como modo, dono, horários)
- `-b` faz o backup se o destino estiver para ser sobrescrito
- `-f` se existir no destino, sobrescreva
- `-i` habilite modo windows, pergunte antes
- `-r` em diretórios, também copia todos seus subdiretórios
- `-u` só copia se a origem for mais nova ou caso não exista no destino
- `-v` modo verboso, útil em depuração
- `-S {ch}` informa o sufixo do backup, padrão é ~

## mv

Movimenta ou renomeia arquivos

### Opções úteis:

- `-b` faz o backup se o destino estiver para ser sobrescrito
- `-f` se existir no destino, sobrescreva
- `-i` habilite modo windows, pergunte antes
- `-u` só copia se a origem for mais nova ou caso não exista no destino
- `-v` modo verboso, útil em depuração

## mkdir

Cria diretórios

### Opções úteis:

- `-m` define o modo do diretório
- `-p` se houver algum diretório não existente no caminho, cria-os
- `-v` modo verboso, útil em depuração

### Exemplos:

```bash
# se a ou b não existir falha
mkdir /tmp/a/b/c
# sempre cria a árvore completa de diretórios
mkdir -p /tmp/a/b/c
```

## info

No mundo Unix por padrão é utilizado o `man` para visualizar manuais, o que por sua vez é herdado pelo mundo Linux. No mundo GNU o padrão é utilizar o `info` que visualiza um formato de arquivo diferente do suportado pelo `man`.

### Atalhos úteis:

- `n` vai ao nó seguinte
- `p` vai ao nó anterior
- `l` vai ao último nó visualizado
- `d` vai ao nó geral do info, permitindo visualizar todos os documentos registrados.
- `u` ou `t` vai ao nó raíz do documento
- `m` lista os nós possíveis a partir do nó corrente (útil quando está no primeiro nó)
- `H` mostra um guia das teclas de atalho e comandos
- `C-v` e `M-v` rola a página inteira para baixo e para cima, respectivamente. `M` significa `alt`
- `C-a` e `C-e` vai para o início e fim da linha, respectivamente
- `C-f` e `C-b` vai para o caractere próximo e anterior, respectivamente

**Dica rápida:** No geral o esquema de teclas é o mesmo do *Emacs*. Setas também funcionam!

## ps

Mostra informações de processos

### Colunas e significados

- `%CPU` porcentagem de utilização da CPU
- `%MEM` porcentagem de utilização da memória física
- `C` porcentagem de utilização da CPU (valor inteiro)
- `COMMAND` linha de comando completa do processo incluindo argumentos
- `UID` *id* ou nome de usuário que iniciou o processo
- `PID` número de processo
- `PPID` número de processo pai
- `ELAPSED` tempo decorrido após começar a executar
- `STIME` hora que começou a executar
- `STARTED` data e hora precisa em que começou a executar
- `EXE` o nome do executável
- `TIME` tempo de CPU utilizado (cumulativo)
- `MACHINE` contâiner ou VM associada
- `TTY` interface de terminal utilizada - se real (`tty`), se emulado (`pts`)
- `SZ` número de páginas ocupadas fisicamente - inclue áreas .text, .data e .stack
- `VSZ` memória virtual (em `KB`)
- `RSS` memória ocupada em RAM que não foi trocada via *swap* (em `KB`)
- `USS` memória ocupada em RAM privada que não foi trocada via *swap* (em `KB`)
- `DRS` memória privada do processo (ou .data)
- `PSR` qual core do processador ultima vez utilizado
- `RBYTES` número de bytes lidos do disco
- `WBYTES` número de bytes escritos do disco
- `ROPS` número de operações de leitura por I/O
- `WOPS` número de operações de escrita por I/O
- `NLWP` número de threads em execução (vêm de *light weight process*)
- `STACKP` endereço do início da pilha
- `LWP` número de identificação da thread
- `NI` número de *nice* que permite ajustar a nível de usuário a prioridade do processo (valor entre -20 e 19; quanto menor mais prioritário se torna o processo)
- `PRI` número de prioridade (varia entre `0` (mais prioritário) a `139` (menos prioritário), onde `0` a `99` são processos *real-time* enquanto `100` a `139` são processos normais). O valor da prioridade é ajustado seguindo a expressão `PRI = NI + 20`.
- `F` *flags* associados ao processo

**Dica rápida:** O programa `nice` serve para executar um programa com um *nice* aplicado enquanto que o `renice` serve para ajustar o *nice* de algum programa já em execução. Ambos necessitam ser `root` para serem chamados.

### Opções úteis:

#### Seleção

- `-e` ou `-A` seleciona todos os processos de todos os usuários
- `-p {pid}` seleciona apenas os que possuem PID informados (mais de um, separados por vírgula)
- `-C {term}` seleciona apenas os que casam o termo informado em seu CMD
- `--ppid {pid}` seleciona apenas o que possuem PPID informados (mais de um, separados por vírgulas)
- `-t {tty}` seleciona apenas os que possuem o mesmo número de TTY informados (mais de um, separados por vírgulas)
- `-u {uid|name}` seleciona apenas os que possuem UID ou nome de usuário informados (mais de um, separados por vírgulas)

#### Formatação

- `-f` exibe informações detalhadas
- `-F` exibe o máximo (padrão) de informações detalhadas. Pode ser combinado com `-L` que mostra o número de threads e os identificadores delas
- `-j` exibe os processos daquele shell (similar ao `ps` sem argumento)
- `-ly` exibe informações detalhadas com adição do estado do processo, prioridade, qual função de kernel que mantém o processo dormindo, número de *nice*.
- `-o {cols}` exibe apenas as colunas informadas
- `-O {cols}` exibe as colunas listadas em conjunto com algumas pré-determinadas

**Dica rápida:** Em `-o` ou `-O` é possível definir a largura da coluna usando a sintaxe `col:length`. Também é possível renomear a coluna usando a sintaxe `col=name`, onde `name` é uma *string* sem conter espaços, incluindo dentro de aspas (que não é aceita). Se desejar espaço substitua por `_`. É válido também `col=`, ou seja, a coluna terá seu nome em branco.

#### Modificadores de saída

- `-w` define a largura do terminal (trunca o texto sem maior)
- `-ww` define largura ilimitada do terminal (continua na próxima linha)
- `--rows={num}` define a altura do terminal
- `-H` ou `--forest` mostra hierarquia no formato de árvore
- `--headers` em listagem que excedem a altura do terminal, lista para cada novamente o cabeçalho
- `--no-headers` sem cabeçalho
- `--sort={columns}` ordena conforme as colunas informadas que quando mais de uma são separadas por vírgulas. Um prefixo `-` na coluna inverte a ordem natural, que é crescente para colunas numéricas e lexicográfica para colunas textuais.

#### Formatação de threads

- `-L` exibe *threads*, pode ser útil adicionar `-Olwp,nlwp`
- `-m` *threads* vêm após seus processos, pode ser útil adicionar `-Olwp,nlwp`

#### Miscelânia

- `L` sem `-` mesmo, uma vez que é opção ao sabor do BSD. Mostra todas as colunas válidas com seus termos para serem fornecidos em `--sort` ou `-O` (ou `-o`).

### Processos e seus estados

O estado de um processo é exibido na coluna `S` ou `STAT`, e pode ser a qualquer momento um dos abaixos:

- `D` dormindo de forma não interrompível, normalmente fazendo I/O
- `I` *thread* de *kernel* que está em *standby*
- `R` executando
- `S` dormindo de forma interrompível, normalmente aguardando algum evento completar
- `T` parado por sinal de controle de *jobs*, normalmente quando coloca alguma tarefa em *background* no terminal via `C-z`
- `t` parado por de um depurador durante seu *tracing*
- `X` morto (não deveria ser visto)
- `Z` defunto ou zumbi (quando ele finaliza mas o seu pai não é notificado apropiadamente; o processo de PID 1 - `/sbin/init` - normalmente é quem encerra processos nesse estado)

### Chaves dos cabeçalhos

São usadas como argumentos do `--sort`, `-o` ou `-O` para selecionar as colunas desejadas na operação. Todas as chaves são mostradas também via `ps L`. Abaixo algumas:

| chave | coluna  |
| :---: | :----:  |
| %cpu | %CPU |
| %mem | %MEM |
| c | C |
| cgroup | CGROUP |
| cmd | CMD |
| comm | COMMAND |
| cpuid | CPUID |
| cputime | TIME |
| etime | ELAPSED |
| exe | EXE |
| f | F |
| flags | F |
| gid | GID |
| longtname | TTY |
| lstart | STARTED |
| lwp | LWP |
| m_drs | DRS |
| m_size | SIZE |
| m_trs | TRS |
| machine | MACHINE |
| nice | NI |
| nlwp | NLWP |
| numa | NUMA |
| opri | PRI |
| pcpu | %CPU |
| pid | PID |
| ppid | PPID |
| pri | PRI |
| psr | PSR |
| pss | PSS |
| rbytes | RBYTES |
| rchars | RCHARS |
| rops | ROPS |
| rss | RSS |
| rsz | RSZ |
| s | S |
| sgi_rss | RSS |
| size | SIZE |
| stackp | STACKP |
| start | STARTED |
| start_time | START |
| stat | STAT |
| state | S |
| stime | STIME |
| sz | SZ |
| time | TIME |
| trs | TRS |
| trss | TRSS |
| tsig | PENDING |
| tty | TT |
| ucmd | CMD |
| ucomm | COMMAND |
| uss | USS |
| vsize | VSZ |
| vsz | VSZ |
| wbytes | WBYTES |
| wcbytes | WCBYTES |
| wchan | WCHAN |
| wchars | WCHARS |
| wname | WCHAN |
| wops | WOPS |

## du

Estima o uso de espaço do diretório (aparentemente o nome vem de *disk usage*)

### Opções úteis:

- `-a` contabiliza tudo, o padrão
- `-h` mostra números em formato legível para humanos
- `-H` segue links simbólicos
- `-B{K|M|G|T}` define o formato numérico: kilobytes, megabytes etc; padrão é bytes (sem o `-B`)
- `-c` mostra o total na última linha
- `-s` mostra apenas o total

## kill

Envia sinais para processos, sendo o padrão `SIGTERM` se não especificado

### Lista do sinais:

```
 ------------------------------------------------------------------------------
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX	
 ------------------------------------------------------------------------------
```

**Dica rápida:** Um sinal pode ser referenciado tanto pelo número como pelo nome (sendo aceito tanto o formato `KILL` como `SIGKILL`).

**Dica rápida:** Os sinais mais comuns são SIGINT(2), SIGQUIT(3), SIGABRT(6), SIGKILL(9), SIGALRM(14) e SIGTERM(15)

### Opções úteis:

- `-{SIGNAL}` envia o sinal informado
- `{PID}` um ou mais PID dos processos que receberão o sinal

### Exemplos:

```bash
# mata o processo do shell atual
kill -SIGKILL $$
# o mesmo acima
kill -9 $$
# o mesmo acima
kill -KILL $$
# aborta o processo do firefox
kill -SIGABRT `pidof firefox`
```

## read

Lê uma linha e coloca em uma ou mais variáveis

### Opções úteis:

- `-a {var}` lê a linha e armazena no array `var` cada palavra (separadas por espaço ou o caractere em `$IFS`)
- `-s` não ecoa na tela o conteúdo digitado
- `{var}` a variável na qual armazenará
- `-p {texto}` mostre o texto informado como prompt antes de tentar ler
- `-n {count}` lê até `count` caracteres e retorna imediatamente
- `-d {char}` o padrão é ler até encontrar um `\n` mas pode ser outro caractere

### Exemplos:

```bash
# Lê uma linha e coloca em $name
read -p 'Informe seu nome: ' name
# Lê sem ecoar e coloca em password
read -s -p 'Informe sua senha: ' password
# Lê duas palavras
read -p 'Informe seus números: ' n1 n2
# Lê e coloca em array
read -a numbers 'Informe seus números: '
```

## chmod

Modifica as permissões de um arquivo, através de duas sintaxes:

- *octal*, como em `chmod 777`
- *simbólico*, como em `chmod u+o=rw`

O formato simbólico base é: `chmod [ugoa][-+=][rwxXst]` onde `[ugoa]` representa a entidade que recebe as permissões para o dono (*user*), grupo (*group*), outros (*others*) ou todos (*all*), respectivamente. As permissões podem ser setadas via `+`, podem ser limpas via `-` e podem ser atribuídas como são, via `=`. A letra `X` define permissão para entrar no diretório somente se já houver para outros usuários, enquanto `s` (o *set-user-bit* ou *set-group-id*) e `t` (o *sticky-bit* ou *deletion-flag*).

Por outro lado, no formato octal a base é: `chmod ugo` onde cada letra `u`, `g` e `o` representa um digito octal (`0` a `7`) que define as permissões de leitura (*read*), escrita (*write*) e execução (*execution*), ou rwx. Assim, `7` define `111` em binário que resulta em permissões concebidas de leitura, escrita e execução, enquanto `5` define `101` que resulta em permissões de leitura e execução mas não em escrita.

**Dica rápida:** Uma permissão comum para arquivos é `chmod 644` que concede permissões de leitura e escrita para o dono (usuário) e leitura para o grupo e outros.

**Dica rápida:** No modo simbólico, quando não fornecido quem recebe, considera todos (*all*). Também quando atribuído as permissões, limpa as que não foram Explicitamente informadas.

### Opções úteis:

`-c` quando houver modificação reporta na saída
`-f` modo silencioso
`-R` modo recursivo
`-RH` siga os links no modo recursivo

### Exemplos:

```bash
# todos recebem permissão para execução, outras permissão são deixadas intactas
chmod +x arq
# todos tem a permissão de execução revogada, outras permissão são deixadas intactas
chmod -x arq
# o dono tem as permissões de escrita e leitura, outras permissão são deixadas intactas
chmod u+rw arq
# o dono tem as permissões de escrita e leitura mas é revogada a de execução
chmod u=rw arq
# todos tem as permissões de leitura mas as demais (escrita e execução) são revogadas
chmod a=r arq
# todos tem todas as permissões concedidas
chmod 777 arq
# todos tem todas as permissões revogadas
chmod 000 arq
```

## chown

Modifica o dono e grupo de arquivo

Há três formatos:

- `chown {owner} file` que modifica o dono apenas
- `chown {owner}:{group} file` que modifica o dono e o grupo
- `chown :{group} file` que modifica o dono apenas

### Opções úteis:

`-c` quando houver modificação reporta na saída
`-f` modo silencioso
`-R` modo recursivo
`-RH` siga os links no modo recursivo

**Dica rápida:** Normalmente essas modificações exigem permissões de super-usuário (root).

### Exemplos:

```bash
# modifica o dono para r2d2 e o grupo para droids
chown r2d2:droids arq
# modifica apenas o dono para r2d2
chown r2d2 arq
# modifica o dono para r2d2 de todos os arquivos a partir do .
chown -R r2d2 .
# modifica o grupo para droids de todos os arquivos a partir do .
chown -R r2d2 .
```

## passwd

Altera a senha do usuário assim como mostra estado dela

Usuários comuns alteram apenas sua senha e root a de todos

### Opções úteis:

- `-d` deleta a senha, deixando-a vazia, fazendo a conta do usuário sem senha
- `-e` expira a senha imediatamente
- `-l` trava a senha imediatamente, não deixando o usuário se logar por esse método de autenticação (login e senha) mas ainda via outros (como chaves SSH)
- `-n` define a quantidade mínima de dias para alterações de senha
- `-x` define a quantidade de dias para expiração da senha, após isso será necessário alterar; valor de `-1` irá desativar a expiração!
- `-w` define a quantidade de dias antes da senha expirar para ser avisado sobre
- `-S` mostra o estado da senha
- `-Sa` mostra o estado da senha de todos os usuários
- `-u` destrava uma senha

**Dica rápida:** O estado da senha é mostrada em 7 campos: o primeiro é o nome do usuário; o segundo é L se a senha está travada, NP se não tem senha ou P se tem uma senha válida; o terceiro é a data da última modificação da senha; o quarto é a quantidade mínima de dias para modificação; o quinto é o tempo para expiração; o sexto a quantidade de dias para avisar sobre modificação; e o sétimo, o número de dias em que a conta se encontra expirada.

**Dica rápida:** Normalmente modificações além da senha, como tempo de expiração exigem conta de super-usuário.

### Exemplos:

```bash
# é solicitado a senha atual e caso esteja correta, o programa solicita a nova e sua repetição
passwd
# mostra o estado da senha de todos os usuários
sudo passwd -Sa
# modifica o tempo de expiração e a quantidade de dias para avisar sobre 
sudo passwd -x 365 -w 30 $USER
```

## dd

Converte e copia um arquivo (a nível de byte)

### Operandos comuns:

- `bs={bytes}` vem de `block size` e define o tamanho básico do bloco; se não informado, padrão é `512`
- `count={n}` define a quantidade de blocos a serem lidos; se não informado é `1`
- `if={file}` define o arquivo de entrada
- `of={file}` define o arquivo de saída
- `iflag={flags}` define um ou mais flags na entrada
- `oflag={flags}` define um ou mais flags na saída
- `seek={n}` salta os primeiros `n` blocos na saída
- `skip={n}` salta os primeiros `n` blocos na entrada
- `status={none|noxfer|progress}` define o nível de verbosidade, onde `none` é silencioso até encontrar erros, `noxfer` só mostra a estatística final e `progress` mostra em tempo real as estatísticas
- `conv={convs}` define um esquema de conversão (múltiplos valores são separados por vírgulas)

### Conversões comuns:

|  conv | descrição |
| :----: | :--------:|
| ascii | de EBCDIC para ASCII                                                              |
| ebcdic | de ASCII para EBCDIC                                                             |
| block | preencha com espaços para ocupar o tamanho de `cbs` nos blocos terminados em `\0` |
| unblock | desfaça a operação acima                                                        |
| lcase | converta maiúsculas para minúsculas                                               |
| ucase | converta minúsculas para maiúsculas                                               |
| swab | faça swap entre cada par de bytes da entrada                                       |
| excl | aborte se o arquivo de saída existir                                               |
| notrunc | não trunque o arquivo de saída, que é o padrão                                  |
| noerror | se houver erros na leitura, continue!                                           |

### Flags comuns:

|  flags | descrição |
| :----: | :--------:|
| append | apenas em `oflag` e com `conv=notrunc`; cria se não existir; adiciona ao final       |
| direct | use se possível accesso direto                                                       |
| directory | falha a menos que seja diretório                                                  |
| dsync | nos dados utilize I/O síncrona                                                        |
| sync | nos dados e metadados utilize I/O síncrona                                             |
| fullblock | apenas em iflags; preencha completamente os blocs de entrada                      |
| nonblock | utilize I/O não-bloqueante                                                         |
| noatime | não atualize o atime                                                                |
| nofollow | não siga links                                                                     |

**Dica rápida:** Podem ser utilizados sufixos nos valores numéricos, como `K`, `M`, `G` etc.

**Dica rápida:** Se for enviado `SIGUSR1` para o `dd` (sem operando `status`) em execução ele irá mostrar na stderr o estado atual e continuará em seguida

### Exemplos:

```bash
# converte os caracteres minúsculos de carta em maiúsculos em carta_mod
dd conv=ucase if=carta of=carta_mod
# 1M de zeros em nulos
dd bs=1K count=1K if=/dev/zero of=nulos
# incremente 1G de zeros em nulos em mostre o status em tempo real
dd conv=notrunc oflag=append bs=1M count=1K if=/dev/zero of=nulos status=progress
```

## df

Mostra o uso dos sistemas de arquivos

### Opções úteis:

- `-h` formato numérico legível para humanos
- `-a` mostra todos, incluindo pseudo, duplicados ou inacessíveis
- `-B` define o tamanho base do bloco, se `K`, `M`, `G` etc
- `-t` apenas mostra os sistemas de arquivos do tipo informado
- `-T` mostra uma coluna informando o tipo de sistema de arquivo

### Exemplos:

```bash
# mostra todos os sistemas de arquivos reais em uso mostrando seus tipos
df -Th
# mostra os sistemas de arquivos a partir de /dev/sda1
df -Th /dev/sda1
# mostra apenas os sistemas FAT32
df -htvfat
```

## find

Busca arquivos no disco

Há dois grupos de opções para o `find`, o primeiro *precede* os diretórios de busca, enquanto o segundo *sucede* os diretórios de busca.

### Opções úteis:

#### Primeiro grupo - ou argumentos (antecede os diretórios)

- `-P` nunca segue links simbólicos (padrão)
- `-L` segue links simbólicos
- `-H` não segue links com exceção exclusivamente na resolução dos diretórios de busca 
- `-D {exec|opt|rates|search|stat|tree|all|help}` opções de depuração, que pode ser bem útil quando quer saber exatamente o que o `find` está fazendo
- `-O {0|1|2|3}` habilita otimizações na busca ao custo de processamento; padrão é 1; otimização total é 3.

#### Segundo grupo - ou expressões (sucede os diretórios)

Há cinco tipos de expressões: a) testes; b) ações; c) opções globais; d) opções posicionais; e e) operadores

##### Testes

**Dica rápida:** Todos os números `n` podem ter o formato `+n` significando maiores que `n`, `-n` para menores que `n` e `n` para exatamente `n`.

- `-amin {n}` tempo em minutos desde o último acesso; o sufixo *min* não é de *mínimo*!
- `-anewer {file}` tempo desde o último acesso deve ser mais recente que de `file`
- `-atime {n}` tempo em horas desde o último acesso
- `-cmin {n}` tempo em minutos desde a última alteração de metadados
- `-cnewer {n}` tempo desde a última alteração de metadados deve ser mais recente que de `file`
- `-ctime {n}` tempo em horas desde a última alteração de metadados
- `-empty` arquivo de tamanho zero
- `-executable` permissão de execução
- `-name {glob}` busca sensível ao caso no *basename* (`c` de `./a/b/c`) do caminho apenas (o arquivo retirado a parte das pastas)
- `-iname {glob}` mesma busca acima porém insensível ao caso
- `-regex {regex}` busca sensível ao caso no caminho relativo do arquivo (`./a/b/c`) a partir do diretório informado
- `-iregex {regex}` mesma busca acima porém insensível ao caso
- `-mmin {n}` tempo em minutos desde a última alteração no conteúdo do arquivo
- `-mtime {n}` tempo em horas desde a última alteração no conteúdo do arquivo
- `-newer {file}` tempo desde a última alteração no conteúdo do arquivo deve ser mais recente que de `file`
- `-perm {mode}` o modo deve ser exatamente o das permissões do arquivo
- `-perm -{mode}` o modo deve conter nas permissões do arquivo
- `-perm /{mode}` ao menos uma das permissões deve conter nas permissões do arquivo
- `-readable` permissão de leitura
- `-samefile {file}` é o mesmo arquivo que `file`
- `-size {n}` pode conter sufixos `b` (padrão, bloco de 512 bytes), `c` (bytes), `w` (words), `k`, `M`, `G`
- `-type {type}` `b` arquivo de bloco, `c` arquivo de caractere, `d` diretórios, `p`, `f` arquivo comum, `l` link simbólico, `s` socket; múltiplos são separados por vírgulas
- `-used {n}` tempo em dias desde o último acesso
- `-user {name}` dono
- `-writable` permissão de escrita

##### Ações

- `-delete` deleta arquivos e retorna `0` no sucesso
- `-exec {cmd} \;` executa comando e retorna `0` no sucesso; tudo é parte do comando até o primeiro `;`; a string `{}` dentro de `cmd` é interpolada com o nome do arquivo
- `-exec {cmd} {} +` similar ao acima, porém para cada arquivo, anexa o `{}` corrente e executa após passar por todos os arquivos
- `-ok {cmd} \;` como `-exec` porém só executa o comando se o usuário concordar, para cada arquivo
- `-ls` lista no formato `ls -dils`
- `-fls {arq}` lista no formato `ls -dils` porém redirecionado para `arq`
- `-fprint {file}` imprime cada caminho completo de arquivo em `file`
- `-print` imprime cada caminho completo de arquivo seguido com `\n`
- `-printf {fmt}` imprime conforme o formato; algumas diretivas são `%h` (*dirname*), `%f` (*basename*), `%t` (última modificação em), `%M` (as permissões em formato simbólico), `%s` (tamanho em bytes)

##### Opções globais

- `-depth` sempre processa o conteúdo do diretório antes do próprio diretório
- `-maxdepth {n}` profundidade máxima na qual a busca será efetuada; mínimo é `0` porém só faz efeito com `-depth`, senão o mínimo é `1` (o atual, ou seja o conteúdo).
- `-mindepth {n}` profundidade mínima; mínimo `1`.

##### Opções posicionais

- `-daystart` defina o momento que `amin`, `cmin`, `ctime` (e tantos outros testes relacionados) irão se basear como sendo o início do dia e não sendo mais as últimas 24h (padrão)
- `-regextype {ed|emacs|awk|grep|egrep|sed}` o tipo de expressão regular que irá ser aplicada em `regex` ou `iregex`; há mais, veja com `find -regextype help`

##### Operadores

Na ordem de precedência (maior para menor)

- `(expr)`
- `! expr`
- `expr1 -a expr2` (ou `expr1 expr2`) *AND*; se `expr1` é falsa não avalia `expr2`
- `expr1 -o expr2` *OR*; se `expr1` é verdade não avalia `expr2`

### Exemplos:

```bash
# encontre todos os arquivos sem se preocupar com o caso de c, cc, cpp e h em /usr/share
find /usr/share -iname '*.c' -o -iname '*.cpp' -o -iname '*.cc' -o -iname '*.h' 2>/dev/null
# encontre coisas relacionadas a google e chrome
find /opt -regextype egrep -iregex '.*chrome.*' -a -iregex '.*google.*'
# os arquivos de /opt acessados nos últimos 5 minutos
find /opt -type f -amin -5
# arquivos em /opt que são mais novos que /opt/asdf-vm/asdf.sh 
find /opt -anewer /opt/asdf-vm/asdf.sh
# arquivos em /opt que foram acessados a mais de 24 horas
find /opt -atime +24
# arquivos txt em /usr/share/doc que foram modificados nos últimos 30 minutos
find /usr/share/doc -iname '*.txt' -amin -30
# arquivos txt de /usr/share/doc acessados no último ano
find /usr/share/doc -iname '*.txt' -atime -"$((24 * 30 * 12))"
# quantidade de arquivos vazios em /usr
find / -empty 2>/dev/null | wc -l
# arquivos que foram acessados a mais de 1 ano em /boot
find /boot -used +365 2>/dev/null
```

## grep

Verifica a entrada em busca de padrões definididos por expressões regulares (ou *regex*)

São suportados três formatos de expressões regulares:

1. Básica (ou **BRE**) - é a padrão
2. Extendida (ou **ERE**)
3. Compatível com Perl (ou **PCRE**) - não exposto aqui, exceto `\w`

**Dica rápida:** O `grep` foi criado em 1973 por Ken Thompson nos laboratórios da Bell Labs como parte da implementação do Unix original

### Gramática

Os caracteres `.?*+{|()[\^$` são especiais, ou seja, são metacaracteres

Qualquer outro é ordinário

- `.` algo; casa um caractere, qualquer que seja 
- `?` opcional; casa um ou nenhum caractere, qualquer que seja
- `+` mandatório; casa com um ou mais caracteres, qualquer que seja
- `*` qualquer; casa nenhum ou um ou mais caracteres, qualquer que seja
- `{N}` casa o item que precede exatamente `N` vezes, qualquer que seja
- `{N,}` casa o item que precede ao menos `N` vezes, qualquer que seja
- `{,M}` casa o item que precede no máximo `M` vezes, qualquer que seja
- `{N,M}` casa o item que precede ao menos `N` vezes e no máximo `M` vezes, qualquer que seja
- `|` alternativa; casa o item que precede ou o item que sucede, qualquer que seja
- `[lst]` lista; casa com um caractere da lista, qualquer que seja; a lista pode estar no formato de intervalo `[A-Z]`, `[a-z0-9]`
- `[^lst]` lista negada; casa com qualquer caractere que não esteja na lista, qualquer que seja
- `[:alnum:]` classe alfanumérica; caracteres alfabéticos ou numéricos
- `[:alpha:]` classe alfabética; caracteres alfabéticos, qualquer que seja
- `[:blank:]` classe brancos; caracteres de espaço ou tabulação
- `[:cntrl:]` classe controles; caracteres de controle, ex.: `<BS>`, `<RET>`
- `[:digit:]` classe dígitos; caracteres digito decimais: `0` a `9`
- `[:graph:]` classe gráfica; caracteres gráficos exceto espaço
- `[:lower:]` classe minúscula; caracteres de letra minúscula
- `[:upper:]` classe maiúscula; caracteres de letra maiúscula
- `[:print:]` classe imprimíveis; a mesma de `[:graph:]` porém incluindo espaço
- `[:punct:]` classe pontuação; caracteres de pontuação
- `[:space:]` clase espaço; caracteres de espaço: `\f`, `\n`, `\r`, `\t` e `\v`
- `[:xdigit:]` classe hexadecimal; caracteres de dígitos hexadecimais; `0` a `F`
- `^` casa com o início da linha
- `$` casa com o fim da linha
- `\b` casa com o início de uma palavra se colocada no início e casa com o fim se colocada no fim
- `\B` casa com o início de uma sequência de brancos (negação da palavra) se colocada no início e casa com o fim se colocada no fim
- `\<` casa com o início de uma palavra
- `\>` casa com o fim de uma palavra
- `\w` sinônimo de `[_[:alnum:]]`
- `\W` sinônimo de `[^_[:alnum:]]`
- `\1` a `\9` retroreferências (ou retrovisor), casa com o n-ésimo grupo casado da expressão

**Dica rápida:** A diferença entre **BRE** e **ERE** é que alguns metacaracteres (especificamente `?+{|()`) perdem seu significado especial na **BRE**, e devem ser usados de forma escapada como `\{2,}`, `[a-z]\+` ou `\(a\{3,5}\)`. Note que o `}` não perde seu significado especial e pode ser ser usado como é, ex.: `\{2,3}`. Na **ERE** não há necessidade de escapar nenhum desses metacarecteres e são escritos como são.

**Dica rápida:** Uma classe é sempre escrita em uma lista, pois internamente ela se transforma em um intervalo `first-last`

**Dica rápida:** O comportamento normal do `grep` é encontrar sempre a maior cadeia que casa com a expressão regular

### Opções úteis:

- `-E` habilita a sintaxe de **ERE**
- `-P` habilita a sintaxe de **PCRE**
- `-G` habilita a sintaxe de **BRE**, que é o padrão
- `-i` não diferencia maiúsculas de minúsculas
- `-v` inverte o comportamento, mostrando todas as linhas que não há casamento
- `-w` similar a envolver uma expressão com `\<` e `\>` ou `\b`
- `-x` similar a envolver a expressão inteira com `^` e `$`
- `-c` mostra apenas a quantidade de casamentos, e não as expressões casadas
- `-m {n}` limita até `m` casamentos, após encerra
- `-o` mostra exclusivamente as partes da linha casada, e não a linha inteira (que é o padrão)
- `-n` mostra o número da linha do casamento

### Exemplos:

```bash
```

## sed

Editor de *stream* para transformação e filtragem

Vem de *stream editor*

Há apenas uma passagem pelo *stream*, ao menos de forma padrão

### Opções úteis:

- `-n` modo silencioso
- `-e` comandos que serão executados
- `-f` comandos que serão executados através de um arquivo
- `-i {sufixe}` edita o original e não envia para `stdout`, caso seja fornecido um sufixo gera uma backup com esse sufixo
- `-E` habilita a gramática de **ERE**

**Dica rápida:** Se os comandos não forem informados via `-e` ou `-f` o `sed` pegará do primeiro argumento que não seja de opção

### Comandos úteis:

Todo comando `sed` segue o formato `{addr}{operation}{opts}`.

O endereço pode ser no formato `2,5` que é uma faixa entre as linhas `2` e `5` ou mesmo `!2,5` indicando qualquer linha que não entre `2` e `5`, ou pode ser uma expressão regular como `/^echo/` indicando que quando haver um `echo` no início da linha será este o endereço escolhido

As opções são aplicadas e específicas de cada comando

- `#` comentário
- `=` mostra o número da linha
- `a\{TEXT}` anexa `TEXT` ao fim
- `a {TEXT}` idem
- `c\{TEXT}` substitue a linha por `TEXT`
- `c {TEXT}` idem
- `d` deleta
- `e` executa um comando de shell dado na linha e substitue-a pelo resultado
- `i\{TEXT}` insere `TEXT` antes da linha
- `i {TEXT}` idem
- `p` mostra a linha (mais útil que o acima)
- `s/{regex}/{replacement}/[{flags}]` substituição de uma expressão regular por uma cadeia, ao estilo do vim, ou ao contrário?
- `y/{src}/{dst}/` transliteração dos caracteres, na ordem, dados de `src` em caracteres respectivos, na ordem, dados por `dst`; não suporta faixas `a-z`, tem que escrever todos os caracteres explicitamente!
- `{ CMD ; CMD ... }` agrupamento de comandos

#### O comando `s`

O termo vêm de *substitute*

A gramática das expressões regulares é basicamente a da **BRE**

Formato básico é `s/{regex}/{replacement}/{opts}`

O caractere separador `/` pode ser alterado para qualquer outro não-ambíguo com a gramática do `sed`, como `#` ou `,`, por exemplo

Algumas dicas:

- `\1` a `\9` retroreferências (ou retrovisor)
- `&` utilizado em `{replacement}`; é a cadeia completa em que houve casamento
- `\L{string}\E` utilizado também em `{replacement}`; serve para tornar a cadeia minúscula até o `\E` ou mesmo `\U` 
- `\U{string}\E` utilizado também em `{replacement}`; serve para tornar a cadeia maiúscula até o `\E` ou mesmo `\L` 
- `\l` utilizado também em `{replacement}`; o próximo caractere se tornará minúsculo
- `\u` utilizado também em `{replacement}`; o próximo caractere se tornará miúsculo
- `g` utilizado em `{opts}`; no comportamento padrão do `sed` quando encontra um casamento ele prossegue para linha seguinte, com `g` tenta casar ao máximo na linha antes de seguir para próxima
- `{n}` utiilzado também em `{opts}`; apenas substitue a `n`-ésima ocorrência
- `p` utiilzado também em `{opts}`; para cada substituição mostra como ficou
- `e` utiilzado também em `{opts}`; se houve uma substituição, executa o comando que foi casado na linha e substitui-a com o resultado dele
- `i` utiilzado também em `{opts}`; não diferencia maiúsculas de minúsculas
- `m` utiilzado também em `{opts}`; suporta linhas múltiplas, `^$` não irá casar somente em uma linha mas em múltiplas

### Exemplos:

```bash
# mostra o número de cada linha (1 a 26) da entrada e o conteúdo dela (uma letra em cada de a até z)
printf "%s\n" {a..z} | sed =
# adicionando após cada linha uma linha separadora
printf "%s\n" {a..z} | sed 'a\--------'
# deletando as números impares
seq 100 | sed '/^[0-9]\+[02468]\+$\|[02468]\+$/d'
# mostrando apenas os números impares
seq 100 | sed -n '/^[0-9]\+[02468]\+$\|[02468]\+$/p'
```

## awk

Ferramenta poderosa que ao mesmo tempo é uma linguagem de programação focada em extração e manipulação de textos e números em *streams*, mesclando funcionalidades do `sed` e do `grep`, assim como a linguagem C

**Dica rápida:** o `awk` foi criado em 1977 e vem dos sobrenomes de seus três co-criadores: **A**ho, **W**einberger e **K**ernighan; o Alfred Aho também criou também o `egrep` que implementou a **ERE**.

**Dica rápida:** O `awk` funciona de acordo com a fala de Alfred Aho - "awk lê a entrada uma linha por vez. Uma linha é varrida para cada padrão definido no programa, e para cada padrão que há casamento, a ação correspondente é executada."

A estrutura básica é:

```
condition { action }
condition { action }
```

### Opções úteis:

- `-F {sep}` define o separador de campos, o padrão é espaço
- `-v {var}={value}` seta alguma variável previamente

**Dica rápida:** O `awk` tem vários sabores mas esse resumo trata principalmente da versão da GNU, o `gawk`

**Dica rápida:** O `awk` foi diretamente influenciado por `grep` e `sed` o que por sua vez influenciou diretamente a linguagem `perl`.

**Dica rápida:** A gramática das expressões regulares é similar à **ERE**

### Variáveis pré-definidas:

- `NR` número de registros; é acumulativo, uma vez que não separa entre diferentes arquivos processados
- `FNR` número de registros do arquivo atual; em `BEGIN` é zero, em `END` é o número de registros
- `NF` número de campos
- `FILENAME` nome do arquivo
- `FS` caractere separador de campos, padrão é espaço
- `RS` caractere separador de registros, padrão é `\n`
- `OFS` caractere separador de campos de saída
- `ORS` caractere separador de registros de saída
- `OFMT` formatação padrão dos números de saída, que é inicialmente `"%.6g"`
- `ARGC` e `ARGV` número de argumentos e os argumentos passados ao script
- `ENVIRON` as variáveis de ambiente do shell

### Dicas:

- Variáveis seguem o alfabeto `A-Za-z_`
- Há suporte para arrays unidimensionais
- Strings são definidas dentro de aspas-duplas `"abc"`
- Concatenação de strings é feita simplemente aproximando-as como em `"abc" "def"`
- Comentários são feitos em linha com a primeira coluna da linha com `#`
- Declarações não precisam terminar em `;`
- `{ }` bloco comum
- `BEGIN { }` executado antes de todos os blocos comuns quando o programa inicia, apenas uma vez
- `END { }` executado após ter executado todos outros blocos e o programa está para encerrar
- Funções são definidas usando `function` como `function sum(x, y) { return x + y }`
- Chamadas de funções também é simplemente `sum(2,3)`
- Bordas em expressões regulares são definidas com `<` e `>`
- `a?`, `{n,m}`, `a|b`, `(abc)` e `a+` são suportados e não são escapados (como **ERE**)
- `str ~ regex` e `str !~ regex` denotam comparação por expressão regular, se `regex` casa ou não com `str`
- Quase todos os operadores são comuns aos das linguagens derivadas do C, com exceção do `^` que é exponenciação
- Algumas funções úteis:

    ```awk
    sin(x) cos(x) atan2(y,x)
    exp(x) log(x) sqrt(x)
    int(x) rand()
    srand([expr])

    gsub(regex, repl[, str]) # substituição global, se não informado str usa $0
    sub(regex, repl[, str]) # substituição unitária, se não informado str usa $0
    index(str, t) # busca t em str, indexado a partir de 1
    length([str]) # tamanho de str ou $0
    split(str, arr[, fs]) # quebra str em um array de substrings separadas pelo token fs ou FS global
    tolower(str)
    toupper(str)

    expr | getline [var] # pega linha via pipe de expr e armazena em $0 ou var
    getline # pega a linha e armazena em $0
    getline var < expr # pega a linha de expr e armazena em var
    system(cmd) # executa comando shell
    ```

## touch

Toca no arquivo modificando o `atime` (*access time*) e o `mtime` (*modification time*), ou apenas um deles. Se o arquivo não existir cria um novo vazio.

### Opções úteis:

- `-a` modifica apenas o *atime*
- `-m` modifica apenas o *mtime*
- `-c` não cria se não existir
- `-d''` define um timestamp arbirtrário

### Exemplos:

```bash
# cria um arquivo novo, supondo que ele não exista, se existir atualiza o *atime* e o *mtime*
touch arquivonovo.txt
touch -d'2024-01-01 00:00:59' arquivoantigo.txt
# modifica apenas o momento de acesso
touch -a arquivonovo.txt
```

## A linguagem do Bash

### Variáveis

Variáveis são setadas via `{var}={value}` (forma tradicional) ou `declare {var}={value}`

Por sua vez, podem ser limpas via `unset {var}`

Toda variável é manipulada como string embora usando `declare` é possível usá-la como inteiro

Em funções ou no próprio script, há algumas variáveis pré-definidas:

- `$0` o nome da função ou do script
- `$1` até `$9` o 1º até o 9º argumento; argumentos acima são acessados via expansão `${10}`
- `$*` expande para todos os argumentos a partir de `$1` separados por espaços a menos que esteja entre aspas duplas que são separadados pelo caractere em IFS
- `$@` o mesmo acima porém despreza o conteúdo de IFS como separador
- `$#` o número de parâmetros
- `$?` o retorno (`status code`) do último comando executado
- `$$` o PID do shell; em subshell retorna o do pai
- `$!` o PID do último job executado em background

Usando o comando **`declare`**:

Algumas vantagens sobre a forma tradicional de declaração:

- Melhora a legibilidade
- Permitir definir atributos das variáveis
- Permite múltiplas declarações ao mesmo tempo
- Permite atribuições conjuntamente com a declaração
- Permite valores constantes
- Permite valores locais
- Permite declarar e ao mesmo tempo exportar

Algumas opções:

- `-g` define variáveis globais quando declaradas em funções; variáveis declaradas sem `-g` em funções são locais!
- `-r` define um valor constante
- `-i` define um valor inteiro; toda atribuição a ela será tratada como expressão aritmética
- `-x` define a variável e em seguida exporta

#### Arrays

É possível definir tanto arrays indexados como arrays associativos

Para declarar há duas formas:

`var=(a b c)` (tradicional, indexada)

ou

`declare -a var=(value1 value2 value3 valueN)` (moderna, indexada)

`declare -A var=([key1]=value1 [key2]=value2 [key3]=value3 [keyN]=valueN)` (moderna, associativo)

Além das formas acima é possível atribuir em qualquer posição numérica qualquer que seja a variável que automaticamente se transformará em array indexado:

`a[100]=10` `a` vira um array de 101 posições

Para arrays associativos isso não é valido!

> Dica: Em qualquer forma indexada também é possível atribuir índices manualmente, como no array associativo, na verdade a diferença entre eles é simplesmente que o associativo tem índices do tipo string e no indexado é inteiro.

```bash
declare -r a="constante"
# erro
a=variável
# b=3 pois ela é inteira (-i)
declare -i b=1+2
echo $b
# declare e exporte
declare -x term=bash
# script.sh pode utilizar $term uma vez que ela foi exportada
./script.sh
```

### Expansões

O shell utiliza o mecanismo de expansão para gerar um conjunto de strings que serão utilizados como argumentos para comandos. As expansões são:

- Expansão de chaves
- Expansão de til
- Expansão de parâmetros e variáveis
- Expansão de comandos
- Expansão aritmética
- Expansão por substituição de processos

Em todas expansões é possível utilizar curingas (da lib `glob`):

- `*` expande para zero ou mais caracteres
- `?` expande para um caractere apenas
- `[...]` expande para um caractere da lista de caracteres informada; intervalos e classes de caracteres POSIX são suportados.
- `[!...]` similar ao anterior, porém negada, expande para qualquer um caractere exceto o da lista
- `\` escapa o caractere que sucede a barra

> Importante: A expansão de curinga não ocorre se o padrão for entre aspas simples: `'*'.txt`

Um outro nome para expansão de curingas é globbing.

Além da `glob` há também a `extglob` que por padrão é desativada em scripts mas ativa no prompt:

- `?(PADRAO)` expande para zero ou uma única ocorrência de `PADRAO`
- `*(PADRAO)` expande para zero ou mais ocorrências de `PADRAO`
- `+(PADRAO)` expande para uma ou mais ocorrências de `PADRAO`
- `@(PADRAO)` expande para uma ocorrência apenas de `PADRAO`
- `!(PADRAO)` expande para qualquer coisa exceto `PADRAO`; negação de `PADRAO`

O padrão `PADRAO` pode ser uma lista de padrões separados por `|`.

Assim como no `glob` as classes de caracteres POSIX são suportadas.

Outro mecanismo importante na expansão de parâmetros e variáveis é o da *quebra de palavras*:

O shell quando faz a expansão utiliza o caractere em `$IFS` para determinar as palavras ou termos gerados. Por padrão é o branco, que é definido como `IFS=$' \t\n'`, porém é totalmente modificável no shell.

```bash
# define manualmente os parâmetros do shell
set entrada=/usr/share/docs/file.txt saida=/usr/share/docs/file.pdf
# coleta os argumentos no formato key=value
for arg in "$@"; do
    # IFS é o caractere =, dessa forma $arg é expandido para tokens separados por =
    IFS='=' read -r key value <<< "$arg"
    echo "Chave: $key; Valor: $value"
done
```

#### Expansão de chaves

É um mecanismo parar gerar strings arbitrárias, através de `{}`

Primeiro formato segue os exemplos:

```bash
# gera as strings ab.ef abcef abdef
echo ab{.,c,d}ef
# gera as strings a b c
echo {a,b,c}
```

Segundo formato segue os exemplos:

```bash
# gera as strings a b c d e ... z
echo {a..z}
# gera as strings a d g j ... z
echo {a..z..3}
# gera as strings 0 10 20 30 ... 100
echo {0..100..10}
# gera as strings 1 2 3 4 5 ... 10
echo {1..10}
```

#### Expansão de til

É um mecanismo para facilitar o acesso a certos diretórios, através de `~`

Por padrão, o `~` é expandido para o conteúdo de `$PWD`

Caso seja `~-` o conteúdo é expandido para o `$OLDPWD`, que é o último diretório antes do `$PWD`

O `$PWD` também pode ser acessado via `$+`

Se o termo após o til for um nome de usuário, ele expande para o caminho do home desse usuário

```bash
# expande para /root
echo ~root
# expande para /home/saulo se o usuário for logado for saulo
echo ~
# expande para /home/paulo/Pictures
echo ~paulo/Pictures
# vai para /usr depois /tmp e volta com para /usr com ~-
cd /usr; cd /tmp; cd ~-;
```

#### Expansão de parâmetros e variáveis

O primeiro uso é para acessar mais que nove parâmetros posicionais: `${10}` ou `${22}`

O segundo uso é para proteger de mal-interpretação do nome da variável: `echo ${var}var`

Outros usos são:

- `${var[@]}` se `$var` for um array, expande para todos os valores separados por espaço; caso a indexação seja por `*` e a expansão seja entre aspas duplas separa pelo caractere em `$IFS`

- `${var:-TEXTO}` se `$var` for nula ou limpa (via `unset`) `TEXTO` é expandido para `stdout` mas não atribui a ela, senão usa o valor dela

- `${var:=TEXTO}` o mesmo acima, porém atribue a `$var` caso seja nula ou limpa senão usa o valor dela

- `${var:?TEXTO}` se `$var` for nula ou limpa `TEXTO` é expandido para a `stderr` com uma mensagem de erro, senão usa o valor dela

- `${var:+TEXTO}` se `$var` for nula ou limpa não faz nada, senão `TEXTO` é expandido para `stdout`

- `${var:OFFSET}` e `${var:OFFSET:LENGTH}` são formas de obter a substring da variável; para `OFFSET` negativo separe de var com um espaço para não ter ambiguidade com `{var:-TEXTO}`, ex: `${var: -5:2}`; se `$var` for um array para para acessar suas posições é necessário indexá-la por `@` ou `*`, ex: `${arr[@]:5:3}`

- `${!PREFIXO*}` expande para o nome de todas as variáveis no shell que tem prefixo `PREFIXO` separados pelo caractere em `$IFS`

- `${!PREFIXO@}` o mesmo acima, porém expande cada nome dentro de aspas duplas

- `${!var[@]` ou `${!var[*]}` sendo `$var` um array irá expandir para uma lista dos índices em que há valore não-nulos atribuídos; usando `@` a expansão é feita dentro de aspas duplas

- `${#var}` sendo `$var` um array expande para o tamanho dele; se `var` for `*` ou `@` expande para o número de parâmetros

- `${var#TEXTO}` expande `TEXTO` e remove a menor substring que casa no início dentro de `$var`; se `var` for `*` ou `@` é aplicado à cada parâmetro posicional, resultando em uma lista; `TEXTO` pode ser um `glob`!

- `${var##TEXTO}` o mesmo acima, porém remove a maior substring

- `${var%TEXTO}` expande `TEXTO` e remove a menor substring que casa no fim dentro de `$var`; se `var` for `*` ou `@` é aplicado à cada parâmetro posicional, resultando em uma lista; `TEXTO` pode ser um `glob`!

- `${var%%TEXTO}` o mesmo acim, porém busca a maior substring

- `${var/PADRAO/SUBST}` expande `PADRAO` e o busca em `$var` substituindo na primeira ocorrência por `SUBST`

- `${var//PADRAO/SUBST}` o mesmo acima porém substitue em todas as ocorrências

- `${var/#PADRAO/SUBST}` expande `PADRAO` e o busca no início de `$var`, substituindo na primeira ocorrência por `SUBST`

- `${var/%PADRAO/SUBST}` o mesmo acima porém busca no fim de `$var`; em todos os casos acima `PADRAO` pode ser vazio, nesse caso deletaria o padrão em `$var`

- `${var^}` converte o primeiro caractere de `$var` em maiúsculo

- `${var^^}` converte todos os caracteres de `$var` para maiúsculo

- `${var,}` converte o primeiro caractere de `$var` para minúsculo

- `${var,,}` converte todos os caracteres de `$var` para minúsculo

- `${var@OPERATOR}` converte os caracteres de `$var` de acordo com o valor de `OPERATOR` que pode ser: `U` (todos vão para maiúsculo), `u` (o primeiro para minúsculo), `L` (todos vão para minúsculo), `Q` (coloca o valor entre aspas duplas), `k` com `var` sendo um array e indexado por `@` ou `*` expande para a string de pares chave-valor separados por espaço, `K` o mesmo do anterior porém entre aspas duplas no valor

#### Expansão de comandos

Expande o conteúdo interpretando como um comando do shell e substitue por sua saída

Há apenas duas formas: a antiga \``cmd`\` e a moderna `$(cmd)`

A execução ocorre em subshell

Note que `$(cat ARQUIVO)` é mais LENTO que `$(< ARQUIVO)` (usando diretamente na expansão)

```bash
echo `ls -1 *.txt`
```

#### Expansão aritmética

Expande para o valor calculado da expressão aritmética dada por `$((expr))`, ou via`let var={expr}` ou mesmo usando `declare -i var={expr}`.

Exemplos:

```bash
a=10
# 23
echo $((a * 2 + 3))
# 5
echo $((a / 2))
# 16
let a=2 * 3 + 10
echo $a
# 9
declare -i a='2 * 6 - 3'
echo $a
```

#### Substituição de processos

A sintaxe `<(LISTA)` e `>(LISTA)` criam respectivamente arquivos temporários nas quais contém o conteúdo da saída de `LISTA`. O primeiro é um arquivo aberto para leitura e o segundo é um arquivo aberto para escrita. Não pode haver espaços entre `<` ou `>` e `(`.

Os comandos em `LISTA` são executados em subshell

```bash
# cria os arquivos temporários na quais seus conteúdos são a saída de `seq 10` e `seq 20` respectivamente e ainda joga a saída para outro comando temporário que nunca irei saber o nome e após é descartado
diff <(seq 10) <(seq 20) > >(echo -n)
```

### Comandos internos

- `:` faz nada mas serve para expandir o seu argumento
- `.` ou `source` lê o arquivo no shell atual
- `alias` define apelidos para comandos (opcionalmente com seus argumentos)
- `bind` lista e define atalhos para GNU Readline, que é ativo por ex. via `C-e` (início da linha) ou `C-a` (limpa a tela)
- `break`
- `cd`
- `continue`
- `command` executa um comando com seus argumentos ignorando qualquer função que possua o mesmo nome
- `declare` declara variáveis e seus atributos
- `echo`
- `enable` habilita ou desabilita (via `-n`) algum comando interno, permitindo usar comandos externos
- `eval` avalia uma string como um comando shell normal
- `exec` substitui a imagem do processo atual do shell por um outro comando e não retorna dele
- `exit`
- `export` faz variáveis ou funções declaradas no shell atual serem passadas para o ambiente de algum subshell; observe que com isso variáveis no shell atual não são herdadas automaticamente por subshell, a menos que sejam exportadas! Para listar todas variáveis exportadas `export -p` ou simplemente `export`
- `getopts` usado por scripts para ajudar no parseamento de argumentos
- `help` mostra uma ajuda do comando interno; com `-s` mostra uma breve linha informando seus argumentos
- `let` permite calcular expressões aritméticas de forma simples
- `local` define variáveis locais (dentro de funções)
- `logout` sai da sessão atual do shell e retorna ao pai
- `mapfile` lê e armazena em array com apenas esse comando todas as linhas de um arquivo, uma linha por posição; ex. armazenando em `$lines` as linhas de `arq`: `mapfile -t lines <arq`
- `printf` saída formatada
- `pwd` qual meu diretório atual?
- `readonly` torna variáveis constantes; ex.: `readonly a=10`
- `read` lê da entrada para variáveis
- `readarray` sinônimo de `mapfile`
- `return` retorna um número (código de retorno)
- `shift` retorna o primeiro dos parâmetros e faz shift-left neles
- `set` comando com muitas funcionalidades, entre elas definir parâmetros arbitrariamente e alterar opções e comportamentos do shell
- `shopt` ativa ou desativa extensões e bibliotecas no shell; `shopt -s {lib}` habilita a biblioteca; `shopt {lib}` testa se está ativa ou não; `shopt -u {lib}` desabilita a biblioteca
- `type` super útil, serve para dizer o que o comando de fato é: o caminho dele, se é um alias, se é um comando interno etc.
- `test` ou `[...]` executa expressões condicionais
- `times` mede o tempo que o comando informado levou para executar
- `trap` conecta um comando que será executado quando o shell receber um sinal arbitrário
- `unmask` define o modo de permissão padrão para novos arquivos e diretórios
- `unset` limpa variáveis
- `ulimit` define limites para processos iniciados pelo shell
- `unalias` remove apelidos

### Redireções

Segue o formato básico `descritor[[&]<|>]caminho-para-arquivo`.

O caractere `>` é para escrita e o caractere `<` é para leitura.

Por padrão `0` é a `stdin` (ou entrada-padrão), `1` é `stdout` (ou saída-padrão) e `2` é `stderr` (ou saída-erro-padrão).

Toda redireção é avaliada da esquerda para direita, antes de executar o comando.

Os descritores de arquivos estão em `/dev/fd`.

#### Formatos

- Redireção de entrada

    `[n]< ARQUIVO`

- Redireção de saída

    `[n]> ARQUIVO`

- Anexação de saída

    `[n]>> ARQUIVO`

- Redireção conjunta (`stdout` e `stderr`)

    `&> ARQUIVO` (ou `>& ARQUIVO`, ou também `> ARQUIVO 2>&1`)

- Anexação conjunta (`stdout` e `stderr`)

    `&>> ARQUIVO` (ou `>> ARQUIVO 2>&1`)

- *Here-document*

    ```
    [n]<<[-] DELIMITADOR
        TEXTO
    DELIMITADOR
    ```

    Por padrão, as variáveis no texto serão interpoladas, a menos que o delimitador seja no formato de aspas simples: `'DELIMITADOR'`

    O delimitador final não pode ser antecedido por nenhum caractere, ou seja, ele deve começar na primeira coluna

    Se o delimitador inicial for precedido por `-` então o texto manterá a indentação original

- *Here-string*

    `[n]<<< TEXTO`

    O texto sempre conterá o `\n` final

    **Dica rápida:** Se `n` não for informado subentende `0`

- Duplicação de descritor

    `[n]<& ARQUIVO`

    **Dica rápida:** Se `n` não for informado subentende `0`

    `[n]>& ARQUIVO`

    **Dica rápida:** Se `n` não for informado subentende `1`

    **Dica rápida:** ARQUIVO pode ser avaliado como `-` ou como algum caminho válido, porém se for avaliado como um `-` ele fecha o descritor `n`

    **Dica rápida:** Para abrir um descritor faça `exec [n]> ARQUIVO`, `exec [n]< ARQUIVO` ou `exec [n]<> ARQUIVO` para abrir para escrita, para leitura e para leitura e escrita respectivamente, onde `n` é o descritor

    **Dica rápida:** E como dito acima, para fechar `exec [n]>&-` onde `n` é o descritor

- Transferência de descritor

    `[n]<&[m]-` transfere o descritor `m` para `n` e fecha `m`

    **Dica rápida:** Se `n` não for informado subentende `0`

    `[n]>&[m]-` transfere o descritor `m` para `n` e fecha `m`

    **Dica rápida:** Se `n` não for informado subentende `1`

- Abertura de descritor para leitura e escrita

    `[n]<> ARQUIVO`

#### Exemplos

```bash
# stdout é desviada para arq
echo 1+1 >arq
# stdin é o arq 
bc <arq
# stderr é desviada para /dev/null (descarta)
ls nao-existo 2>/dev/null
# here-document é o stdin de cat que por sua tem seu stdout conectado a stdin de python
cat <<'FIM' | python
print('Ok from Python')
FIM
# stdout e stderr são desviados para arq
ls -R /opt &>arq
# stdin é a string 1+2
bc <<<1+2
# stdout é desviado como anexo em arq
echo a >>arq
#
ls 2>&/dev/null
```

### Pontos gerais

- Um separador de comandos é algum dos caracteres: `\n`, `;`, `||`, `&`, `&&`, `;;`, `;&`, `;;&`, `(`, `)`, `|` ou `|&`.
- O retorno do último comando é sempre guardado em `$?`.
- Comandos retornam de `0` (sucesso) a `127` quando executam e encerram sem intervenção externa
- Comandos que são encerrados por sinais externos retornam `128 + N` onde `N` é o número do sinal

### Tipos de comandos

- **Simples**: um comando com seus argumentos e opcionalmente redireções.

- **Pipeline**: uma sequência de comandos conectados por `|` ou `|&`, na qual a `stdout` do primeiro é canalizada a `stdin` do segundo, e assim sucessivamente. Usando `|&` a `stderr` também é canalizada para a `stdout` (é um atalho para `2>&1`). O valor de retorno do pipeline é dado pelo último comando. Cada comando no pipeline é executado em subshell (um shell que herda o shell corrente e roda em separado).

- **Lista**: uma sequência de comandos simples e/ou pipelines separados por um dos operadores `;`, `&`, `&&` ou `||`  e opcionalmente terminado por `;`, `&` ou `\n`. O valor de retorno da lista é dado pelo último comando.

    - O operador `&` faz o comando que o precede rodar em *background*

    > Uma vez o processo em background para retornar a ele execute `jobs` para procurar o `id` dele e faça `fg {id}`; para retonar ao shell ele precisa ir ao estado de parado, para isso tecle `C-z`; se quiser que ele volte a executar, porém no background faça `bg {id}`

    > Qualquer processo pode ser colocado em background, a menos que ele rejeite isso explicitamente no seu código

    - O operador `&&` faz o comando que o sucede rodar somente se o que o precede for bem-sucedido
    - O operador `||` faz o comando que o sucede rodar somente se o que o precede for mal-sucedido

    > Os operadores `&&` e `||` tem igual precedência e é maior que a dos outros operadores `&` e `;`.

- **Composto**: formam o que o shell fornece como suporte de sua linguagem de programação:

    **Condicionais**

    O comando **`if`** segue a estrutura:

    `if LISTA; then LISTA; [ elif LISTA; then LISTA; ] ... [ else LISTA; ] fi`

    Exemplos: 

    ```bash
    # note o ';' na parte que finaliza o then, é obrigatório em comandos de uma só linha
    if true; then echo sucesso; fi
    # note o ';' do else, também é obrigatório em comandos de uma só linha
    if false; then echo falha; else echo sucesso; fi
    # o mesmo visto anteriormente
    if ((1)); then
        echo sucesso
    fi
    # o mesmo visto anteriormente
    if false; then
        echo falha
    else
        echo sucesso
    fi
    # com elif
    if false; then
        echo falha de primeira
    elif false; then
        echo falha de segunda
    elif true; then
        echo sucesso de terceira
    else
        echo falha por último
    fi
    ```

    O comando **`case`** segue a estrutura:

    `case PALAVRA in [ [(] PADRAO [ | PADRAO ] ... ) LISTA ;; ] ... esac`

    Similar ao `switch` de linguagens derivadas do C, expande `PALAVRA` que normalmente é uma variável, e verifica cada uma das opções dado por `PADRAO` afim de encontrar um casamento, caso havendo executando `LISTA` que deve terminar obrigatoriamente `;;`, não sendo parte dela mas do `case`

    Exemplos:

    ```bash
    opcao="sim"
    case $opcao in
        [sS][iI][mM])
            echo Foi escolhido um sim!
            ;;
        [nN][ãaÃA][oO])
            echo Foi escolhido um não!
            ;;
    esac
    ```

    O comando **`select`** segue a estrutura:

    `select NOME [ in PALAVRA ] ; do LISTA ; done`

    Gera um menu iterativo com opções de escolha pelo usuário sendo cada termo provindo da expansão de `PALAVRA`

    Uma vez executado, o comando precisa encontrar algum `break` para sair dele, se não irá repetir até que o usuário informe `C-d` (fim-de-linha)!

    `select NOME [ in PALAVRA ]; do LISTA; ... break; done`

    ```bash
    select fruta in {maçã,uva,pêra,banana,abacate}; do
        echo "Você escolheu $fruta"
        break
    done
    ```

    O comando **`((...))`** segue a estrutura:

    `((EXPR))`

    Avalia a expressão no shell atual e retorna `0` se o resultado for diferente de zero e `1` caso contrário; tudo na expressão é tratado como se estivesse entre aspas duplas; não há necessidade de prefixar variáveis com `$`.

    O comando **`[[...]]`** segue a estrutura:

    `[[EXPR]]`

    Avalia a expressão (ou expressões) e retorna o valor exato da avaliação: `0` ou `1`.

    Diferentemente do comando `test` (ou `[]`) as expressões podem ser combinadas usando `&&` e `||`: `[[ 2 < 3 && 3 > 5 || 2 > 0 ]]` retorna `0` uma vez que `||` tem menor precedência que `&&`

    É necessário prefixar as variáveis!

    Pode conter expressões que serão expandidas pelo shell como `COMANDOS`, `$(COMANDOS)`, `~` (ou `~+`, `~-`, `~:`), `<(COMANDOS)`, `>(COMANDOS)`, sendo que as variáveis delas serão tratadas como se estivesse entre aspas duplas

    Os operadores `<` e `>` usam o locale para comparar cadeias

    O operador `==` e `!=` serve para comparar cadeias tratando como se o operando da direita tivesse o `extglob` habilitado

    O operador `=~` verifica se o padrão da direita (usando a sintaxe extendida `grep -E`) casa com a cadeia da esquerda; tem a mesma precedência de `==` e `!=`

    Exemplos:

    ```bash
    # comando simples
    ls -R /tmp >/dev/null 2>&1
    # comando em pipeline
    echo 'a;b;c' | cut -d';' -f2-
    ```

    **Loops**

    O comando **`for`** segue a estrutura:

    `for NOME [ [ in [ PALAVRA ... ] ] ; ] do LISTA ; done`

    Para cada termo a ser consumido da expansão de `PALAVRA`, armazene em `NOME` e execute `LISTA`

    Alternativamente, o **`for`** pode ter outra forma:

    `for (( EXPR1 ; EXPR2 ; EXPR3 )) ; do LISTA ; done`

    Loop ao estilo C e derivados, precisa explicar? Note que qualquer `EXPR` não precisar levar o prefixo `$` na variável

    O comando **`while`** segue a estrutura:

    `while LISTA-1; do LISTA-2; done`

    Executa os comandos de `LISTA-2` enquanto `LISTA-1` retornar um valor igual de zero

    O comando **`until`** segue a estrutura:

    `until LISTA-1; do LISTA-2; done`

    Executa os comandos de `LISTA-2` enquanto `LISTA-1` retornar um valor diferente de zero

    **Agrupamentos**

    O agrupador **`(...)`** segue a estrutura:

    `(LISTA)`

    É executado em subshell; o último comando da lista dá o valor de retorno

    O agrupador **`{...}`** segue a estrutura:

    `{ LISTA; }`

     É executado no shell atual; LISTA deve terminar ou com `;` ou `\n`; o último comando da lista dá o valor de retorno; pode ser aninhado; note que o espaço entre `LISTA;` e as chaves é obrigatório

### Operadores

- `a++` e `a--`

    Pós-incrementos

- `-a` e `+a`

    Sinais de menos e mais unários

- `++a` e `--a`

    Pré-incrementos

- `!expr`

    Negação lógica

- `~a`

    Negação bit a bit

- `a ** b`

    Exponenciação

- `a * b`, `a / b` e `a % b`

    Multiplicação, divisão e módulo

- `a + b` e `a - b`

    Soma e subtração

- `a >> b` e `a << b`

    Deslocamento de bits à direita e à esquerda

- `a <= b`, `a < b`, `a >= b` e `a > b`

    Comparações menor-ou-igual, menor, maior-ou-igual, maior

- `a == b` e `a != b`

    Comparações de igualdade e diferença

- `a & b`, `a | b` e `a ^ b`

    Operações AND, OR e XOR bit a bit

- `a && b` e `a || b`

    Comparações lógicas

- `a ? b : c`

    if ternário

- `a = b`

    Atribuição

- `a *= b`, `a /= b`, `a %= b`, `a += b`, `a -= b`, `a <<= b`, `a >>= b`, `a &= b`, `a ^= b` e `a |= b`

    Atribuições compostas

- `a, b`

    Operador vírgula

### Funções

As duas sintaxes são:

`NOMEFUNC () COMANDO-COMPOSTO [REDIRECOES]`

ou

`function NOMEFUNC [()] COMANDO-COMPOSTO [REDIRECOES]`

É recomendado utilizar chaves no corpo `{ COMANDO-COMPOSTO }` quando utilizado o `function` e não informado os parênteses `function NOMEFUNC { ... }`

O valor de retorno da função é dado ou por `return STATUS` (onde `STATUS` deve ser um inteiro) ou por qualquer comando que deve ser executado por último no corpo da função, sendo o valor de retorno deste o valor dela

Variáveis locais dentro da função são definidas utilizando `local {var}={value}` ou `declare {var}={value}`

Como visto no comando `declare`, é possível visualizar funções declaradas no ambiente via `declare -f` ou `declare -f NOME-FUNC`

Funções podem ser recursivas

Relembrando:

- `$0` nome da função
- `$1`, `$2`, ..., `$N` argumentos posicionais
- `${#@}` quantidade de argumentos passados

