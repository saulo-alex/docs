# Tmux (Terminal Multiplexer)

## Conceitos

Tmux é um multiplexador de terminal, ou seja, ele permite acessar, criar, gerenciar multiplas sessões abertas de terminal em uma única janela.

As principais estruturas são:

- **Session**: representa uma coleção única de pseudo terminais (pty) gerenciadas pelo Tmux

- **Window**: representa uma tela virtual (a porção total de trabalho do terminal que chamou o tmux) dentro de uma **session**.

- **Pane**: representa porções independentes de uma **window** que agem como se fossem *windows*.

- **Instance**: representa um processo em execução do Tmux no sistema operacional

É possível ter múltiplas *instances* abertas conectadas a uma mesma *session*.

É possível ter múltiplas *windows* apresentadas em uma mesma *session*.

```
                    (nível dos processos)
-------------------------------------------------------------------
            Instance  Instance  Instance Instance
-------------------------------------------------------------------
               \        /          \        /
                \      /            \      /
                 \    /              \    /
                  \  /                \  /
                   \/                  \/
                      (nível do tmux)
-------------------------------------------------------------------
                 Session           Session
-------------------------------------------------------------------
                   / \                |
                  /   \               |
-------------------------------------------------------------------
               Window       Window      Window
-------------------------------------------------------------------
               /  |          /   \          \
              /   |         /     \          \
             /    |        /       \          \
-------------------------------------------------------------------
       Pane    Pane      Pane      Pane        Pane 
-------------------------------------------------------------------
```

**Opções úteis**:

- `-2` habilita suporte a 256 cores
- `-f` especifica uma arquivo de configuração à parte (ao invés do $HOME/.tmux.conf)

**Atalhos úteis**:

- `:`   informa um comando
- `z`   dá zoom na pane
- `C-o` rotaciona panes no sentido anti-horário mantendo foco da atual
- `M-o` rotaciona panes no sentido horário mantendo foco da atual
- `{`   rotaciona a pane no sentido horário perdendo foco da atual
- `}`   rotaciona a pane no sentido anti-horário perdendo foco da atual
- `"`   abre uma nova pane horizontalmente
- `%`   abre uma nova pane verticalmente
- `$`   renomeia a session
- `,`   renomeia a window
- `&`   mata window
- `x`   mata pane
- `c`   cria uma window
- `w`   seleciona uma window
- `!`   transforma a pane em window
- `;`   vai para pane previamente selecionada
- `0-9` vai para window referenciada pelo index
- `l`   vai para window previamente selecionada
- `n`   vai para a próxima window
- `p`   vai para a window anterior
- `[`   entra no modo de cópia
- `]`   cola o buffer mais recente de cópia
- `d`   desconecta, mantendo intacta a session
- `f`   busca de texto na window
- `i`   sobre session
- `q`   sobre panes
- `t`   mostra hora
- `~`   mostra última messagem no tmux (útil para saber qual comando)
- `SPACE` comuta entre layouts de panes
- `C-up` `C-down` `C-left` `C-right` redimensiona em 1 célula o pane
- `M-up` `M-down` `M-left` `M-right` redimensiona em 5 células o pane

**Comandos úteis**:

- `set clock-mode-colour {color}` modifica a cor do modo relógio
- `set status-style fg={color},bg={color}` modifica a cor do status
- `set pane-active-border-style fg={color},bg={color}` modifica a cor da barra de pane ativa
- `set pane-border-lines {single|double|heavy|simple|number}` modifica o tipo da barra das panes
- `set message-style fg={color},bg={color}` modifica a cor das mensagens
- `set mode-keys {vi|emacs}` modifica o modo de teclas (padrão emacs)
