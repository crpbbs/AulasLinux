# Como instalar e Configurar Nextcloud no Debian 12

Nextcloud é um software de código aberto para a criação de armazenamento de arquivos públicos e privados. Ele permite que você crie seus serviços auto-hospedados como Dropbox, Google Drive ou Mega.nz. Inicialmente, foi criado pelo desenvolvedor original do owncloud, Frank Karlitschek. Em 2016, ele bifurcou o projeto Owncloud e criou um novo projeto com o novo nome "Nextcloud"

Até agora, o projeto Nextcloud está crescendo rapidamente e se tornando mais do que um software de hospedagem de arquivos. É mais como uma plataforma de sincronização de arquivos e colaboração de conteúdo. Apoiado por muitos plug-ins, Nextcloud se tornou um software de colaboração muito poderoso. Você pode instalar plug-ins para gerenciamento de projetos, videoconferência, edição colaborativa, anotações, cliente de e-mail, etc.

Este tutorial mostrará como instalar o Nextcloud em um servidor Debian 12. Você instalará o Nextcloud com servidor web Apache2, servidor MariaDB e PHP 8.2. Além disso, você também protegerá sua instalação com certificados UFW (Firewall Descomplicado) e SSL/TLS da Letsencrypt.

## Pré-requisitos
Para concluir este guia, certifique-se de ter o seguinte:

Um servidor Debian 12 com pelo menos 4 GB de memória e 2 CPUs.
Um usuário não root com privilégios de administrador.
Um nome de domínio apontado para o endereço IP do servidor.

## Instalando o servidor web Apache2

Na primeira etapa, você instalará o servidor web Apache2 que será usado para executar o Nextcloud.

Primeiro, atualize o índice do seu pacote Debian através do comando apt update abaixo. Quando terminar, você obterá as informações mais recentes do pacote que permitem instalar a versão mais recente dos pacotes.

```console
sudo apt update
sudo apt upgrade
```

Agora digite o seguinte comando apt install para instalar o servidor web Apache. Insira y para confirmar quando solicitado e pressione ENTER para prosseguir com a instalação.

```console
sudo apt install apache2 apache2-utils
```

![Imagem da instalação do apache2](imagens/Aula7-NextCloud-01.png)

Após a instação, habilitamos o mod_rewrite do Apache que é muito utilizado. Este é um módulo que utiliza um mecanismo baseado em regras de reescrita. (phpipa, wordpress todos usam), e o mod_headers, este módulo fornece diretivas para controlar e modificar os cabeçalhos de solicitação e resposta HTTP. Comando para habilita-lo:

```console
sudo a2enmod rewrite
sudo a2enmod headers
sudo systemctl restart apache2
```

Agora execute os comandos systemctl abaixo para verificar o status do serviço apche2.

```console
sudo systemctl is-enabled apache2
sudo systemctl status apache2
```

A saída habilitada deve indicar que o serviço Apache2 será iniciado automaticamente na inicialização do sistema. E o status ativo (em execução) confirma que o serviço Apache2 está em execução.

![Imagem do apache2 em execução](imagens/Aula7-NextCloud-02.png)

Já pode-se acessar no servidor apache pelo http://localhost ou http://nome_do_servidor_na_rede.

![Imagem do apache2 em execução](imagens/Aula7-NextCloud-02b.png)

A página que vimos ao abrir o ip do nosso servidor no navegador fica no diretório /var/www/html, isso está sendo informado no arquivo default do apache que fica em /etc/apache2/sites-enabled/000-default.conf, e para que nosso mod_rewrite e headers funcione corretamente será necessário adicionar alguma linhas.

O HTTP Strict Transport Security ou HSTS (RFC 6797) é um novo padrão de segurança SSL aprovado recentemente pelo IETF. Ele traz diversas melhorias para o SSL como forçar a utilização do HTTPS impedindo que sites sejam acessados usando o protocolo HTTP ou que partes do código de um site que está usando HTTPS seja executado em servidores usando o HTTP entre outras.

```console
sudo vi /etc/apache2/sites-enabled/000-default.conf
```

Adicione abaixo de “DocumentRoot /var/www/html” o seguinte:

```console
    Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains"

    <Directory /var/www/html/>
        Options FollowSymLinks
        AllowOverride All
    </Directory>
```

Ou pode-se executar o comando abaixo para incluir o texto automaticamente:

```console
sudo cp /etc/apache2/sites-enabled/000-default.conf /etc/apache2/sites-enabled/000-default.conf.bak

sudo awk 'NR==14 {print "\tHeader always set Strict-Transport-Security \"max-age=63072000; includeSubDomains\"\n\n\t<Directory /var/www/html/>\n\t\tOptions FollowSymLinks\n\t\tAllowOverride All\n\t</Directory>\n"} 1' /etc/apache2/sites-enabled/000-default.conf.bak > /etc/apache2/sites-enabled/000-default.conf
```

A aparência deverá ser esta:

![Imagem do apache2 em execução](imagens/Aula08-00-Server-Lamp-01.png)

## Instalando o UFW

Após a instalação do Apache2, você instalará o UFW (Uncomplicated Firewall) e abrirá portas para OpenSSH, HTTP e HTTPS. Você configurará o UFW como firewall padrão em seu servidor Debian.

Instale o pacote ufw em seu servidor Debian através do comando apt install abaixo. Insira y para confirmar a instalação e pressione ENTER para continuar.

```console
sudo apt install ufw
```

![Imagem da instalação do ufw](imagens/Aula7-NextCloud-03.png)

Após a instalação do ufw, execute os comandos ufw abaixo para permitir o serviço ssh e habilitar o ufw.

```console
sudo ufw allow OpenSSH
sudo ufw enable
```

Insira y quando solicitado a iniciar e ativar o serviço ufw. Se for bem-sucedido, você deverá obter uma saída " O Firewall está ativo e ativado na inicialização do sistema ".

![Imagem da pergunta sobre firewall ativado e SSH](imagens/Aula7-NextCloud-04.png)

Com o ufw em execução, você deve adicionar portas HTTP e HTTPS que o servidor web Apache2 usará.

Execute o comando ufw abaixo para obter a lista de perfis de aplicativos disponíveis no ufw. Você deverá ver perfis como OpenSSH para serviço ssh e WWW Full para servidor web Apache2, ambos protocolos HTTP e HTTPS.

```console
sudo ufw app list
```

Agora execute o seguinte comando para adicionar e ativar o perfil WWW Full e recarregar o ufw para aplicar as alterações.

```console
sudo ufw allow "WWW Full"
sudo ufw reload
```

Por último, execute o comando ufw status abaixo para verificar as regras habilitadas no ufw. Certifique-se de ter o perfil WWW Full habilitado, o que significa que as portas HTTP e HTTPS estão abertas.

```console
sudo ufw status
```

![Imagem das portas abertas no ufw](imagens/Aula7-NextCloud-05.png)

## Installing PHP 8.2

O Debian 12 Bookwork mais recente vem com pacotes PHP 8.2 por padrão, que é a versão PHP recomendada para instalação do Nextcloud. Agora, você instalará os pacotes PHP 8.2 e configurará o PHP para a instalação do Nextcloud. Você também habilitará o PHP Opcache que será usado como cache de memória para Nextcloud.

Execute o comando apt install abaixo para instalar pacotes PHP em seu sistema Debian. O comando instalará o PHP e algumas extensões necessárias ao Nextcloud, como GD, MySQL, Imagick, pear e apcu. Verifique a página de requisitos do servidor Nextcloud para obter a lista completa de pacotes que você precisa.

```console
sudo apt install php libapache2-mod-php libmagickcore-dev
sudo apt install php-{common,mysql,xml,xmlrpc,curl,gd,imagick,cli,dev,imap,mbstring,opcache,soap,zip,intl,pear,gmp,bcmath,json,bz2,apcu}
```

Insira y/s para confirmar a instalação e pressione ENTER para continuar.

![Imagem da instalação do php-8.2](imagens/Aula7-NextCloud-06.png)

Após a instalação do PHP, verifique a versão do PHP e as extensões PHP habilitadas usando o comando abaixo.

```console
php --version
php -m
```

Você deverá ver que o PHP 8.2 está instalado com extensões habilitadas, como GD, MySQL, Imagick, xml e zip.

![Imagem sobre a versão do php e as extensões instaladas](imagens/Aula7-NextCloud-07.png)

Em seguida, execute o comando do editor vim abaixo para abrir o arquivo de configuração PHP /etc/php/8.2/apache2/php.ini .

```console
sudo vi /etc/php/8.2/apache2/php.ini
```

Remova o comentário do parâmetro date.timezone e insira o fuso horário adequado para PHP.

```console
date.timezone = America/Sao_Paulo
```

Aumente o valor padrão dos parâmetros memory_limit, upload_max_filesize, post_max_size e max_execution_time . Altere o valor conforme necessário.

```console
memory_limit = 512M
upload_max_filesize = 1024M
post_max_size = 600M
max_execution_time = 300
```

Habilite file_uploads e allow_url_fopen alterando o valor padrão para On .

```console
file_uploads = On
allow_url_fopen = On
```

Desative os parâmetros display_errors e output_buffering alterando o valor padrão para Off .

```console
display_errors = Off
output_buffering = Off
```

Remova o comentário do parâmetro zend_extension e altere o valor para opcache. Isso habilitará o PHP OPcache, que é necessário para Nextcloud.

```console
zend_extension=opcache
```

Adicione as seguintes linhas à seção [opcache] ou localize e altere estas variáveis, descomentando as mesmsas. A configuração OPCache é recomendada pela Nextcloud.

```console
opcache.enable = 1
opcache.interned_strings_buffer = 8
opcache.max_accelerated_files = 10000
opcache.memory_consumption = 128
opcache.save_comments = 1
opcache.revalidate_freq = 1
```

Salve o arquivo e feche o editor quando terminar.

Por último, digite o comando systemctl abaixo para reiniciar o serviço Apache2. Cada vez que você fizer alterações na configuração do PHP, reinicie o serviço Apache2 para aplicar as alterações feitas.

```console
sudo systemctl restart apache2
```

## Instalando o Servidor MariaDB

Após instalar o servidor web Apache2 e PHP 8.2, você instalará o servidor MariaDB que será usado como banco de dados para Nextcloud e configurará a senha root do MariaDB por meio do utilitário mariadb-secure-installation.

Instale o servidor MariaDB através do comando apt install abaixo. Insira y quando solicitado e pressione ENTER para prosseguir com a instalação

```console
sudo apt install mariadb-server
```

![Imagem de instalação do mariadb-server](imagens/Aula7-NextCloud-08.png)

Depois que o MariaDB estiver instalado, insira os seguintes comandos systemctl para verificar o serviço mariadb.

```console
sudo systemctl is-enabled mariadb
sudo systemctl status mariadb
```

A saída habilitada indica que o serviço mariadb será executado automaticamente na inicialização do sistema. E a saída ativa (em execução) deve indicar que o serviço mariadb está em execução.

![Imagem do mariadb em execução](imagens/Aula7-NextCloud-09.png)

Agora que o servidor MariaDB está em execução, você deve proteger a instalação do MariaDB, e isso pode ser feito através do utilitário mariadb-secure-installation . O comando mariadb-secure-installation ajuda a configurar a senha root e a autenticação do MariaDB e ajuda a remover o teste de banco de dados padrão do usuário anônimo.

Execute o comando mariadb-secure-installation para proteger seu servidor MariaDB.

```console
sudo mariadb-secure-installation
```

Durante o processo, você deve inserir Y para concordar e aplicar a configuração ao MariaDB, ou inserir n para discordar e deixar a configuração como padrão. Abaixo estão algumas configurações do MariaDB que serão solicitadas:

* Pressione ENTER quando for solicitada a senha root do MariaDB.
* Insira n quando questionado sobre o método de autenticação unix_socket.
* Insira Y para configurar uma nova senha para o usuário root do MariaDB. Em seguida, insira a nova senha e repita.
* Insira Y para remover o usuário anônimo padrão do MariaDB.
* Em seguida, insira Y novamente para desabilitar o login remoto para o usuário root do MariaDB.
* Insira Y para remover o teste de banco de dados padrão do MariaDB.
* Por último, insira Y novamente para recarregar os privilégios da tabela e aplicar as alterações.

Com isso, o servidor MariaDB está instalado e protegido.

Conforme abaixo:

```console
NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): (SOMENTE PRESSIONE ENTER SEM NADA)
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Criando Banco de Dados e Usuário

Após instalar o servidor MariaDB, agora você criará um novo banco de dados e usuário para Nextcloud. Para conseguir isso, você deve fazer login no servidor MariaDB através do cliente mariadb.

Faça login no servidor MariaDB usando o comando do cliente mariadb abaixo. Insira a senha root do MariaDB quando solicitado.

```console
sudo mariadb -u root -p
```

Uma vez logado no MariaDB, execute as seguintes consultas para criar um novo banco de dados Mariadb e usuário para Nextcloud. Neste exemplo, você criará um novo banco de dados nextcloud_db e o usuário nextclouduser com a senha StrongPassword. Certifique-se de alterar a senha StrongPassword por uma nova senha.

```console
CREATE DATABASE nextcloud_db;
CREATE USER nextclouduser@localhost IDENTIFIED BY 'StrongPassword';
GRANT ALL PRIVILEGES ON nextcloud_db.* TO nextclouduser@localhost;
FLUSH PRIVILEGES;
```

![Imagem criando a base de dados e o usuário para mariadb](imagens/Aula7-NextCloud-10.png)

Ainda dentro do mariadb e por último, execute a consulta a seguir para garantir que o usuário nextclouduser possa acessar o banco de dados nextcloud_db .

```console
SHOW GRANTS FOR nextclouduser@localhost;
```

Se tudo correr bem, você deverá ver que o usuário nextclouduser tem privilégios para o banco de dados nextcloud_db .

![Imagem vendo os privilégios do usuário criado](imagens/Aula7-NextCloud-11.png)

Digite quit para sair do servidor MariaDB e conclua esta seção.

```console
quit
```

## Baixando o código-fonte do Nextcloud

Neste ponto, todos os pacotes de software para executar o Nextcloud estão instalados. Agora você fará o download da versão mais recente do código-fonte do Nextcloud e a instalará. Verifique a página de download do Nextcloud antes de começar para obter informações sobre a versão mais recente do Nextcloud.

Antes de baixar o código-fonte do Nextcloud, execute o comando apt install abaixo para instalar o curl e descompactar.

```console
sudo apt install curl unzip
```

![Imagem mostrando a instalação do curl e do unzip](imagens/Aula7-NextCloud-12.png)

Vá para o diretório /var/www e baixe o código-fonte do Nextcloud por meio do comando curl abaixo. Visite a página de download do Nextcloud para obter a versão mais recente do Nextcloud.

```console
cd /var/www/html
sudo curl -o nextcloud.zip https://download.nextcloud.com/server/releases/latest.zip
```

![Imagem do downloado do nextcloud](imagens/Aula7-NextCloud-13.png)

Quando baixei a última versão era a nextcloud-27.1.2.zip, mas baixei a latest.zip mesmo.

Agora extraia o arquivo nextcloud.zip por meio do comando unzip e, em seguida, altere a propriedade do diretório nextcloud para www-data .

```console
sudo unzip nextcloud.zip
sudo chown -R www-data:www-data nextcloud
```

Com isso, você deve notar que o diretório raiz do documento para instalação do Nextcloud é o diretório /var/www/html/nextcloud . E o servidor web Apache2 pode acessar o código-fonte do nextcloud por meio do usuário www-data .

## Configurando o host virtual Apache2

Após baixar o código-fonte do Nextcloud, você deve criar a nova configuração do host virtual Apache2 que será usada para executar o Nextcloud. Certifique-se de ter o nome de domínio apontado para o endereço IP do servidor Debian para a instalação do Nextcloud.

Crie uma nova configuração de host virtual Apache2 /etc/apache2/sites-available/nextcloud.conf usando o comando nano abaixo.

```console
sudo vi /etc/apache2/sites-available/nextcloud.conf
```

Altere o nome de domínio no parâmetro ServerName com seu domínio e o caminho completo do log para os parâmetros ErrorLog e CustomLog.

```console
<VirtualHost *:80>
    ServerAdmin admin@linuxlab.blog.br
    ServerName nextcloud.linuxlab.blog.br
    DocumentRoot /var/www/html/nextcloud/

    Alias /nextcloud "/var/www/html/nextcloud/"

    # log files
    ErrorLog /var/log/apache2/files.linuxlab.blog.br-error.log
    CustomLog /var/log/apache2/files.linuxlab.blog.br-access.log combined

    <Directory /var/www/html/nextcloud/>
        Options +FollowSymlinks
        AllowOverride All

        <IfModule mod_dav.c>
            Dav off
        </IfModule>

        SetEnv HOME /var/www/html/nextcloud
        SetEnv HTTP_HOME /var/www/html/nextcloud
    </Directory>
</VirtualHost>
```

Quando terminar, salve o arquivo e saia do editor.

Em seguida, execute o comando a2ensite abaixo para habilitar a configuração do host virtual nextcloud.conf . Em seguida, verifique a configuração geral do Apache2 por meio do comando apachectl abaixo.

```console
sudo a2ensite nextcloud.conf
sudo apachectl configtest
```

![Imagem executando o comando para habilitar o site e ver como está a configuração geral](imagens/Aula7-NextCloud-14.png)

Você deverá ver a saída Sintaxe OK se tiver configurações corretas e adequadas do Apache.

Agora insira o seguinte comando systemctl para reiniciar o serviço Apache2 e aplicar a configuração do host virtual Nextcloud.

```console
sudo systemctl restart apache2
```

Após a reinicialização do Apache2, sua instalação do Nextcloud deverá estar acessível por meio de um protocolo HTTP inseguro.

http://linuxlab.blog.br/nextcloud

Visite o seu nome de domínio Nextcloud e você deverá obter a página de instalação como esta:

![Imagem da tela inicial do nextcloud](imagens/Aula7-NextCloud-15.png)

## Protegendo Nextcloud com certificados SSL/TLS

Para adicionar uma camada de segurança adicional ao seu Nextcloud, você configurará HTTPS na configuração do host virtual Apache2 via Certbot. O Certbot é uma ferramenta de linha de comando para gerar certificados SSL/TLS gratuitos do Letsencrypt e vem com um plugin adicional que permite configurar HTTPS automaticamente para vários servidores web.

Execute o comando apt install abaixo para instalar o plugin Certbot e Certbot Apache. Insira y, quando solicitada a confirmação, e pressione ENTER para continuar.

```console
sudo apt install certbot python3-certbot-apache
```

![Imagem da instalação do cerbot e do python3-cerbot-apache](imagens/Aula7-NextCloud-16.png)

Agora execute o comando certbot abaixo para gerar certificados SSL/TLS para seu nome de domínio Nextcloud e configurar automaticamente HTTPS dentro do host virtual Apache2. Certifique-se de alterar o nome de domínio e o endereço de e-mail no comando a seguir.

```console
sudo certbot --apache --agree-tos --redirect --hsts --staple-ocsp --email crpbbs@yahoo.com.br -d nextcloud.linuxlab.blog.br
```

Após executar este comando você recebe uma mensagem. Eu respondi a mensagem que não, porque a pergunta é sobre eles enviarem e-mails para você sobre o trabalho deles.

```console
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.
Requesting a certificate for nextcloud.linuxlab.blog.br

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/nextcloud.linuxlab.blog.br/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/nextcloud.linuxlab.blog.br/privkey.pem
This certificate expires on 2023-12-30.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for nextcloud.linuxlab.blog.br to /etc/apache2/sites-available/nextcloud-le-ssl.conf
Congratulations! You have successfully enabled HTTPS on https://nextcloud.linuxlab.blog.br

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

Assim que o processo for concluído, o nome de domínio Nextcloud deve ser configurado com HTTPS, que é gerenciado pelo plugin Certbot Apache. E os certificados SSL/TLS estão localizados no diretório /etc/letsencrypt/live/domain-name.com/ .

## Instalando Nextcloud

Nesta seção, você iniciará a instalação do Nextcloud a partir do seu navegador. Neste processo, você também criará o usuário administrador do Nextcloud.

Inicie seu navegador da web e visite o nome de domínio de sua instalação Nextcloud (no meu caso: http://linuxlab.blog.br/nextcloud). Você deverá ser redirecionado automaticamente para uma conexão HTTPS segura e será solicitado a criar um usuário administrador para Nextcloud.

Insira o novo usuário administrador e senha para seu Nextcloud. Você também pode configurar um diretório de dados personalizado ou deixá-lo como padrão.

![Imagem iniciando o nextcloud](imagens/17-create-admin-user.webp)

Em seguida, role até a página inferior e insira os detalhes do nome do banco de dados, usuário e senha. Em seguida, clique em Concluir configuração para concluir a instalação.

![Imagem instalando nextcloud](imagens/18-install-nextcloud.webp)

Assim que a instalação for concluída, você deverá obter a recomendação do Nextcloud para instalar alguns dos aplicativos Nextcloud. Caso queira, poderá instalar os aplicativos recomendados agora.

![Imagem Instalando plugins no nextcloud](imagens/19-skip-recomended-apps.webp)

Agora você deve ver o painel do usuário como este:

![Imagem do painel do nextcloud](imagens/Aula7-NextCloud-20.png)

Agora clique no ícone da pasta para obter o gerenciador de arquivos do Nextcloud.

![Imagem do gerenciador de arquivos do nextcloud](imagens/21-nextcloud-files.webp)

Por último, clique no ícone do usuário no menu da direita e selecione Configurações de administração .

![Inagem de configurações de administração](imagens/22-administration-settings.webp)

Na seção Administração, clique em Visão geral. Você deve obter informações sobre sua versão do Nextcloud e algumas recomendações que pode aplicar ao seu Nextcloud, incluindo  algumas recomendações de segurança e otimizações de desempenho.

![Imagem da visão geral do nextcloud](imagens/23-recommendation.webp)

## Ajuste básico de desempenho para Nextcloud

Nas etapas a seguir, você adicionará configurações à instalação do Nextcloud habilitando o cache de memória via OPCache e configurando o cron via crontab.

Abra a configuração padrão do Nextcloud /var/www/nextcloud/config/config.php usando o comando do editor nano abaixo.

```console
sudo vi /var/www/html/nextcloud/config/config.php
```

Na seção $CONFIG = array , adicione a nova configuração abaixo para habilitar o cache de memória para Nextcloud.

```console
<?php
$CONFIG = array (
....
  # Additional configuration
  'memcache.local' => '\OC\Memcache\APCu',
);
```

Salve as alterações e feche o arquivo quando terminar.

Em seguida, execute o seguinte comando para criar um novo crontab que será usado para executar o script Nextcloud crontab. O parâmetro -u www-data é usado porque o servidor web Apache2 está rodando sobre esse usuário.

```console
sudo crontab -u www-data -e
```

Escolha o editor que deseja usar 1-nano ou 2-vim.

Adicione a seguinte configuração ao arquivo crontab.

```console
*/5  *  *  *  * php -f /var/www/html/nextcloud/cron.php
```

Salve e saia do arquivo quando terminar.

Verifique a lista crontab do usuário www-data usando o seguinte comando. Certifique-se de ter o script crontab que você adicionou.

```console
sudo crontab -u www-data -l
```

# Conclusão

Está tudo pronto! Você concluiu a instalação do Nextcloud em seu sistema Debian. Você instalou Nextcloud com servidor web Apache2, PHP 8.2 e o servidor de banco de dados MariaDB. Você também protegeu seu Nextcloud com UFW (Uncomplicated Firewall) e certificados SSL/TLS via Certbot e Letsencrypt.

Com toda essa configuração, agora você pode usar o Nextcloud para armazenar seus documentos com segurança ou adicionar armazenamento de dados de terceiros ao seu Nextcloud.