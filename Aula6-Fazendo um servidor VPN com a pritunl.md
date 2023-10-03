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

Voltando ao nosso servidor e quando aparecer a mensagem Installation complete! é porque terminou.

![Aula6-Criando-Pritunl-08](imagens/Aula6-Criando-Pritunl-08.png)

Já pode fechar esta janela. Na verdade nem precisa estar aberta. Foi somente para saber quando o processo termina.

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

Pensar numa senha forte de 10 dígitos, tipo = **Kb[vW8%8S.JB3W)j7=w&**




Dê um nome para seu servidor. O meu se chamará **pritunl-newark-crpbbs**.

Sistema operacional que vamos utilizar = DEBIAN 10 Buster

```console
sudo tee /etc/apt/sources.list.d/pritunl.list << EOF
deb https://repo.pritunl.com/stable/apt buster main
EOF

sudo apt install dirmngr
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A
sudo apt update
sudo apt install pritunl-client-electron
```



Ubuntu 23.04 Lunar
sudo tee /etc/apt/sources.list.d/pritunl.list << EOF
deb https://repo.pritunl.com/stable/apt jammy main
EOF
sudo apt --assume-yes install gnupg
gpg --keyserver hkp://keyserver.ubuntu.com --recv-keys 7568D9BB55FF9E5287D586017AE645C0CF8E292A
gpg --armor --export 7568D9BB55FF9E5287D586017AE645C0CF8E292A | sudo tee /etc/apt/trusted.gpg.d/pritunl.asc
sudo apt update
sudo apt install pritunl-client-electron


OU BAIXAR O PACOTE ADEQUADO PELO GITHUB
wget https://objects.githubusercontent.com/github-production-release-asset-2e65be/35986723/3c84be85-f8e1-44d8-9b77-e20a0f19cd7a?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20230930%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20230930T165822Z&X-Amz-Expires=300&X-Amz-Signature=6c7c969c79d60b0fa56e9a97f00f6c50685433efc1382bad1b85fe6f4338360e&X-Amz-SignedHeaders=host&actor_id=62070772&key_id=0&repo_id=35986723&response-content-disposition=attachment%3B%20filename%3Dpritunl-client-electron_1.3.3637.72-0debian1.bookworm_amd64.deb&response-content-type=application%2Foctet-stream


sudo pritunl setup-key
gerou
a9e1025aad374eb3919bd9894207812d


sudo pritunl default-password
gerou
Administrator default password:
  username: "pritunl"
  password: "IDc5GcNXFPNM"

