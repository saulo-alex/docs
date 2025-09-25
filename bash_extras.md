# Dicas de ouro de Bash

1. Sempre expanda variáveis usando aspas-duplas a menos que queira explicitamente que a quebra de palavras ocorra.

2. Para atribuir nulo a uma variável use `var= ` (ou seja, atribua-lhe branco).

3. Quebra de palabras só ocorre se estiver fora de aspas-duplas e após expansão de parâmetros, substituição de comandos ou expansão aritmética, apenas!

4. Na quebra de palavras é usado o conteúdo de $IFS (que é por padrão brancos: espaço, nova-linha e tabulação).

5. A ordem das expansões do Bash é: expansão de chaves; expansão de til; expansão de parâmetros; expansão aritmética; substituição de comandos; quebra de palavras; globbing; remoção de aspas.

6. Como regra geral, aspas-duplas previne o comportamento de qualquer expansão!
