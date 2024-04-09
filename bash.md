# Bash: linguagem, comandos e usos

✍  por *saulod2* 😉 em Abril de 2024

### basename

Mostra a última parte (o arquivo) de um caminho completo

```bash
# qml.hpp
basename /usr/include/qt/qml.hpp
```

### dirname

Mostra o que sobra de um caminho completo retirado a última parte (o caminho para)

```bash
# /usr/include/qt
dirname /usr/include/qt/qml.hpp
```

### cut

Exibe campos de um texto separados por um delimitador

**Opções úteis:**

- `-f` informa o número ou faixa do campo
- `-f2` campo 2
- `-f2-` todos os campos do 2 até o último
- `-f-7` todos os campos do primeiro até o 7
- `-f3-7` todos os campos entre 3 e 7
- `--output-delimiter=' '` informa que a faixa extraída deve ter o delimitador alterado para ' '

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

### head

Exibe as primeiras linhas de um arquivo (por padrão 10)

Também é possível exibir a nível de bytes (usando `-c`)

**Opções úteis:**

**Com o - em números descarta os últimos**

- `-n1` apenas a primeira linha
- `-n-1` todas exceto a última linha
- `-n20` as primeiras 20 linhas
- `-n-20` todas exceto as últimas 20 linhas
- `-c2` os dois primeiros bytes
- `-c-2` todos exceto os dois últimos bytes

### tail

Exibe as últimas linhas de um arquivo (por padrão 10)

Também é possível exibir a nível de bytes (usando a opção `-c`)

É muito comum para visualizar em tempo real o log de programas com `tail -f`

**Opções úteis:**

**Com o + em números descarta as primeiras N-1 linhas**

- `-n1` apenas a última linha
- `-n+2` todas exceto a primeira linha
- `-n20` as últimas 20 linhas
- `-n+21` todas exceto as primeiras 20 linhas
- `-c2` os dois últimos bytes
- `-c+3` todos exceto os dois primeiros bytes
- `-f` siga qualquer modificação

**Dica de ouro:** Em programas GNU os símbolos + e - normalmente tem o significado: + significa TOPO e - significa CHÃO

### seq

Gera uma sequência **numérica** arbitrária

**Nota:** Usando expansão de chaves `{min..max[..step]}` é possível obter o mesmo resultado com diferencial de que este gera sequências de caracteres. Exemplos: `echo {1..10}`, `echo {a..z}`, `echo Z{A..Z..4}`

**Exemplos:**

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

### sort

Ordena um conjunto de linhas usando diversos critérios de comparação (numérico, alfabético, meses, randomicamente). O padrão é alfabético e crescente.

**Opções úteis:**

- `-f` desativa a diferenciação entre maiúsculas e minúsculas
- `-n` comparação numérica
- `-M` comparação baseada nos prefixos de meses (JAN, FEV, MAR, ...)
- `-g` comparação numérica geral (mais lenta e pode ponto flutuante)
- `-s` comparação estável
- `-s` comparação estável
- `-k` define a chave para ordenação de linhas de múltiplos campos
- `-t` define o separador para ordenação com base nos campos

**Dica de ouro:** Comparação sempre utiliza o locale para nomes de meses e pontos decimais.

**Exemplos:**

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
JAN saulo alex
FEV jorge de la penha
MAR jorge de sá
ABR daniel matias
ABR roberta santana
MAI roberto santos
DEZ sílvio fabiano
FIM
```

### uniq

Seleciona os valores únicos de um conjunto de linhas

**Opções úteis:**

- `-c` conta ocorrências de cada valor único
- `-i` não diferencia maiúsculas de minúsculas

**Importante:** O `uniq` só consegue determinar linhas iguais quando são adjacentes, então é necessário ordenar antes!

**Exemplos:**

```bash
# aa\nab\naz\ncc\ncd\nce
echo ab cd cc ab ab de az aa ab de ab de | tr ' ' '\n' | sort | uniq
# conta as ocorrências unicas e não diferencia minúscula de maiúscula
echo ab cd Cc ab Ab de AZ aa ab DE ab de | tr ' ' '\n' | sort | uniq -ci
# 1\n5\n8\n10
echo {1,5,10} {1,5,10} {1,5,10} 8 | tr ' ' '\n' | sort -n | uniq
```

### shuf

Seleciona linhas de forma randômica da entrada

**Opções úteis:**

- `-n` selecione no máximo a quantidade especificada
- `-e` a entrada será uma string (use ***here-string*** `<<<` no bash)

**Exemplos:**

```bash
# cinco números aleatórios entre 1 e 10
shuf -e -n5 <<< echo {1..10}
# cinco números aleatórios entre 0 e 1
seq 0 .05 1 | shuf -n5
```

### date

Exibe a data e hora atuais do sistema em formato local e padrão ou personalizado

**Opções úteis:**

- `+''` define um formato. Note as aspas simples que não pertencem ao comando mas evitam o shell interpretar termos do formato (que usam o prefixo %)
- `-d''` transforma do formato ***unix timestap*** em hora e data locais. É necessário prefixar o timestamp com @ como em '@129034'.
- `-R` exibe no formato RFC 5322 (utilizado em e-mail)
- `-u` não utiliza o timezone local mas sim o UTC

**Exemplos:**

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

### join

Junta linhas de dois arquivos nas quais existem campos em comum. Por padrão utiliza a primeira coluna como campo de junção e o separador espaço.

**Dica de ouro:**

- Para ver usos do `join` suponha que haja dois arquivos, um contendo em cada linha um ID e comandos prediletos desse usuário e outro arquivo, contendo o mesmo ID, não necessariamente em ordem com o primeiro, e o sistema operacional que ele usa.

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

**Opções úteis:**

- `-t` define um separador alternativo
- `-j` define a coluna de comparação, ou seja o campo de junção (iguais nos dois arquivos)
- `-o` define um formato de saída alternativo
- `-1` define a coluna de comparação no arquivo 1
- `-2` define a coluna de comparação no arquivo 2

### paste

Mescla dois arquivos, linha por linha, em um único sem haver nenhuma comparação.

**Opções úteis:**

- `-d` define o delimitador

**Exemplos:**

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

### expr

Avalia expressões numéricas ou de cadeia (string)

**Expressões:**

- `A | B`, `A & B`
- `A < B`, `A <= B`, `A > B`, `A >= B`
- `A = B`, `A != B`
- `A + B`, `A - B`, `A * B`, `A / B`
- `A % B`
- `str : regxp` ou `match str regxp` verifica se str casa com regxp
- `substr str pos len` pos inicia em 1
- `index str substr`
- `length str`
- `()`

**Dicas:**

- Valores numéricos são sempre inteiros
- Alguns operadores como o * precisam ser escapados como em `expr 2 \* 3`

### comm

É um comando arcaico que serve para realizar comparações entre dois arquivos ordenados linha a linha, informando em três colunas
o resultado, sendo a 1ª os valores únicos do primeiro arquivo, sendo a 2ª os valores únicos do segundo arquivo
e a terceira os valores comuns em ambos.

**Dica de ouro:** Na prática, esse comando serve para testar a igualdade entre dois arquivos, e apenas isso. Use então `comm file1 file2 >/dev/null 2>&1; echo $?` para verificar isso!

### test

Testa uma porção de coisas em arquivos e também compara cadeias (strings)

Há duas sintaxes: `test expr` ou `[ expr ]`.

**Expressões:**

- `( expr )`
- `! expr` NOT
- `expr1 -a expr2` AND
- `expr1 -o expr2` OR
- `-n str` ou `str` str tem tamanho maior que zero
- `-z str` str tem tamanho zero
- `str1 = str2`
- `str1 != str2`
- `num1 -eq num2`
- `num1 -ge num2`
- `num1 -gt num2`
- `num1 -le num2`
- `num1 -lt num2`
- `num1 -ne num2`
- `file1 -ef file2` arquivos estão no mesmo dispositivo e possuem o mesmo inode
- `file1 -nt file2` file1 é mais novo que file2
- `file1 -ot file2` file1 é mais velho que file2
- `-b file` file existe e é de bloco
- `-c file` file existe e é de caractere
- `-d file` file existe e é diretório
- `-e file` file existe (lembre-se: diretórios também são arquivos)
- `-f file` file existe e é arquivo comum
- `-g file` file existe e tem o bit *set-group-ID* setado
- `-G file` file existe e é mantido pelo GID efetivo
- `-h file` ou `-L file` file existe e é um arquivo simbólico
- `-k file` file existe e está setado o *sticky bit*
- `-N file` file existe e foi modificado desde a sua última leitura
- `-O file` file existe e é mantido pelo UID efetivo
- `-p file` file existe e é um *named pipe*
- `-r file` file existe e o usuário tem permissão de leitura
- `-s file` file existe e tem tamanho maior que zero
- `-S file` file existe e é um *socket*
- `-t FD` FD (file descriptor) está aberto no terminal
- `-u file` file existe e tem o bit *set-user-ID* setado
- `-w file` file existe e o usuário tem permissão de escrita
- `-x file` file existe e o usuário tem permissão de execução

**Dica de ouro:** Lembre-se que o shell considera 0 (true) e 1 (ou qualquer outra coisa, false)

**Exemplos:**

```bash
# 0
test saulo = saulo; echo $?
# 1
test 2 -ne 2; echo $?
# Existe e você pode escrever nele! 
[ -w bash_commands.md ] && echo Existe e você pode escrever nele! 
```

### cat

Concatena um ou mais arquivos e mostra na saída

**Opções úteis:**

- `-n` mostra o número da linha
- `-E` mostra $ no fim de cada linha
- `-T` mostra tabulações como ^I

**Exemplos:**

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

### bc

Uma calculadora de números de ponto flutuante arbitrária


**Dica de ouro:**

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
- Nas bases numéricas `ibase` e `obase` os valores literais são definidos usando apenas um dígito de `{0-9,A-Z}`, assim: `ibase=G` significa base 16, `obase=A` significa base 10 enquanto `ibase=2` significa 2 mesmo, ou binário.
- A maior base numérica permitida é 36
- A menor é 2
- Os operadores seguem os da linguagem C, com exceção do `A ^ B` que é o de potência.
- **Cuidado:** `a = 3 < 5` no bc é interpretado como (1) `a = 3`; (2) `a = (3 < 5)`, ou seja primeiramente ele atribui!
- Há arrays `var[index]` porém não se pode inicializar múltiplas posições na declaração: `a[0] = 10; a[1] = 20; a[2] = 30;`
- Há diversas estruturas como: `{ }`, `if (expr) stmt1 [else stmt2]`, `while (expr) stmt`, `for ([expr1]; [expr2]; [expr3]) stmt`, `break`, `continue`, `return`, `define funcname (params) { NEWLINE AUTO_LIST stmt }` para funções.

**Funções:**

- `sqrt(X)`
- `read()` lê (um número) da stdin

**Funções usando o argumento -l (*math library*)**

- `s(X)` seno
- `c(X)` cosseno
- `a(X)` arctg
- `l(X)` log
- `e(X)` e^X

**Exemplos:**

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
```

### tr

Transforma a entrada substituindo caracteres de um alfabeto por um outro

**Opções úteis:**

- `-c` utiliza ao invés, o complementar do primeiro alfabeto
- `-d` deleta caracteres do primeiro alfabeto
- `-s` reduz caracteres repetidos (em sequência) do alfabeto para um único
- `-t` trunca o primeiro alfabeto para o tamanho do segundo

**Dica de ouro:**

- O alfabeto pode ser informado como `aeiou`, por exemplo.
- Suporta intervalos como: `A-Z`, `a-z`, `0-9`, `a-f`
- Suporta listas: `[12345abc]`, `[^A-Z]`
- Suporta classes de caracteres: `[[:alnum:]]`, `[[:digit:]]`, `[[:alpha:]]`, `[[:blank:]]` e outras mais
- Suporta repetição de caractere como no exemplo: `[A*5]`, 5 caracteres A ou `[A*]`, repetição de A tantas vezes for necessário para casar com o tamanho do outro alfabeto

**Exemplos:**

```bash
# deleta todas as linhas vazias
tr -s '\n' < main.c
# troca maiúsculas por minúsculas
cat | tr A-Z a-z
# troca os caracteres do alfabeto aeiou por 12345, nessa ordem
echo era uma vez três porquinhos | tr aeiou 12345
```

### wc

Informa o número de bytes, caracteres, palavras, linhas ou a largura máxima do arquivo

**Opções úteis:**

- `-c` conta bytes
- `-m` conta caracteres
- `-l` conta linhas
- `-w` conta palavras
- `-L` diz a largura máxima

### echo

Ecoa na tela conteúdo de cadeias e gera uma nova linha

**Opções úteis:**

- `-e` permite usar caracteres de controle como \n ou \x
- `-n` não gera uma nova linha

### cd

Muda para algum diretório

### pwd

Mostra o diretório atual

### od

Realiza um dump dos bytes do arquivo em várias bases, que por padrão é octal

**Opções úteis:**

- `-A` define a base numérica dos endereços: d (decimal), x (hexadecimal), o (octal) ou n (não exibe ela!)
- `-t` define a representação de cada byte: a (caracteres como apresentados no ASCII), b (octal), c (como a, \n, \t, ã), (f) reais, x (hexa)

>> ✅ É possível definir a largura do byte manualmente usando um sufixo no valor de `-t`, como -tx1 (hexa de 1 byte)

>> ✅ Qualquer tipo com sufixo, último de todos, `z` permite que seja exibido na coluna mais a direita a representação ASCII da linha, como em `-tx1z`

- `-j` define o offset inicial, ou seja, quantos bytes serão deslocados antes de exibir o dump
- `-N` define a quantidade máxima de bytes a serem lidos

**Dica de ouro:** É possível utilizar sufixos nos números do offset ou do tamanho como `b` (bloco), `KB` (1000), `K` (1024), `MB` (1000\*1000), `M` (1024\*1024) etc.

**Exemplos:**

```bash
# pega 10 bytes e exibe com endereços hexadecimais e um byte hexadecimal por coluna mostrando o caractere (na coluna mais a direita)
head -c10 /dev/random | od -Ax -tx1z
# gerador aleatório de letra minúscula
printf "\x$(bc <<< "obase=G;97 + $(head -c1 /dev/random | od -An -td | tr -d ' ') % 27")\n"
```

### tee

Lê a stdin e ecoa na stdout e em arquivos ao mesmo tempo

**Opções úteis:**

- `-a` anexa conteúdo no final

**Exemplos:**

```bash
# leia a entrada padrão e cuspa na saída padrão assim como nos arquivos, zerando primeiro
tee file1.txt file2.txt
# leia a entrada padrão e cuspa na saída padrão assim como nos arquivos, anexando ao final
tee -a file1.txt
```

### tac

Faz o que cat faz, porém concatena os arquivos a partir da última linha até a primeira (reversamente)

**Exemplos:**

```bash
tac | sort
```

### ls

Lista o conteúdo de diretórios

Por padrão a listagem é em ordem alfabética

**Opções úteis:**

- `-l` formato de lista
- `-m` formato de vírgulas (útil em script)
- `-C` formato de colunas (padrão)
- `-1` formato um por linha
- `-a` mostra arquivos ocultos
- `-R` lista o diretório corrente e todos seus subdiretórios
- `-I {pattern}` esconde arquivos que casam com pattern (usando curingas do shell)
- `-A` não mostra os diretórios `.` e `..`
- `-B` não mostra os arquivos de backups (sufixo ~)
- `-g` não mostra o dono
- `-o` não mostra grupo
- `-s` mostra o tamanho do arquivo
- `-i` mostra o inode do arquivo
- `--group-directories-first` mostra diretórios primeiro
- `-d` lista o diretório em si, não o conteúdo
- `-r` ordem reversa
- `-S` ordena pelo tamanho (maior primeiro)
- `-t` ordena pelo horário de modificação (mais recente primeiro)
- `-u` ordena pelo horário de acesso (mais recente primeiro)
- `-x` ordena pela extensão
- `--time={type}` ordena por horário ou modifica a ordenação por tempo usado em `--sort=time`: `atime`, `access`, `use`, `ctime`, `status`, `birth` ou `creation`
- `--sort={type}` ordena por tamanho `size`, horário `time`, versão `version`, extensão `extension` ou não ordena `none`
- `--color=never|auto|always` habilita ou desabilita cores

Em script é interessante ver o retorno 0 - sucesso, 1 - falha simples, 2 - falha séria.

### cp

Copia arquivos (e também diretórios)

**Opções úteis:**

- `-a` preserva atributos do arquivos (como modo, dono, horários)
- `-b` faz o backup se o destino estiver para ser sobrescrito
- `-f` se existir no destino, sobrescreva
- `-i` habilite modo windows, pergunte antes
- `-r` em diretórios, também copia todos seus subdiretórios
- `-u` só copia se a origem for mais nova ou caso não exista no destino
- `-v` modo verboso, útil em depuração
- `-S {ch}` informa o sufixo do backup, padrão é ~

### mv

Movimenta ou renomeia arquivos

**Opções úteis:**

- `-b` faz o backup se o destino estiver para ser sobrescrito
- `-f` se existir no destino, sobrescreva
- `-i` habilite modo windows, pergunte antes
- `-u` só copia se a origem for mais nova ou caso não exista no destino
- `-v` modo verboso, útil em depuração

### mkdir

Cria diretórios

**Opções úteis:**

- `-m` define o modo do diretório
- `-p` se houver algum diretório não existente no caminho, cria-os
- `-v` modo verboso, útil em depuração

**Exemplos:**

```bash
# se a ou b não existir falha
mkdir /tmp/a/b/c
# sempre cria a árvore completa de diretórios
mkdir -p /tmp/a/b/c
```

### ps

### kill

### find

### grep

### sed

### awk

### read

### chmod

### chown

### passwd

### touch

Toca no arquivo modificando o `atime` (*access time*) e o `mtime` (*modification time*), ou apenas um deles. Se o arquivo não existir cria um novo vazio.

**Opções úteis:**

- `-a` modifica apenas o *atime*
- `-m` modifica apenas o *mtime*
- `-c` não cria se não existir
- `-d''` define um timestamp arbirtrário

**Exemplos:**

```bash
# cria um arquivo novo, supondo que ele não exista, se existir atualiza o *atime* e o *mtime*
touch arquivonovo.txt
touch -d'2024-01-01 00:00:59' arquivoantigo.txt
# modifica apenas o momento de acesso
touch -a arquivonovo.txt
```