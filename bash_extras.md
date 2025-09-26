# Dicas de ouro de Bash

1. Sempre expanda variáveis usando aspas-duplas a menos que queira explicitamente que a quebra de palavras ocorra.

2. Para atribuir nulo a uma variável use `var= ` (ou seja, atribua-lhe branco).

3. Quebra de palabras só ocorre se estiver fora de aspas-duplas e após expansão de parâmetros, substituição de comandos ou expansão aritmética, apenas!

4. Na quebra de palavras é usado o conteúdo de $IFS (que é por padrão brancos: espaço, nova-linha e tabulação).

5. A ordem das expansões do Bash é: expansão de chaves; expansão de til; expansão de parâmetros; expansão aritmética; substituição de comandos; quebra de palavras; globbing; remoção de aspas.

6. Como regra geral, aspas-duplas previne o comportamento de qualquer expansão!

7. Em expressões lógicas `<`, `>`, `==` e suas combinações avaliam sempre no contexto textual e `-lt`, `-gt`, `-eq` e suas combinações avaliam sempre no contexto numérico.

8. Para operações com regex (estilo Perl) só é possível com `=~` e usando o comando `[[`; além disso a expressão regular deve ser escrita *como é*, sem uso de aspas (a menos que queira casar também com elas!)

9. Não existe um operador oposto de `=~` (que funcione na negação); deve ser usado o `!` antes da comparação.

10. Quatro formas de se escapar:

    - Aspas simples `' '` - nada é interpolado ou avaliado

    - Aspas duplas `" "` - variáveis são interpoladas e caracteres de controle são avaliados (`\n` *expande* como nova linha)

    - Contrabarra `\` - nada é interpolado ou interpretado no caractere imediatamente seguinte à barra

    - Aspas simples com dólar `$' '` - variação de aspas simples: nada é interpolado porém avalia caracteres de controle mas sem expandir seu resultado (`\n` é compreendido como uma nova linha mas não é *expandido* na string); na prática é útil apenas quando é usado a string para atribuir à alguma variável como por exemplo o $IFS (que é definido como `IFS=$' \n\t'`)

11. `$" "` traduz a string usando locale!
