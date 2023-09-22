# Título: Aula3-Instalação-Básica do Debian 12
## Subtítulo: Como Instalar, Configurar e Usar o Docker no Debian 12

![](imagens/debian-e-pinguin-121.png)

**Introdução:**

Bom dia, boa tarde, boa noite.

O objetivo desta vídeo-aula é dar continuidade a aula 2, onde terminamos a instalação básica do debian 12.

Vamos implementar melhorias no aspecto visual de nosso desktop e nas próximas aulas abordaremos a instalação e configuração do kvm e posteriormente trataremos do assunto sobre docker.

* 1 Atualização do Sistema:

    ```console
    sudo apt update
    sudo apt upgrade
    ```

* 2 Instalação de pacotes essenciais e complementares:

    Vamos instalar alguns pacotes essenciais e complementares

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

    Instalando **Zshell** e o **Git**

    **Zshell**, também conhecido como **Zsh**, é um interpretador de linha de comando para sistemas Unix-like. Ele é uma poderosa alternativa ao shell padrão (como o **Bash**) e oferece recursos avançados, como autocompletar, correção ortográfica, histórico de comandos melhorado, personalização extensiva e muito mais.

    O **Zsh** é altamente configurável e suporta plugins e temas, permitindo que os usuários personalizem sua experiência de linha de comando de acordo com suas preferências. Ele também possui uma sintaxe semelhante ao Bash, o que facilita a transição para os usuários que estão acostumados com o Bash.

    O **Git** é um sistema de controle de versão distribuído, amplamente utilizado para gerenciar projetos de desenvolvimento de software. Ele permite que várias pessoas trabalhem em um projeto simultaneamente, rastreando todas as alterações feitas nos arquivos ao longo do tempo.

    O **Git** armazena todas as versões de um projeto em um repositório, permitindo que os desenvolvedores acessem qualquer versão específica do código quando necessário. Além disso, ele facilita a colaboração entre membros da equipe, permitindo que eles compartilhem suas alterações e mesclando-as de forma eficiente.

    ```console
    sudo apt install zsh git
    ```

    Para mudar do bash para o zsh faça:

    ```console
    chsh -s $(which zsh)
    ```

    Encerrar a seção e iniciar novamente para que produza efeito.

    Entrar no Terminal e escolher a opção (2) para criar o arquivo .zshrc no home do usuário normal.

    Instalando temas para o zsh.

    Baixe o **install.sh** no **github** do **ohmyzsh** com o **wget** e execute o **install.sh**.

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
    
    <a href="https://github.com/ohmyzsh/ohmyzsh" target="_blank" rel="noreferrer noopener nofollow">GitHub do Oh My Zsh</a>

    No site oficial do <a href="https://ohmyz.sh/" target="_blank" rel="noreferrer noopener nofollow">Oh My Zsh</a>, você encontra telas com exemplos dos diversos temas, escolha um e mude no arquivo ~/.zshrc a variável ZSH_THEME para o da sua preferência.

    Desinstalando **Oh My Zsh**

    Oh My Zsh não é para todos. Sentiremos sua falta, mas queremos tornar esta separação fácil.

    Se você deseja desinstalar oh-my-zsh, basta executar **uninstall_oh_my_zsha** partir da linha de comando. Ele se removerá e reverterá sua configuração bash anterior ao zsh.

    ### Faça isso somente se quiser o bash de volta.

    ```console
    uninstall_oh_my_zsha
    ```

    Instalando o **neofetch**

    ```console
    sudo apt install neofetch
    ```

    Inclua o neofetch no final do seu arquivo de configuração do shell. Abra o arquivo **/home/$USER/.zsharc** e inclua na última linha o comando **neofetch**.

    ```console
    vi /home/$USER/.zshrc
    ```
    Saia do shell com exit e entre novamente para ver o efeito do programa neofetch. Deverá ficar como este:

    ![](imagens/neofetch.png)

    Configuração de Ambiente de Trabalho:

    Vamos arrumar alguns aspectos de aparência do nosso desktop, como tamanho dos ícones, tamanho do painel 1, tamanho dos ícones no painel 1, etc...

* 3 Baixando novos papéis de parede para o Debian

    Entrar no site <a href="https://wallpaperswide.com/" target="_blank" rel="noreferrer noopener nofollow">wallpaperswide</a> e baixar os papéis de parede que mais lhe agrade.

    Instalando novos temas para o Debian

    Entrar no site <a href="https://www.xfce-look.org/browse/" target="_blank" rel="noreferrer noopener nofollow">xfce-look.org/browse</a> e baixar os temas que mais lhe agrade.

    instalar o tema Goldy-Dark-GTK

    Instalando o ocs-url para o Debian

    Entrar no site <a href="https://www.opendesktop.org/p/1136805/" target="_blank" rel="noreferrer noopener nofollow">opendesktop.org/p/1136805/</a> e baixar o ocs-url para o Debian. Atualmente <a href="https://ocs-dl.fra1.cdn.digitaloceanspaces.com/data/files/1467909105/ocs-url_3.1.0-0ubuntu1_amd64.deb?response-content-disposition=attachment%3B%2520ocs-url_3.1.0-0ubuntu1_amd64.deb&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=RWJAQUNCHT7V2NCLZ2AL%2F20230922%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230922T223917Z&X-Amz-SignedHeaders=host&X-Amz-Expires=3600&X-Amz-Signature=723d3bc84e575b3920b07e1a0506d075f1a545c5c09871c49e1ed7a8d3afd20d" target="_blank" rel="noreferrer noopener nofollow">ocs-url_3.1.0-0ubuntu1_amd64.deb</a> última alteração feita em 2018-07-05.
    
    Para instalar, vá no diretório que você baixou o arquivo e faça:

    ```console
    sudo apt install ./ocs-url_3.1.0-0ubuntu1_amd64.deb
    ```

* 4 Instalando o dia-shapes e o dia-rib-network

    **Dia-shapes** é um recurso do software Dia, que é um programa de desenho vetorial disponível para sistemas operacionais Linux. Ele permite criar e manipular diagramas técnicos, fluxogramas, organogramas e outras representações gráficas.

    **Dia-Rib-Network** é uma ferramenta de simulação de redes em tempo real para o sistema operacional Linux. Ela permite a criação e configuração de redes virtuais com múltiplos nós e a simulação de tráfego de rede entre eles.

    ```console
    sudo apt install dia dia-shapes dia-rib-network
    ```

## Conclusão:

Encerramos esta vídeo aula neste momento e na próxima aula iremos apreender sobre kvm.

Encerramento:

Agradeço por assistirem essa vídeo aula e caso tenha gostado inscreva-se no canal, curta e compartilhe com seus amigos.

# FIM