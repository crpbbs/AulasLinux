# Título: Aula2-Instalação-Básica do Debian 12
## Subtítulo: Como Instalar e Configurar o desktop do Debian 12

**Introdução:**

Bom dia, boa tarde, boa noite.

Meu nome é Poletto.

Muito prazer em tê-lo aqui no meu canal.

Hoje vou dar continuidade ao vídeo que fiz para servir de tutorial na instalação do Debian 12.

Assim que assistir e gostar por favor deixe seu like e compartilhe este vídeo com seus amigos.

Aproveite e se inscreva no canal para receber novas aulas.

Nesta vídeo aula instalaremos alguns pacotes necessárias para podermos utilizar um desktop enxuto.

Vamos fazer passo a passo a instalação e posteriormente em aulas futuras iremos instalar e testar o kvm e posteriormente o docker.

O kvm para criarmos máquinas virtuais, e o Docker para apreendermos sobre virtualização não convencional.

Mas vamos deixar esses assuntos para aulas futuras.

Na última vez instalamos apenas o net-tools, para utilizarmos o comando ifconfig e descobrir nosso IP. Na verdade nem precisaríamos do ifconfig.
ifconfig.

Para descobrir nosso ip bastaria dar o comando ip a.

* 1 Instalação de Pacotes Essenciais:

  Temos 2 editores de textos que vem instalado por padrão no Debian. O vi e o nano. Os 2 são bons.

  Mas vamos instalar uma versão mais completa do vi, pois o que vem instalado é a versão vim-tiny (muito reduzida).

  ```console data-lang="shell-session"
  apt update && apt upgrade
  apt install vim
  ```

  O vim é um clone do programa editor de textos vi para Unix de Bill Joy e trouxe melhorias ao seu predecessor.

  Ao longo deste curso iremos utilizar o vim como  editor de texto.

  Vamos instalar o htop que é uma ferramenta multiplataforma, visual e interativa, para visualizar processos e recursos em sistemas unix.

  ```console data-lang="shell-session"
  apt install htop
  ```

  Vamos instalar o bash-completion que é o autocompletar no terminal. É um recurso muito útil e que agiliza o seu trabalho ao digitar comandos longos.

  ```console data-lang="shell-session"
  apt install bash-completion
  ```

  O Debian instalou uma versão do vim bem reduzida então vamos remové-la pois não precisamos mais dela.

  ```console data-lang="shell-session"
  apt remove vim-tiny
  ```

  Vamos dar uma olhada no nosso sources.list para ver como estão os links para repositórios.

  ```console data-lang="shell-session"
  vi /etc/apt/sources.list
  ```

  Para que possamos administrar melhor nossa máquina,
  vamos fazê-la através da rede, portanto usando o ssh
  Instale o SSH através do comando:

  ```console data-lang="shell-session"
  apt update
  apt install openssh-server
  ```

  Para descobrir nosso ip faça:

  ```console data-lang="shell-session"
  ip a
  ```

  No nosso caso o IP é 192.168.122.154.

  Para acessar vamos em outra máquia da rede e faremos:

  ```console data-lang="shell-session"
  ssh carlos@192.168.122.154
  ```

  O sistema informa:

  A autenticidade do host '192.168.122.154 (192.168.122.154)' não pode ser estabelecida.

  A chave ED25519 é SHA256:2d7QUJnfQgHnkf0T6pFxWRk/ZEGIKrN+xOrceqsJV9c.

  Esta chave não é conhecida pela nossa máquina.

  Tem certeza de que deseja continuar se conectando (sim/não/[impressão digital])?

  Respondendo yes o sistema gravará essa chave na nossa máquina e partir daí não emitirá mais este aviso.

  Nesta primeira tentativa ele informa que a chave foi adicionada, mas dá um erro fatal. Isto porque não tinhamos informado a nossa senha. Na próxima conexão o sistema acatará e pedirá a senha.

  Então faça novamente:

  ```console data-lang="shell-session"
  ssh carlos@192.168.122.154
  ```

  Observe que o prompt mudou já informando nossa máquina: vm-debian. Neste momento estamos na máquina como usário normal, para fazer novas alterações precisamos de previlégio root então faremos

  ```console data-lang="shell-session"
  su -
  ```

  Assim mudamos para root.

  Vamos fazer a instalação de alguns pacotes que iremos utilizar.

  Para permitir que nossa usuário normal tenha previlégios de root vamos usar o comando sudo

  ```console data-lang="shell-session"
  apt install sudo
  vi /etc/sudoers
  ```

  Coloque seu USER igual ao do root com ALL em tudo. A minha ficou:

  ```console data-lang="shell-session"
  root    ALL=(ALL:ALL) ALL
  carlos	ALL=(ALL:ALL) ALL
  ```

  Agora saia com exit e exit novamente e entre novamente:

  Agora teste com o comando:

  ```console data-lang="shell-session"
  sudo apt update
  ```

  Como pode ver já está funcional.

  Edite o arquivo /root/.bashrc e faça como mostrado para ter acesso a algumas facilidades. Usando o sudo edite com o vi.
  
  ```console data-lang="shell-session"
  sudo vi /root/.bashrc
  ```

  > INICIO DO ARQUIVO

  ```console data-lang="shell-session"
  # ~/.bashrc: executed by bash(1) for non-login shells.

  # Note: PS1 and umask are already set in /etc/profile. You should not
  # need this unless you want different defaults for root.
  # PS1='${debian_chroot:+($debian_chroot)}\h:\w\$ '
  # umask 022

  # You may uncomment the following lines if you want `ls' to be colorized:
  export LS_OPTIONS='--color=auto'
  eval "$(dircolors)"
  alias ls='ls $LS_OPTIONS'
  alias ll='ls $LS_OPTIONS -l'
  alias l='ls $LS_OPTIONS -lA'
  #
  # Some more alias to avoid making mistakes:
  alias rm='rm -i'
  alias cp='cp -i'
  alias mv='mv -i'
  source /etc/bash_completion
  ```
  > FIM DO ARQUIVO

  Edite também o bash.bashrc em etc com o comando:

  ```console data-lang="shell-session"
  sudo vi /etc/bash.bashrc
  ```

  > INICIO DO ARQUIVO

  ```console data-lang="shell-session"
  # System-wide .bashrc file for interactive bash(1) shells.

  # To enable the settings / commands in this file for login shells as well,
  # this file has to be sourced in /etc/profile.

  # If not running interactively, don't do anything
  [ -z "$PS1" ] && return

  # check the window size after each command and, if necessary,
  # update the values of LINES and COLUMNS.
  shopt -s checkwinsize

  # set variable identifying the chroot you work in (used in the prompt below)
  if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
      debian_chroot=$(cat /etc/debian_chroot)
  fi

  # set a fancy prompt (non-color, overwrite the one in /etc/profile)
  # but only if not SUDOing and have SUDO_PS1 set; then assume smart user.
  if ! [ -n "${SUDO_USER}" -a -n "${SUDO_PS1}" ]; then
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
  fi

  # Commented out, don't overwrite xterm -T "title" -n "icontitle" by default.
  # If this is an xterm set the title to user@host:dir
  #case "$TERM" in
  #xterm*|rxvt*)
  #    PROMPT_COMMAND='echo -ne "\033]0;${USER}@${HOSTNAME}: ${PWD}\007"'
  #    ;;
  #*)
  #    ;;
  #esac

  # enable bash completion in interactive shells
  if ! shopt -oq posix; then
    if [ -f /usr/share/bash-completion/bash_completion ]; then
      . /usr/share/bash-completion/bash_completion
    elif [ -f /etc/bash_completion ]; then
      . /etc/bash_completion
    fi
  fi

  # if the command-not-found package is installed, use it
  if [ -x /usr/lib/command-not-found -o -x /usr/share/command-not-found/command-not-found ]; then
    function command_not_found_handle {
            # check because c-n-f could've been removed in the meantime
                  if [ -x /usr/lib/command-not-found ]; then
        /usr/lib/command-not-found -- "$1"
                    return $?
                  elif [ -x /usr/share/command-not-found/command-not-found ]; then
        /usr/share/command-not-found/command-not-found -- "$1"
                    return $?
      else
        printf "%s: command not found\n" "$1" >&2
        return 127
      fi
    }
  fi
  ```
  > FIM DO ARQUIVO

  Agora para ver o status do servidor ssh faça:

  ```console data-lang="shell-session"
  sudo systemctl status sshd
  ```

  Para parar o ssh faça:

  ```console data-lang="shell-session"
  sudo systemctl stop sshd
  ```

  Para iniciar o ssh faça:

  ```console data-lang="shell-session"
  sudo systemctl start sshd
  ```

  Para reiniciar o ssh faça:

  ```console data-lang="shell-session"
  sudo systemctl restart sshd
  ```

  **Observação: Não faça isso estando conectado via sshd senão vai te derrubar**

  > PARA ENVIAR ARQUIVO VIA SSH

  Estando na sua máquina que não é a que estamos fazendo e no intuito de enviar um arquivo da sua máquina para a máquina que estamos montando faremos o seguinte:

  Vamos criar alguns arquivos para enviar.

  ```console data-lang="shell-session"
  touch teste{1..3}.txt
  ```
  Agora faremos o comando scp com a seguinte sintaxe:

  ```console data-lang="shell-session"
  scp teste{1..3}.txt carlos@192.168.122.154:/home/carlos
  ```
  
  Onde:
  
  * “scp” é o nome da função;

  * teste{1..3}.txt é o nome dos arquivos a enviar;

  * carlos é o usuário do ssh para onde quer enviar os arquivos;

  * o ip após o "@" é o Host do ssh para onde será enviado os arquivos;

  * dois pontos separam o ip do diretório para onde vamos enviar os arquivos, ou seja é o caminho absoluto da pasta onde será colocado os arquivos.

  Agora logue-se novamente lá pra ver se os arquivos que foram:

  ```console data-lang="shell-session"
  ssh carlos@192.168.122.154
  lst -l
  ```
  > FIM DE COMO ENVIAR ARQUIVO VIA SSH

  Agora edite o arquivo /home/carlos/.bashrc e descomente algumas linhas como as do arquivo seguinte:

  ```console data-lang="shell-session"
  vi /home/carlos/.bashrc
  ```

  >INICIO DO ARQUIVO

  ```console data-lang="shell-session"
  # ~/.bashrc: executed by bash(1) for non-login shells.
  # see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
  # for examples

  # If not running interactively, don't do anything
  case $- in
      *i*) ;;
        *) return;;
  esac

  # don't put duplicate lines or lines starting with space in the history.
  # See bash(1) for more options
  HISTCONTROL=ignoreboth

  # append to the history file, don't overwrite it
  shopt -s histappend

  # for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
  HISTSIZE=1000
  HISTFILESIZE=2000

  # check the window size after each command and, if necessary,
  # update the values of LINES and COLUMNS.
  shopt -s checkwinsize

  # If set, the pattern "**" used in a pathname expansion context will
  # match all files and zero or more directories and subdirectories.
  #shopt -s globstar

  # make less more friendly for non-text input files, see lesspipe(1)
  #[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

  # set variable identifying the chroot you work in (used in the prompt below)
  if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
      debian_chroot=$(cat /etc/debian_chroot)
  fi

  # set a fancy prompt (non-color, unless we know we "want" color)
  case "$TERM" in
      xterm-color|*-256color) color_prompt=yes;;
  esac

  # uncomment for a colored prompt, if the terminal has the capability; turned
  # off by default to not distract the user: the focus in a terminal window
  # should be on the output of commands, not on the prompt
  #force_color_prompt=yes

  if [ -n "$force_color_prompt" ]; then
      if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
    # We have color support; assume it's compliant with Ecma-48
    # (ISO/IEC-6429). (Lack of such support is extremely rare, and such
    # a case would tend to support setf rather than setaf.)
    color_prompt=yes
      else
    color_prompt=
      fi
  fi

  if [ "$color_prompt" = yes ]; then
      PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
  else
      PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
  fi
  unset color_prompt force_color_prompt

  # If this is an xterm set the title to user@host:dir
  case "$TERM" in
  xterm*|rxvt*)
      PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
      ;;
  *)
      ;;
  esac

  # enable color support of ls and also add handy aliases
  if [ -x /usr/bin/dircolors ]; then
      test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
      alias ls='ls --color=auto'
      alias dir='dir --color=auto'
      alias vdir='vdir --color=auto'

      alias grep='grep --color=auto'
      alias fgrep='fgrep --color=auto'
      alias egrep='egrep --color=auto'
  fi

  # colored GCC warnings and errors
  #export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

  # some more ls aliases
  alias ll='ls -l'
  alias la='ls -A'
  alias l='ls -CF'

  # Alias definitions.
  # You may want to put all your additions into a separate file like
  # ~/.bash_aliases, instead of adding them here directly.
  # See /usr/share/doc/bash-doc/examples in the bash-doc package.

  if [ -f ~/.bash_aliases ]; then
      . ~/.bash_aliases
  fi

  # enable programmable completion features (you don't need to enable
  # this, if it's already enabled in /etc/bash.bashrc and /etc/profile
  # sources /etc/bash.bashrc).
  if ! shopt -oq posix; then
    if [ -f /usr/share/bash-completion/bash_completion ]; then
      . /usr/share/bash-completion/bash_completion
    elif [ -f /etc/bash_completion ]; then
      . /etc/bash_completion
    fi
  fi
  export HISTTIMEFORMAT="%F-%T "
  ```
  >FIM DO ARQUIVO

  Agora faremos a instalação do xorg que é um servidor de exibição de código aberto que fornece uma infraestrutura para sistemas operacionais baseados em Unix e Linux. Ele permite que os aplicativos gráficos sejam executados em um ambiente de janela, gerenciando a exibição de gráficos, entrada de dispositivos e recursos de aceleração de hardware. Ele fornece uma camada de abstração entre o hardware gráfico e os aplicativos, permitindo que diferentes drivers de dispositivo sejam usados para suportar uma ampla variedade de placas gráficas.

  ```console data-lang="shell-session"
  sudo apt install xorg
  ```

  Vamos instalar o wireshark, o evince, o file-roller, zip, unzip, rar e unrar, sendo que:

  Enquanto o Wireshark é um analisador de protocolos utilizado para monitorar o tráfego de rede.

  O Evince é um visualizador de documentos em formatos PDF e PostScript

  
  O File Roller é um aplicativo que permite você abrir, modificar e criar arquivos compactados, praticamente é um gerenciador de arquivs compactados.

  zip unzip rar e unrar são compactadores e descompactadores de arquivos.

  ```console data-lang="shell-session"
  sudo apt install wireshark evince file-roller zip unzip rar unrar
  ```

  Vamos instalar o menulibre e o celluloid que são:

  O menulibre é um editor de menus para o ambiente de desktop Linux. Ele permite que os usuários personalizem e editem os menus de aplicativos em seus sistemas operacionais Linux de forma fácil e intuitiva.

  O celluloid é um reprodutor de mídia simples que pode reproduzir praticamente todos os formatos de vídeo e áudio. Ele suporta listas de reprodução e controles de media player.

  ```console data-lang="shell-session"
  sudo apt install menulibre celluloid
  ```

  Vamos instalar agora o Libreoffice e alguns pacotes relacionados:

  libreoffice-l10n-pt-br para deixar o nosso libreoffice em portugues.

  libreoffice-help-pt-br um help em portugues.

  hunspell-pt-br corretor gramatical.

  hyphen-pt especifica como as palavras devem ser hifenizadas quando há quebra de texto em múltiplas linhas.

  mythes-pt-br dicionário da língua portuguesa.

  libreoffice-lightproof-pt-br verificador ortográfico da língua portuguesa.

  ```console data-lang="shell-session"
  sudo apt install libreoffice libreoffice-l10n-pt-br libreoffice-help-pt-br hunspell-pt-br hyphen-pt-br mythes-pt-br libreoffice-lightproof-pt-br
  ```

* 2 Instalação do nosso desktop e pacotes complementares

  Vamos instalar o xfce4, xfce4-terminal, xfce4-goodies que são:

  O XFCE4 é um ambiente de desktop leve e de código aberto para sistemas operacionais baseados em Linux. Ele foi projetado para ser rápido, eficiente e fácil de usar, oferecendo uma experiência de desktop completa, mas sem consumir muitos recursos do sistema. Uma das principais vantagens do XFCE4 é sua leveza. Ele consome menos recursos de hardware em comparação com outros ambientes de desktop populares, como GNOME e KDE. Isso o torna uma ótima escolha para computadores mais antigos ou com especificações mais modestas.

  o xfce4-terminal é um emulador de terminal para o ambiente de desktop Xfce no Linux. É uma aplicação leve e fácil de usar que fornece uma interface de linha de comando para executar comandos e interagir com o sistema operacional.

  O xfce4-goodies é um pacote de software adicional para o ambiente de desktop Xfce no Linux. Ele contém uma coleção de aplicativos e plugins que podem ser instalados para aprimorar a funcionalidade e a experiência do usuário no ambiente Xfce. Alguns dos componentes incluídos no pacote xfce4-goodies são: xfce4-mixer: um mixer de áudio para ajustar as configurações de som. xfce4-weather-plugin: um plugin de previsão do tempo para exibir informações meteorológicas na área de trabalho. xfce4-taskmanager: um gerenciador de tarefas para monitorar o uso de recursos do sistema. xfce4-notes-plugin: um plugin para adicionar notas na área de trabalho. xfce4-screenshooter: uma ferramenta para capturar screenshots da área de trabalho. xfce4-power-manager: um gerenciador de energia para controlar as configurações de energia do sistema. Esses são apenas alguns exemplos dos aplicativos e plugins disponíveis no pacote xfce4-goodies. Eles podem ser instalados separadamente ou como parte de um pacote completo, dependendo da distribuição Linux que você está usando.

  ```console data-lang="shell-session"
  sudo apt install xfce4 xfce4-terminal xfce4-goodies
  ```

  Como instalamos o xfce4-terminal não precisamos mais do xterm, então vamos desinstalá-lo com o comando:

  ```console data-lang="shell-session"
  sudo apt remove xterm
  sudo apt autoremove
  ```

  Em adicionar novos itens no painel 1 inclua o menu whisker e retire o menu padrão que vem nele. Faça também alguns ajustes na posição do painel 1 para baixo e elimine o painel 2.

  Vamos instalar o google-chrome

  ```console data-lang="shell-session"
  sudo wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
  sudo apt install ./google-chrome-stable_current_amd64.deb
  sudo apt install -f
  ```

# FIM