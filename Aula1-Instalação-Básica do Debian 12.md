# Título: Aula1-Instalação básica do Debian 12

## Subtítulo: Como instalar o Debian 12 no modo texto sem nada, apenas o Debian.

### Introdução:

Bom dia, boa tarde, boa noite.

Seja bem-vindo ao meu canal, onde juntos construíremos habilidades que vão além do sistema operacional.

Em nosso canal, você encontrará guias passo a passo detalhados sobre a instalação e configuração de várias distribuições, juntamente com dicas úteis para aprimorar sua experiência.

Queremos tornar o Linux acessível a todos, eliminando qualquer confusão que possa surgir ao longo do caminho.

Junte-se a nós enquanto exploramos os aspectos fascinantes do Linux, desde as personalizações elegantes até as poderosas ferramentas de linha de comando. 

Assim que assistir e gostar por favor deixe seu like e compartilhe este vídeo com seus amigos.

Aproveite e se inscreva no canal para receber novas aulas.

Hoje vou apresentar um vídeo que fiz para servir de tutorial na instalação do Debian 12.

1. Mão na massa

   Nosso propósito é realizar uma instalação limpa e vazia do Debian, sem desktop, sem praticamente todos os aplicativos que viriam numa instalação full.

   Vamos fazer passo a passo a instalação e posteriormente em aulas futuras iremos instalar um desktop enxuto, além disso, iremos instalar somente os aplicativos que iremos utilizar, sem outros componentes desnecessários que são instalados numa instalação full.

   Vamos utilizar um HD vazio.

   Para esta primeira aula iremos instalar o Debian num HD de 40 GB. Deixaremos 512 MB para o BOOT; 20 GB para a partição RAIZ; 18 para a partição HOME e 2 GB para área de SWAP.

2. Boot pelo Pen-Drive ou CD com a imagem do Debian 12

    ```console
    Advanced Options
        Expert Install
            Choose Language = Português do Brasil
                Localidade = Brasil
                Configurar Locales = Brasil - pt_BR.UTF-8
                Configurar Locales - Pressione ENTER
            Configure o teclado = Portugues Brasileiro
            Detectar e montar mídia de instalação = [*] usb-storage (USB storage) <Continuar>
            Carregar componentes a partir da mídia de instalação <Continuar>
            Detectar hardware de rede
            Configurar a rede
                Configurar a rede automaticamente <Sim>
                Tempo de espera = 3 <Continuar>
                Nome da máquina = vm-debian
                Nome do domínimo = casa.local
            Configurar usuário e senhas
                Permiti login como root = <Sim>
                Senha do root = VOCÊ ESCOLHE QUAL VAI USAR
                Repete a mesma senha denovo
                Criar uma conta de usuário normal agora = <Sim>
                Nome completo para o novo usuário = COLOQUE SEU NOME COMPLETO AQUI
                Nome de usuário para sua conta = UM LOGIN DE ACESSO EM MINÚSCULO
                Senha para o novo usuário = VOCÊ ESCOLHE QUAL VAI USAR
                Repete a mesma senha denovo do novo usuário
            Configurar o Relógio
                Ajustar o relógio usando NTP = <Sim>
                Servidor NTP a ser usado = 0.debian.pool.ntp.org <Continuar>
                Estado para definir fudo horário = São Paulo
            Detectar discos
            Particionar discos
                Manual
                    Disco virtual 1 (vda) - 42.9 GB Virtio Block Device
                        Criar nova tabela de partição vazia neste dispositivo = <Sim>
                            Tipo = msdos
                            Selecione pri/log 42.9 GB ESPAÇO LIVRE <ENTER>
                                Criar uma nova partição
                                    Novo tamnho da partição = 512 MB
                                    Tipo da nova partição = Primária
                                    Localização para a nova partição = Início
                                    Usar como = Sistema de arquivos com "journaling" ext4
                                    Ponto de montagem = /boot
                                    Opções de montagem = marque os 3 primeiros
                                    Rótulo = Boot
                                    Blocos reservados = 0
                                    Finalizar a configuração da partição
                            Selecione pri/log 42.4 GB ESPAÇO LIVRE <ENTER>
                                Criar uma nova partição
                                    Novo tamnho da partição = 20 GB
                                    Tipo da nova partição = Primária
                                    Localização para a nova partição = Início
                                    Usar como = Sistema de arquivos com "journaling" ext4
                                    Ponto de montagem = /
                                    Opções de montagem = marque os 3 primeiros
                                    Rótulo = Raiz
                                    Blocos reservados = 0
                                    Finalizar a configuração da partição
                            Selecione pri/log 22.4 GB ESPAÇO LIVRE <ENTER>
                                Criar uma nova partição
                                    Novo tamnho da partição = 20.4 GB
                                    Tipo da nova partição = Primária
                                    Localização para a nova partição = Início
                                    Usar como = Sistema de arquivos com "journaling" ext4
                                    Ponto de montagem = /home
                                    Opções de montagem = marque os 3 primeiros
                                    Rótulo = Raiz
                                    Blocos reservados = 0
                                    Finalizar a configuração da partição
                            Selecione pri/log 2.0 GB ESPAÇO LIVRE <ENTER>
                                Criar uma nova partição
                                    Novo tamnho da partição = 2.0 GB
                                    Tipo da nova partição = Primária
                                    Usar como = Área de troca (swap)
                                    Finalizar a configuração da partição
                            Finalizar o particionamento e escrever as mudanças no disco
                Escrever as mudanças nos discos? = <Sim>
            Instalar o sistema básico
                Kernel a ser instalado = linux-image-amd64
                Direcionado: Só inclui drivers necessários para este sistema
            Configurar o gerenciador de pacotes
                Ler mídia de instalação adicional = <Não>
                Usar um espelho de rede = <Sim>
                Procolo para baixar arquivos = http
                País do espelho do repositório = Brasil
                Espelho do respositório Debian = deb.debian.org
                Informação sobre proxy HTTP (deixe em branco para nenhum) = <Continuar>
                Usar firmware não-livre? = <Sim>
                Usar programas não-livres ("non-free")? = <Sim>
                Habilitar repositório fonte no APT? = <Sim>
                Serviços a serem usados (faça como abaixo):
                    [*] atualizações de segurança (de security.debian.org)
                    [*] atualizações da distribuição
                    [ ] software portado de versões mais novas
            Selecionar e instalar software
                Configurando discover:
                    Gerenciamento de atualizações neste sistema:
                        Sem atualizações automáticas
                Configurando popularity-contest
                    Participar do concurso de utilzações de pacotes ? <Sim>
                Seleção de software (desmarque todos, como abaixo):
                    [ ] ambiente de área de trabalho no Debian
                    [ ] ... GNOME
                    [ ] ... Xfce
                    [ ] ... GNOME Flashback
                    [ ] ... KDE Plasma
                    [ ] ... Cinnamon
                    [ ] ... MATE
                    [ ] ... LXDE
                    [ ] ... LXQt
                    [ ] servidor web
                    [ ] servidor SSH
                    [ ] utilitários de sistema padrão
            Instalar o carregador de inicialização GRUB
                Executar os-prober automaticamente para detectar outros sistemas = <Não>
                Instalar o carregador de inicialização GRUB no disco primário ? = <Sim>
                    Dispositivo no qual instalar o carregador de inicialização:
                        /dev/vda
            Finalizar a instalação
                O relógio do sistema está configurado para UTC ? <Sim>
                Por favor, escolha continuar para reinicializar o computador <Continuar>
    ```

3. Primeiro boot no novo sistema já instalado

    Entrar com seu usuário normal e tentar fazer um apt update

    ```console
    apt update
    ```

    ```console
    df -h
    ```
    
    ```console
    free -h
    ```

    Sair com o exit e voltar como root

    ```console
    exit
    ```

    Agora sim vamos fazer um apt update para ver se tem atualizações.

    ```console
    apt update
    ```

    ```console
    apt upgrade
    ```
    
    ```console
    ifconfig
    apt install net-tools
    ifconfig
    ```

    ```console
    halt -p
    ```

# FIM DA AULA