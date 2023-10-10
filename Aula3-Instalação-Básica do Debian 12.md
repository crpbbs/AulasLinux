# Título: Aula3-Instalação-Básica do Debian 12
## Subtítulo: Como Instalar e Configurar o desktop do debian 12

![](imagens/debian-e-pinguin-121.png)

### Melhorando a aparência
### Introdução

Bom dia, boa tarde, boa noite.

O objetivo desta vídeo-aula é dar continuidade a aula 2, onde terminamos a instalação básica do debian 12.

Vamos implementar melhorias no aspecto visual de nosso desktop e nas próximas aulas abordaremos a instalação e configuração do kvm e posteriormente trataremos do assunto sobre docker e containers.

Configuração de Ambiente de Trabalho:

Vamos arrumar alguns aspectos de aparência do nosso desktop, como resolução do monitor (vamos deixar 1920 x 1080), tamanho dos ícones, tamanho do painel 1, tamanho dos ícones no painel 1, etc...

### 1 Atualização do Sistema:

```console
sudo apt update
sudo apt upgrade
```

### 2 Instalação de pacotes essenciais e complementares:

Vamos instalar alguns pacotes essenciais e complementares

O programa **figlet** é um utilitário que nos permite criar alguns banners de texto ASCII incríveis e atraentes. Ele cria letras grandes ou banners de texto ASCII usando texto simples.

O programa **toilet** ajuda a criar um texto personalizado ou banner, que pode ser usado dentro de um script, por exemplo, ou diretamente na linha de comando.

```console
sudo apt install figlet
sudo apt install toilet
```
Exemplo

```console
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

**Zshell**, também conhecido como **Zsh**, é um interpretador de linha de comando para sistemas Unix-like. Ele é uma poderosa alternativa ao shell padrão (como o **Bash**) e oferece recursos avançados, como autocompletar, correção ortográfica, histórico de comandos melhorado, personalização extensiva e muito mais. O **Zsh** é altamente configurável e suporta plugins e temas, permitindo que os usuários personalizem sua experiência de linha de comando de acordo com suas preferências. Ele também possui uma sintaxe semelhante ao Bash, o que facilita a transição para os usuários que estão acostumados com o Bash. Para mais informações sobre o **Zsh** visite o site https://github.com/zsh-users/zsh.

O **Git** é um sistema de controle de versão distribuído, amplamente utilizado para gerenciar projetos de desenvolvimento de software. Ele permite que várias pessoas trabalhem em um projeto simultaneamente, rastreando todas as alterações feitas nos arquivos ao longo do tempo. O **Git** armazena todas as versões de um projeto em um repositório, permitindo que os desenvolvedores acessem qualquer versão específica do código quando necessário. Além disso, ele facilita a colaboração entre membros da equipe, permitindo que eles compartilhem suas alterações e mesclando-as de forma eficiente.

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

Edite o arquivo ~/.zshrc e mude a variável ZSH_THEME para o tema que mais lhe agrade. Eu estou usando o tema "jonathan".

```console
vi ~/.zshrc
ZSH_THEME="jonathan"
```

Saia do terminal com exit e entre novamente. Deve estar com esta aparência:

Caso use o tema jonathan ficará como esse:

![Prompt do zsh com o tema "jonathan"](imagens/Aula3-Prompt-zsh-jonathan.jpg)

Caso use o tema bira ficará como esse:

![Prompt do zsh com o tema "bira"](imagens/Aula3-Prompt-zsh-bira.jpg)

Para mais informações sobre o o projeto **Oh My Zsh** dê uma olhada no site: <a href="https://github.com/ohmyzsh/ohmyzsh" target="_blank" rel="noreferrer noopener nofollow">https://github.com/ohmyzsh/ohmyzsh</a>

No site sobre temas <a href="https://github.com/ohmyzsh/ohmyzsh/wiki/Themes" target="_blank" rel="noreferrer noopener nofollow">https://github.com/ohmyzsh/ohmyzsh/wiki/Themes</a>, você encontra telas com exemplos dos diversos temas, escolha um e mude no arquivo ~/.zshrc a variável ZSH_THEME para o da sua preferência.

Instalando novos plugins para o **Oh My Zsh** através do site <a href="https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins" target="_blank" rel="noreferrer noopener nofollow">https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins. Vamos escolher e instalar alguns plugins, como por exemplo: web-search

Vamos digitar o seguinte comando:

```console
man zsh
```

Observe que o resultado está em preto e branco.

Agora vamos acrescentar no arquivo .zshrc o plugin colored-man-pages conforme abaixo:

```console
vi .zshrc
plugins=(git colored-man-pages)
```

Digite novamente o mesmo comando anterior para ver como ficou mais destacado.

```console
man zsh
```

O plugin autocomplete do Zsh é uma extensão que fornece recursos de autocompletar ao utilizar o terminal Zsh. Ele permite que você pressione a tecla Tab para completar automaticamente comandos, nomes de arquivos, diretórios e outras informações enquanto você digita. Existem vários plugins de autocomplete disponíveis para o Zsh, mas um dos mais populares é o "**zsh-autosuggestions**". Ele exibe sugestões de comandos com base no histórico de digitação anterior, facilitando a digitação de comandos frequentemente usados.

```console
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

Edite o arquivo .zshrc e inclua no plugins mais este que acabamos de instalar.

```console
vi .zshrc
plugins=(git colored-man-pages zsh-autosuggestions)
```

Carregue novamente o .zshrc com o comando source.

```console
source ~/.zshrc
```

Observe que agora se você digitar algum comando que já tenha digitado antes ele vai sugerir o restante e para aceitar tecle a seta para a direita.

```console
man zsh
```

O plugin zsh-syntax-highlighting é uma ferramenta muito útil para realçar a sintaxe do código enquanto você digita no terminal zsh. Ele destaca erros de sintaxe, palavras-chave, variáveis, comandos e muito mais, ajudando a identificar possíveis erros antes mesmo de executar o código.

```console
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

```console
vi .zshrc
plugins=(git colored-man-pages zsh-autosuggestions zsh-syntax-highlighting)
```

Observe que a partir de agora quando você começa a digitar um comando ele mostra em vermelho e enquanto estiver incompleto ou errado ficará vermelho, assim que estiver certo ele mostra em verde.

O plugin web-search para o Zsh é uma extensão que permite realizar pesquisas na web diretamente do seu terminal Zsh. Com esse plugin, você pode executar pesquisas em mecanismos de busca populares, como Google, Bing, DuckDuckGo, entre outros, sem precisar abrir um navegador. Este plugin já vem pré-instalado então basta editar o arquivo .zshrc e incluir no plugins a palavra web-search.

```console
vi .zshrc
plugins=(git colored-man-pages zsh-autosuggestions zsh-syntax-highlighting web-search)
```

Agora você já pode digitar comandos no teclado como google qualquer coisa ou youtube qualquer coisa que ele abre o navegador e faz a procura.

Desinstalando **Oh My Zsh**

Oh My Zsh não é para todos. Sentiremos sua falta, mas queremos tornar esta separação fácil.

>[!CAUTION]
>
>Se você deseja desinstalar oh-my-zsh, basta executar **uninstall_oh_my_zsha** a partir da linha de comando. Ele se removerá e reverterá sua configuração bash anterior ao zsh.
>
> **Faça isso somente se quiser o bash de volta.**
>
>```console
>uninstall_oh_my_zsha
>```

O **Neofetch** é um utilitário de linha de comando para exibir informações do sistema em sistemas operacionais baseados em Unix, como Linux. Ele exibe informações como distribuição do sistema operacional, versão do kernel, ambiente de desktop, informações da CPU, quantidade de memória, informações da placa gráfica, entre outros. **Neofetch** é altamente personalizável e pode ser configurado para exibir apenas as informações desejadas. Ele também suporta temas e permite que os usuários personalizem a aparência das informações exibidas.

```console
sudo apt install neofetch
```

Inclua o **neofetch** no final do seu arquivo de configuração do shell. Abra o arquivo **/home/$USER/.zsharc** ou no **.bashrc**, caso esteja usando o bash, e inclua na última linha o comando **neofetch**.

```console
vi /home/$USER/.zshrc
```

Saia do shell com exit e entre novamente para ver o efeito do programa neofetch. Deverá ficar como este:

![](imagens/neofetch.png)

## 3 Baixando novos temas e papéis de parede para o Debian

Vamos instalar novos papéis de parede.

Entrar no site <a href="https://wallpaperswide.com/" target="_blank" rel="noreferrer noopener nofollow">wallpaperswide</a> e baixar os papéis de parede que mais lhe agrade.

Agora vamos instalar novos temas.

Vamos instalar o tema Goldy-Dark-GTK, para fazermos isso precisamos primeiramente instalar um utilitário que vai nos ajudar com a instalação dos temas. Então baixaremos o utilitário **ocs-url** para o Debian diretamente do site <a href="https://www.opendesktop.org/p/1136805/" target="_blank" rel="noreferrer noopener nofollow">opendesktop.org/p/1136805/</a>. Estando no site vamos baixar o arquivo <a href="https://ocs-dl.fra1.cdn.digitaloceanspaces.com/data/files/1467909105/ocs-url_3.1.0-0ubuntu1_amd64.deb?response-content-disposition=attachment%3B%2520ocs-url_3.1.0-0ubuntu1_amd64.deb&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=RWJAQUNCHT7V2NCLZ2AL%2F20230922%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230922T223917Z&X-Amz-SignedHeaders=host&X-Amz-Expires=3600&X-Amz-Signature=723d3bc84e575b3920b07e1a0506d075f1a545c5c09871c49e1ed7a8d3afd20d" target="_blank" rel="noreferrer noopener nofollow">ocs-url_3.1.0-0ubuntu1_amd64.deb</a>, sendo que a sua última alteração foi feita em 05/07/2018.
    
Para instalar, vá no diretório que você baixou o arquivo e faça:

```console
sudo apt install ./ocs-url_3.1.0-0ubuntu1_amd64.deb
```

Caso queira poderá instalar outros temas para o Debian e como exemplo vamos baixar o tema Goldy-Dark-GTK e faremos a instalação juntos.

Entrar no site <a href="https://www.xfce-look.org/browse/" target="_blank" rel="noreferrer noopener nofollow">xfce-look.org/browse</a> e baixar os temas que mais lhe agrade.

## 4 Instalando o dia-shapes e o dia-rib-network

**Dia-shapes** é um recurso do software Dia, que é um programa de desenho vetorial disponível para sistemas operacionais Linux. Ele permite criar e manipular diagramas técnicos, fluxogramas, organogramas e outras representações gráficas.

**Dia-Rib-Network** é uma ferramenta de simulação de redes em tempo real para o sistema operacional Linux. Ela permite a criação e configuração de redes virtuais com múltiplos nós e a simulação de tráfego de rede entre eles.

```console
sudo apt install dia dia-shapes dia-rib-network
```

## Conclusão:

Encerramos esta vídeo aula neste momento e na próxima aula iremos apreender sobre kvm.

Agradeço por assistirem essa vídeo aula e caso tenha gostado inscreva-se no canal, curta e compartilhe com seus amigos.

# FIM