# Fazendo um servidor VPN com a pritunl

Links úteis:

https://www.linode.com/

https://pritunl.com/

https://client.pritunl.com/

https://github.com/pritunl/pritunl-client-electron/releases

O Pritunl VPN é uma solução de código aberto para criar e gerenciar servidores VPN. Ele oferece uma interface web fácil de usar e suporta vários protocolos de VPN, como OpenVPN, IPsec, WireGuard, entre outros.

![Site do Pritunl](imagens/Aula6-Site-da-Pritunl.png)

Vamos utilizar o serviço de hospedagem de máquinas da Linode, assim poderemos acessar nosso vpn de qualquer lugar. Siga os seguintes passos:

![Site do Linode](imagens/Aula6-Site-da-Linode.png)

# 1 - Criar um servidor através da linode.com/

Entre no site: https://www.linode.com/pt-br/, mas não entre ainda.

Logar no Linode com a conta do GOOGLE ou criar um cadastro.

Para obter um crédito de $100,00, entrar no site da Linode, utilizando a promoção do Diolinux.

Basta chegar na Linode através do Link = https://www.linode.com/diolinux, que você obterá $100,00 válidos por 60 dias.

![Site da Linode pelo Diolinux](imagens/Aula6-Site-da-Linode-pelo-Diolinux.png)

## 1.1 - Configurando o servidor

Clique na opção Linodes no menu do canto esquerdo.

![Aula6-Criando-Pritunl-00](imagens/Aula6-Criando-Pritunl-00.png)

Depois clique em Create Linode.

![Aula6-Criando-Pritunl-00-b](imagens/Aula6-Criando-Pritunl-00-b.png)

![Aula6-Criando-Pritunl-01](imagens/Aula6-Criando-Pritunl-01.png)

Usuário sudo = carlos

Senha do usuário = Apenas um teste

![Aula6-Criando-Pritunl-02](imagens/Aula6-Criando-Pritunl-02.png)
![Aula6-Criando-Pritunl-03](imagens/Aula6-Criando-Pritunl-03.png)
![Aula6-Criando-Pritunl-04](imagens/Aula6-Criando-Pritunl-04.png)

Senha do root = .8rYx?GV#dVePTZ

![Aula6-Criando-Pritunl-05](imagens/Aula6-Criando-Pritunl-05.png)

Agora clique em Create Linode.

![Aula6-Criando-Pritunl-06-b](imagens/Aula6-Criando-Pritunl-06-b.png)

Para saber o andamento do criação da máquina clique em Launch LISH Console.

![Aula6-Criando-Pritunl-06-c](imagens/Aula6-Criando-Pritunl-06-c.png)

Vai abrir uma nova aba. Aguarde até o término. Demora alguns minutos.
Observe o processamento e a criação.

![Aula6-Criando-Pritunl-07](imagens/Aula6-Criando-Pritunl-07.png)

Durante o período de espera da instalação, é bom instalar o cliente do pritunl no seu computador.

Entre no GitHub da Pritunl pelo site: https://github.com/pritunl/pritunl-client-electron

![Aula6-Criando-Pritunl-07-b](imagens/Aula6-Criando-Pritunl-07-b.png)

E escolha a opção releases

![Aula6-Criando-Pritunl-07-c](imagens/Aula6-Criando-Pritunl-07-c.png)

Depois clique em Show all 35 assets

![Aula6-Criando-Pritunl-07-d](imagens/Aula6-Criando-Pritunl-07-d.png)

Procure na listagem a opção para nosso Debian 12 que é a: pritunl-client-electron_1.3.3637.72-0debian1.bookworm_amd64.deb (ou versão mais nova) e baixe esta versão. Observe que existem client e client-electron. A versão client é só para linha de comando e a versão client-electron é para desktop, então vamos utilizar esta última.

![Aula6-Criando-Pritunl-07-e](imagens/Aula6-Criando-Pritunl-07-e.png)

Caso a versão ainda seja a mesma, poderá fazer na linha de comando, ou alterar a versão para a atual, conforme abaixo:

```console
wget https://github.com/pritunl/pritunl-client-electron/releases/download/1.3.3637.72/pritunl-client-electron_1.3.3637.72-0debian1.bookworm_amd64.deb
```

Para instalar o cliente no Debian 12, primeiro localize onde foi baixado o arquivo e faça conforme abaixo, lembrando de alterar o número da versão para a que você baixou.

```console
cd ~/Downloads
sudo apt install ./pritunl-client-electron_1.3.3637.72-0debian1.bookworm_amd64.deb
```

Nosso cliente já está instalado. Provavelmente você o verá no menu Internet como Pritunl Client. Mas não entre nele ainda.

Voltando ao nosso servidor e quando aparecer a mensagem Installation complete! é porque terminou.

![Aula6-Criando-Pritunl-08](imagens/Aula6-Criando-Pritunl-08.png)

Já pode fechar esta janela. Na verdade nem precisava estar aberta. Foi somente para saber quando o processo termina.

Observe os dados de nosso servidor, bem como o IP do mesmo.

![Aula6-Criando-Pritunl-09](imagens/Aula6-Criando-Pritunl-09.png)

Se copiarmos o IP do nosso servidor e colocarmos no navegador vamos entrar em nosso servidor.

A princípio vai dar este erro, então clicamos no botão Avançado.

![Aula6-Criando-Pritunl-10](imagens/Aula6-Criando-Pritunl-10.png)

Depois clique em Ir para IP_DO_SEU_SERVIDOR/(não seguro)

![Aula6-Criando-Pritunl-11](imagens/Aula6-Criando-Pritunl-11.png)

Vai aparecer esta tela:

![Aula6-Criando-Pritunl-12](imagens/Aula6-Criando-Pritunl-12.png)

Entre em nosso servidor utilizando o SSH que aparece na tela onde está o IP do nosso servidor VPN. Altere o nome do usuário e o número do IP abaixo e coloque o seu.

Mas antes de entrar pelo ssh, vamos entrar pelo Launch LISH Console e fazer uma mudança no arquivo /etc/ssh/sshd_config

![Aula6-Criando-Pritunl-06-c](imagens/Aula6-Criando-Pritunl-06-c.png)

```console
sudo vi /etc/ssh/sshd_config
```

Mude a variável PasswordAuthentication para yes.

```console
PasswordAuthentication yes
```

Em seguinte faça um reboot no sistema.

```console
sudo reboot
```

Demora um pouco para iniciar. Assim que iniciar poderemos fazer acesso pelo ssh.

```console
ssh carlos@45.33.65.57
```

Vai aparecer a mensagem sobre a chave do SSH. Clique em yes e ENTER.

```console
The authenticity of host '45.33.65.57 (45.33.65.57)' can't be established.
ED25519 key fingerprint is SHA256:tlz/oiTGtbeAr3Z+S/S1dX6gADAUXr2JIoO8G+sth9w.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

Estando no servidor faremos o comando que apareceu no login da página do pritunl.

```console
sudo pritunl setup-key
```

```console
[sudo] password for carlos: 
62a6f2fc849148f5a4dbe5b179e9630c
```

Copie o código gerado e coloque na página da web do nosso Pritunl.

![Aula6-Criando-Pritunl-13](imagens/Aula6-Criando-Pritunl-13.png)

Clique em Save. Em seguida aparece um Login. Você também deverá copiar o comando sugerido por ele e colocar no acesso que temos pelo SSH.

```console
sudo pritunl default-password
```

```console
[local][2023-10-03 01:56:44,295][INFO] Getting default administrator password
Administrator default password:
  username: "pritunl"
  password: "ZRqRRU5PsTem"
```

Colocamos o username e o password acima no login do pritunl e clicamos em Sign in.

![Aula6-Criando-Pritunl-14](imagens/Aula6-Criando-Pritunl-14.png)

Entramos em nosso servidor VPN.

![Aula6-Criando-Pritunl-15](imagens/Aula6-Criando-Pritunl-15.png)

Já podemos trocar o username e a senha que utilzamos para entrar.

Pensar numa senha forte de 10 dígitos, tipo = **Kb[vW8%8S.JB3W)j7=w&**. Clicamos em Save.

Pronto estamos em nosso servidor. Este é o dashboard do servidor.

![Aula6-Criando-Pritunl-15](imagens/Aula6-Criando-Pritunl-15-b.png)

Caso saia do servidor, para entrar novamente coloque o seguinte endereço no navegador: https://45.33.65.57/login

Coloque seu login e senha, no meu caso são estes:

login = carlos

senha = Kb[vW8%8S.JB3W)j7=w&

Este é o nosso servidor.

![Aula6-Criando-Pritunl-16](imagens/Aula6-Criando-Pritunl-16.png)

Clique na opção Users e em seguida em Add Organizations como abaixo:

![Aula6-Criando-Pritunl-17](imagens/Aula6-Criando-Pritunl-17.png)

![Aula6-Criando-Pritunl-18](imagens/Aula6-Criando-Pritunl-18.png)

Coloque o nome da sua organização, se não tiver uma, invente um nome, e clique em Add.

![Aula6-Criando-Pritunl-19](imagens/Aula6-Criando-Pritunl-19.png)

Agora clique em Add User e coloque um nome de usuário e um PIN com 6 números. A organização mantém a que aparecer. Caso queira poderá colocar um email também.

![Aula6-Criando-Pritunl-20](imagens/Aula6-Criando-Pritunl-20.png)

No nosso caso vamos colocar o usuário Roberto e o PIN 123456 e clique em Add, como abaixo:

![Aula6-Criando-Pritunl-21](imagens/Aula6-Criando-Pritunl-21.png)

Você terá uma tela como esta:

![Aula6-Criando-Pritunl-22](imagens/Aula6-Criando-Pritunl-22.png)

Agora vamos na opção Servers, depois na opção Add Server, como abaixo:

![Aula6-Criando-Pritunl-23](imagens/Aula6-Criando-Pritunl-23.png)

![Aula6-Criando-Pritunl-24](imagens/Aula6-Criando-Pritunl-24.png)

Agora colocamos um nome para nosso servidor VPN e clicamos em Add, como abaixo:

![Aula6-Criando-Pritunl-25](imagens/Aula6-Criando-Pritunl-25.png)

Irá aparecer uma tela como esta:

![Aula6-Criando-Pritunl-26](imagens/Aula6-Criando-Pritunl-26.png)

Agora precisamos anexar a organização que nos criamos anteriormente. Basta clicar em Attach Organization.

![Aula6-Criando-Pritunl-27](imagens/Aula6-Criando-Pritunl-27.png)

E novamente em Attach.

![Aula6-Criando-Pritunl-28](imagens/Aula6-Criando-Pritunl-28.png)

Agora clicamos em Start Server.

![Aula6-Criando-Pritunl-29](imagens/Aula6-Criando-Pritunl-29.png)

Irá aparecer uma tela como esta.

![Aula6-Criando-Pritunl-30](imagens/Aula6-Criando-Pritunl-30.png)

Entre no nosso servidor VPN pelo SSH, conforme anteriormente fizemos:

```console
ssh carlos@45.33.65.57
```

E dê o seguinte comando:

```console
sudo ufw status
```

Deverá aparecer o seguinte:

```console
[sudo] password for carlos: 
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                  
443                        ALLOW       Anywhere                  
80                         ALLOW       Anywhere                  
22/tcp (v6)                ALLOW       Anywhere (v6)             
443 (v6)                   ALLOW       Anywhere (v6)             
80 (v6)                    ALLOW       Anywhere (v6)
```

Observe que precisamos liberar a porta do nosso servidor para que ele funcione corretamente. Então vamos na tela anterior e pegamos a número da porta. No nosso caso é: 14458/udp.

![Aula6-Criando-Pritunl-31](imagens/Aula6-Criando-Pritunl-31.png)

E daremos um comando no servidor para liberar esta porta.

```console
sudo ufw allow 14458/udp
```

Irá aparecer:

```console
Rule added
Rule added (v6)
```

Podemos conferir as portas que estão abertas.

```console
sudo ufw status
```

E iremos obter:

```console
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere                  
443                        ALLOW       Anywhere                  
80                         ALLOW       Anywhere                  
14458/udp                  ALLOW       Anywhere                  
22/tcp (v6)                ALLOW       Anywhere (v6)             
443 (v6)                   ALLOW       Anywhere (v6)             
80 (v6)                    ALLOW       Anywhere (v6)             
14458/udp (v6)             ALLOW       Anywhere (v6)
```

Agora já podemos sair do acesso SSH com exit.

```console
exit
```

Clique novamente em Users.

![Aula6-Criando-Pritunl-32](imagens/Aula6-Criando-Pritunl-32.png)

Clique no ícone do clipes, conforme abaixo para obter o profile temporário de acesso que utilizaremos em nosso Pritunl Client.

![Aula6-Criando-Pritunl-33](imagens/Aula6-Criando-Pritunl-33.png)

Vamos obter o seguinte:

![Aula6-Criando-Pritunl-34](imagens/Aula6-Criando-Pritunl-34.png)

Na última informação podemos copiar este dado para incluir em nosso Pritunl Client, como abaixo:

Abra o Pritunl Client e clique em Import, conforme abaixo:

![Aula6-Criando-Pritunl-35](imagens/Aula6-Criando-Pritunl-35.png)

Em seguida cole a última informação de profile e clique em Import.

![Aula6-Criando-Pritunl-36](imagens/Aula6-Criando-Pritunl-36.png)

Agora clique em Connect.

![Aula6-Criando-Pritunl-37](imagens/Aula6-Criando-Pritunl-37.png)

Coloque o número do PIN que criamos anteriormente e clique novamente em Connect.

![Aula6-Criando-Pritunl-38](imagens/Aula6-Criando-Pritunl-38.png)

Você terá a seguinte tela:

![Aula6-Criando-Pritunl-39](imagens/Aula6-Criando-Pritunl-39.png)

A partir de agora está utilizando uma VPN. Pode verificar seu IP antes de conectar na VPN e verificar novamente depois que estiver usando sua VPN.

Segue uma tela usando a VPN:

![Aula6-Criando-Pritunl-40](imagens/Aula6-Criando-Pritunl-40.png)

Segue uma tela sem usar a VPN:

![Aula6-Criando-Pritunl-41](imagens/Aula6-Criando-Pritunl-41.png)

Também é possível clicar no ícone de download para baixar uma arquivo profile. O ícone está ao lado direito do ícone do clipes. Trata-se de um arquivo zip que contém um arquivo ovpn para utilizar em sua rede caso queira.

# FIM