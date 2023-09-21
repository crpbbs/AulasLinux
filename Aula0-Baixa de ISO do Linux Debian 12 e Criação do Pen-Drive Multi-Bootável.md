# Título: Aula0-Baixa de ISO do Linux Debian 12 e Criação de Pen-Drive Multi-Bootável

## Subtítulo: Como Baixar e criar um pen-drive Multi-Bootável

### Introdução:

Bom dia, boa tarde, boa noite.

Seja bem-vindo ao meu canal, onde juntos construíremos habilidades que vão além do sistema operacional.

Aperte o botão de inscrição e venha fazer parte desta jornada emocionante pelo Linux!

Em nosso canal, você encontrará guias passo a passo detalhados sobre a instalação e configuração de várias distribuições, juntamente com dicas úteis para aprimorar sua experiência.

Queremos tornar o Linux acessível a todos, eliminando qualquer confusão que possa surgir ao longo do caminho.

Junte-se a nós enquanto exploramos os aspectos fascinantes do Linux, desde as personalizações elegantes até as poderosas ferramentas de linha de comando. 

Nesta vídeo aula vamos aprender a baixar uma distribuição Linux e a criar um pen-drive multi-bootável para os sistemoas operacionais Debian 12.1, Ubuntu 22.04.3 e kali-linux-2023.3.

Acreditamos que esse pen-drive será de muita valia para todos, pois permitirá instalar várias distribuições Linux e várias versões do Windows em um único pen-drive.

Nesta primeira parte vamos baixar uma imagem ISO do debian 12, uma imagem ISO do Ubuntu 22, uma imagem ISO do Kali e uma ferramenta para criação do pen-drive multibootável chamada YUMI.

* 1 Parte - Baixando as ISOS

    Abra o navegador e acesse o [site oficial do Debian](https://www.debian.org/).

    Neste site vamos encontrar ao lado direito uma opção para BAIXAR o Debian.

    Vamos clicar na opção BAIXAR. Em seguida será aberto a página de download.

    Logo abaixo da frase.

    Obrigado por baixar o Debian!

    Você deverá clicar na opção

    debian-12.1.0-amd64-netinst.iso para fazer o download desta versão.

    A atual versão estável do Debian é a versão 12, codinome bookworm. Ela foi inicialmente lançada como versão 12.0 em 10 de Junho de 2023 e sua última atualização, versão 12.1, foi lançada em 22 de Julho de 2023.

    Usuários(as) podem contar com 3 anos de suporte total para cada lançamento e 2 anos de suporte extra LTS - Long Term Support (Suporte de Longo Termo), totalizando assim 5 anos de suporte, sendo assim os usuários desta versão terão suporte até 2028.

    Portanto, sempre escolha a versão estável, pois garante confiabilidade, durabilidade e estabilidade em seu sistema.

    Vamos mudar agora para [baixar a ISO do Ubuntu](https://ubuntu.com/).

    Neste site vamos encontrar no menu suspenso a opção Download, clique nele depois em Get Ubuntu Desktop, depois em Download 22.04.3.

    O Ubuntu foi introduzido em 2004 pela empresa britânica Canonical. Ele é baseado no Debian. Essa é a versão LTS mais recente do Ubuntu. LTS significa suporte de longo prazo — o que significa cinco anos de atualizações gratuitas de segurança e manutenção, garantidas até abril de 2027.

    Vamos mudar agora para [baixar a ISO do Kali Linux](https://www.kali.org/).

    Neste site clique em DOWNLOAD, depois em Installer Images que é a Recomendada para nós, depois em 64-bit e nos Installer 64, dessa forma você estará baixando o Kali Linux 2023.3 de 64 bits.

    O Kali Linux se baseia no Debian e é uma distribuição muito popular entre estudantes e profissionais de segurança da informação por possuir diversas ferramentas e aplicações nativas especializadas em testes de invasão, penetração, dentre outras. A distribuição possui um arsenal com mais de 300 ferramentas nativas exclusivas para atividades de segurança e pentests.

    Vamos mudar agora para [baixar o YUMI](https://www.pendrivelinux.com/), que será nossa ferramente para criação do Pen-Drive Multibootável.

    Desça até encontrar a palavra YUMI Multiboot USB Creator e clique neste link. Nesta nova página, clique no link (quase no final da página) chamado Donwload YUMI. O download irá começar.

    O YUMI exFAT (BIOS and UEFI USB Boot) é a variante mais recente e sugerida para uso daqui para frente. Ele permite que você continue usando um formato exFAT em sua unidade USB e armazene arquivos maiores que 4 GB. Os modos de inicialização modernos UEFI e Legacy BIOS são suportados. Você também pode arrastar e soltar arquivos ISO inicializáveis ​​em pastas em sua unidade flash para serem detectados automaticamente e adicionados ao menu de inicialização.

* 2 Parte - Preparando um Pen-Drive Multi-Bootável:

    Separe um pen-drive que você não usa para servir ao nosso propósito.

    A ideia de um pen-drive multi-boot, vai permitir instalar várias distribuições a partir do mesmo dispositivo.

    Existem várias ferramentas para se criar um pen-drive bootável.

    Nos vamos utilizar o YUMI. Que cria uma unidade USB Legacy\UEFI versátil e de inicialização múltipla gratuitamente.

    * Plugue o pen-drive na sua USB

        Poderá ver que você baixou um programa chamado YUMI-exFAT-1.0.2.0.exe.
    
    * Execute o programa de instalação como administrador.

    * Clique em I Agree.

        Nesta tela você tem a opção no Step 1 para escolher seu pen-drive. Depois disso ele irá informar que seu pen-drive não está preparado para instalação desta versão do YUMI e se você deseja formatá-lo, clique em SIM. Ele dá uma última chance para você desistir e fazer um backup deste seu pen-drive e avisa que será feita 2 partições uma com o exFAT e outra para o MBR que será a partição de boot do pen-drive. A partição exFAT será a maior e conterá as ISOS que vamos colocar. Então clique em SIM e aguarde a criação. Se tudo der certo ele informa que passará para o Step 2, onde você escolhe o que vai colocar neste pen-drive.

    Caso sua distribuição não apareça na lista utilize a última opção que se chama:

    Try an Unlisted ISO.

    Escolhar a ISO através do Browse e clique em CREATE.

    Ao final da inclusão clique em NEXT.

    Ele perguntará se deseja colocar mais ISOS.

    Responda que sim e continue colocando as ISOS e ferramentas/utilitários que quiser.

    Caso responda que não poderá entrar nele futuramente e colocar mais ISOs.

    Ao final clique em FINISH

    Caso tenha alguma coisa que você veja na lista e você ainda não tenha baixado, escolha ela e clique em Visit the "nome da ISO" Home Page para ir para o site de download.

* 3 Parte - Boot de teste do Pen-Drive

    Por último vamos fazer nosso primeiro boot pelo pen-drive para ver como ele ficou.

    Reinicie seu computador com o pen-drive na USB.

    Em algumas máquinas é necessário apertar o F8 ou F12 para poder escolher como será nosso boot. Então escolha USB.

Conclusão:

Agradeço pela participação e interesse.

Parabenizo você que ficou e assistiu essa vídeo aula até o final e por ter criado um pen-drive multi-bootável com as imagens ISOs que desejou.

Lembre-se de se inscrever no canal e ativar as notificações para futuras aulas sobre Linux e Debian.

Convido a curtir o vídeo e a compartilhar com seus amigos.

Pretendemos continuar trazendo conteúdo valioso sobre Linux e Debian no canal.

# Até a próxima se Deus permitir.