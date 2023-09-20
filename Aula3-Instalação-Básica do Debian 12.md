# Título: Aula3-Instalação-Básica do Debian 12
## Subtítulo: Como Instalar, Configurar e Usar o Docker no Debian 12

**Introdução:**

Bom dia, boa tarde, boa noite.
O objetivo desta vídeo-aula é dar continuidade a aula 2, onde terminamos a instalação básica do debian 12.
Vamos implementar melhorias no aspecto de nosso desktop e em seguida vamos Instalar, Configurar e Usar o Docker no Debian 12.

* 1 Atualização do Sistema:

    ```console
    sudo apt update
    sudo apt upgrade
    ```

* 2 Instalação de Pacotes Essenciais:

    Vamos instalar alguns pacotes essenciais

    O programa **figlet** é um utilitário que nos permite criar alguns banners de texto ASCII incríveis e atraentes. Ele cria letras grandes ou banners de texto ASCII usando texto simples.

    O programa **toilet** ajuda a criar um texto personalizado ou banner, que pode ser usado dentro de um script, por exemplo, ou diretamente na linha de comando.
    ```console
    sudo apt install figlet
    sudo apt install toilet
    Exemplo
    figlet vm-Debian > dist
    cat dist
    toilet --metal --font smmono9 "Bem-Vindo"
    ```

    O programa **file** é utilizado para se determinar qual é o tipo de arquivo informado como parâmetro com base no Magic Number (dois primeiros bytes).

    O programa **tree** lista o conteúdo de um diretório usando o formato de árvore. Ele tem a mesma função do comando ls. A diferença consiste na maneira como as informações são exibidas.

    O programa **xxd** é usado para criar uma representação hexadecimal (hexdump) de um arquivo binário ou para reverter a representação hexadecimal de volta para o formato binário original. Ele pode ser útil para visualizar o conteúdo de arquivos binários ou para realizar operações com dados hexadecimais.

    ```console
    sudo apt install file tree xxd
    ```

* 3 Instalando Zshell

    **Zshell**, também conhecido como **Zsh**, é um interpretador de linha de comando para sistemas Unix-like. Ele é uma poderosa alternativa ao shell padrão (como o **Bash**) e oferece recursos avançados, como autocompletar, correção ortográfica, histórico de comandos melhorado, personalização extensiva e muito mais.

    O **Zsh** é altamente configurável e suporta plugins e temas, permitindo que os usuários personalizem sua experiência de linha de comando de acordo com suas preferências. Ele também possui uma sintaxe semelhante ao Bash, o que facilita a transição para os usuários que estão acostumados com o Bash.

    O **Git** é um sistema de controle de versão distribuído, amplamente utilizado para gerenciar projetos de desenvolvimento de software. Ele permite que várias pessoas trabalhem em um projeto simultaneamente, rastreando todas as alterações feitas nos arquivos ao longo do tempo.

    O **Git** armazena todas as versões de um projeto em um repositório, permitindo que os desenvolvedores acessem qualquer versão específica do código quando necessário. Além disso, ele facilita a colaboração entre membros da equipe, permitindo que eles compartilhem suas alterações e mesclando-as de forma eficiente.

    ```console
    sudo apt install zsh git
    chsh -s $(which zsh)
    ```
    Encerrar a seção e iniciar novamente.

    Entrar no Terminal e escolher a opção (2) para criar o arquivo .zshrc

    Instalando temas para o zsh.

    Baixe o install.sh no github do ohmyzsh com o wget e execute o install.sh

    ```console
    wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
    sh install.sh
    ```

    Edite o arquivo ~/.zshrc e mude a variável ZSH_THEME para o tema que mais lhe agrade. Eu estou usando o tema "bira".

    ```console
    vi ~/.zshrc
    ZSH_THEME="bira"
    ```
    Saia do terminal com exit e entre novamente. Deve estar com esta aparência:

    ![Prompt do zsh com o tema "bira"](imagens/Aula3-Prompt-zsh.jpg)

    Para mais informações sobre o zshc dê uma olhada no site:
    [GitHub do Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh)
    
    No site oficial do [Oh My Zsh](https://ohmyz.sh/), você encontra telas com exemplos dos diversos temas, escolha um e mude no arquivo ~/.zshrc a variável ZSH_THEME para o da sua preferência.

* 4 Desinstalando Oh My Zsh

    Oh My Zsh não é para todos. Sentiremos sua falta, mas queremos tornar esta separação fácil.

    Se você deseja desinstalar oh-my-zsh, basta executar uninstall_oh_my_zsha partir da linha de comando. Ele se removerá e reverterá sua configuração bash anterior ao zsh.



ver site para wallpaper
https://wallpaperswide.com/
sudo apt install dia dia-shapes dia-rib-network
Olhe este site
https://www.xfce-look.org/browse/
>>>Baixar o ocs-url para o Debian 12
>>>opendesktop.org/p/1136805/
>>>Vá para a pasta download e instale com
>>>coloque ocs-url e aperte TAB
sudo apt install ./ocs-url_3.1.0-0ubuntu1_amd64.deb
instalar o tema Goldy-Dark-GTK





1. Configuração de Ambiente de Trabalho:

Vamos arrumar alguns aspectos de aparência do nosso desktop

Vamos configurar o ambiente de trabalho.

4. Configuração de Rede:

Vamos configurar a rede

Conclusão:

Encerramos esta vídeo aula neste momento e na próxima aula iremos apreender sobre docker.

Encerramento:

Agradeço por assistirem essa vídeo aula e caso tenha gostado inscreva-se no canal, curta e compartilhe com seus amigos.
