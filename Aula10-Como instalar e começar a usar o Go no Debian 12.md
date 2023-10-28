# Como instalar e começar a usar o Go no Debian

## Instalação

Go é um ambiente de programação de código aberto que faz com que seja fácil de construir software simples, confiável e eficiente. Além de ser um projeto de código aberto desenvolvido pela equipe do Google e muitos contribuidores da comunidade open source, ele é distribuído sob a licença estilo BSD.

Go é multiplataformas e distribuições oficiais dos binários estão disponíveis para o FreeBSD, Linux, MacOS X, NetBSD, e Windows.

Vamos agora instalar o Go lembrando que deveremos ter privilégio de root para a instalação do mesmo. No caso do Debian utilizamos o comando sudo para os privilégios de root, logo, se sua distribuição não esteja com o sudo configurado, logue-se como root para a execução do mesmo ou configure o sudo nela.

1. Download do Go

    No momento da publicação deste post, a versão estável mais recente do Go é a versão 1.21.3. Antes de baixar o tarball, [visite a página oficial de downloads Go](https://go.dev/dl/) e verifique se há uma nova versão disponível.

    ```console
    wget https://go.dev/dl/go1.21.3.linux-amd64.tar.gz -O gol.tar.gz
    ```

2. Remova qualquer instalação anterior do Go excluindo a pasta /usr/local/go (se existir). Não descompacte o arquivo em uma árvore /usr/local/go existente. Sabe-se que isso produz instalações Go quebradas.

    ```console
    sudo rm -rf /usr/local/go
    ```

3. extraia o arquivo que você acabou de baixar em /usr/local, criando uma nova árvore Go em /usr/local/go:

    ```console
    sudo tar -vzxf gol.tar.gz -C /usr/local
    rm gol.tar.gz
    ```

4. Adicione /usr/local/go/bin à variável de ambiente PATH.

    Você pode fazer isso adicionando a seguinte linha ao seu $HOME/.profile ou /etc/profile (para uma instalação em todo o sistema):

    ```console
    exportar PATH=$PATH:/usr/local/go/bin
    ```

    Eu executei os seguintes comandos, para adicionar a variável PATH em meus arquivos:

    ```console
    echo 'export GOROOT=/usr/local/go' | sudo tee -a /etc/profile
    echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee -a /etc/profile

    echo 'export GOROOT=/usr/local/go' | tee -a ~/.bashrc
    echo 'export PATH=$PATH:/usr/local/go/bin' | tee -a ~/.bashrc

    echo 'export GOROOT=/usr/local/go' | tee -a ~/.profile
    echo 'export PATH=$PATH:/usr/local/go/bin' | tee -a ~/.profile

    echo 'export GOROOT=/usr/local/go' | tee -a ~/.zshrc
    echo 'export PATH=$PATH:/usr/local/go/bin' | tee -a ~/.zshrc
    ```

    Para surtir efeito deslogue do seu computador e logue-se novamente.

5. Verifique se você instalou o Go abrindo um prompt de comando e digitando o seguinte comando:

    ```console
    go version
    ```

    Vai aparecer na tela a seguinte informação:

    ```console
    go version go1.21.3 linux/amd64
    ```

## Como escrever código Go

Este documento demonstra o desenvolvimento de um pacote Go simples dentro de um módulo e apresenta a ferramenta go, a maneira padrão de buscar, construir e instalar módulos, pacotes e comandos Go.

## Organização do código

Os programas Go são organizados em pacotes. Um pacote é uma coleção de arquivos de origem no mesmo diretório que são compilados juntos. Funções, tipos, variáveis ​​e constantes definidas em um arquivo de origem são visíveis para todos os outros arquivos de origem do mesmo pacote.

Um repositório contém um ou mais módulos. Um módulo é uma coleção de pacotes Go relacionados que são lançados juntos. Um repositório Go normalmente contém apenas um módulo, localizado na raiz do repositório. Um arquivo nomeado ali go.mod declara o caminho do módulo: o prefixo do caminho de importação para todos os pacotes dentro do módulo. O módulo contém os pacotes no diretório que contém seu arquivo go.mod, bem como os subdiretórios desse diretório, até o próximo subdiretório que contém outro arquivo go.mod (se houver).

Observe que você não precisa publicar seu código em um repositório remoto antes de construí-lo. Um módulo pode ser definido localmente sem pertencer a um repositório. No entanto, é um bom hábito organizar seu código como se você fosse publicá-lo algum dia.

O caminho de cada módulo não serve apenas como um prefixo do caminho de importação para seus pacotes, mas também indica onde o comando go deve procurar para baixá-lo. Por exemplo, para baixar o módulo golang.org/x/tools, o comando go consultaria o repositório indicado por https://golang.org/x/tools (descrito mais [aqui](https://pkg.go.dev/cmd/go#hdr-Remote_import_paths)).

Um caminho de importação é uma string usada para importar um pacote. O caminho de importação de um pacote é o caminho do módulo unido ao seu subdiretório dentro do módulo. Por exemplo, o módulo github.com/google/go-cmp contém um pacote no diretório cmp/. O caminho de importação desse pacote é github.com/google/go-cmp/cmp. Os pacotes na biblioteca padrão não possuem um prefixo de caminho de módulo.

## Seu primeiro programa

Para compilar e executar um programa simples, primeiro escolha um caminho de módulo (usaremos ~/go/src/hello) depois crie o arquivo go.mod com o comando go mod init, dessa forma:

```console
mkdir -p ~/go/src/hello
cd ~/go/src/hello
go mod init go/src/hello
```

Vai aparecer na tela a seguinte informação:

```console
go: creating new go.mod: module go/src/hello
```

Agora digite cat para ver o conteúdo do arquivo:

```console
cat go.mod
```

Vai aparecer na tela a seguinte informação:

```console
module go/src/hello

go 1.21.3
```

A primeira instrução em um arquivo fonte Go deve ser package name. Comandos executáveis ​​devem sempre usar package main.

Em seguida, crie um arquivo nomeado de hello.go dentro desse diretório contendo o seguinte código Go:


```go
package main

import "fmt"

func main() {
    fmt.Println("Olá Mundo!")
}
```

Agora você pode criar e instalar esse programa com a ferramenta go:

```console
go install go/src/hello
```

Este comando cria o comando hello, produzindo um binário executável. Em seguida, ele instala esse binário como $HOME/go/bin/hello (ou, no Windows, %USERPROFILE%\go\bin\hello.exe).

O diretório de instalação é controlado [pelas variáveis ​​de ambiente](https://go.dev/cmd/go/#hdr-Environment_variables) GOPATH e GOBIN. Se GOBIN estiver definido, os binários serão instalados nesse diretório. Se GOPATH estiver configurado, os binários serão instalados no subdiretório bin do primeiro diretório na lista GOPATH. Caso contrário, os binários serão instalados no subdiretório bin do GOPATH padrão ($HOME/go ou %USERPROFILE%\go).

Você pode usar o comando go env para definir de forma portável o valor padrão de uma variável de ambiente para comandos go futuros:

Por exemplo:

```console
go env -w GOBIN=/em-algum-lugar/else/bin
```

Para cancelar a definição de uma variável definida anteriormente por go env -w, use go env -u:

```console
go env -u GOBIN
```

Comandos como go install aplicam-se ao contexto do módulo que contém o diretório de trabalho atual. Se o diretório de trabalho não estiver no módulo go/src/hello, a instalação poderá falhar.

Por conveniência, os comandos go aceitam caminhos relativos ao diretório de trabalho e assumem como padrão o pacote no diretório de trabalho atual se nenhum outro caminho for fornecido. Portanto, em nosso diretório de trabalho, os seguintes comandos são equivalentes:

```console
go install go/src/hello

go install .

go install
```

A seguir, vamos executar o programa para garantir que funciona. Para maior comodidade, adicionaremos o diretório de instalação ao nosso PATH para facilitar a execução de binários:

```console
# Usuários do Windows devem consultar https://github.com/golang/go/wiki/SettingGOPATH
# para definir %PATH%.
export PATH=$PATH:$(dirname $(go list -f '{{.Target}}' .))
hello
Olá Mundo!
```

## Inicializar um repositório Git

Se você estiver usando um sistema de controle de origem, agora seria um bom momento para inicializar um repositório, adicionar os arquivos e submeter sua primeira alteração. Novamente, esta etapa é opcional: você não precisa usar o controle de origem para escrever código Go.

Segue o passo a passo para inicializar um repositório. Digite:

```console
git init
```

Vai aparecer na tela a seguinte informação:

```console
Initialized empty Git repository in /home/carlos/go/src/hello/.git/
```

Depois digite:

```console
git add go.mod hello.go
git commit -m "initial commit"
```

Vai aparecer na tela a seguinte informação:

```console
[master (root-commit) 2332e99] initial commit
 2 files changed, 11 insertions(+)
 create mode 100644 go.mod
 create mode 100644 hello.go
```

O comando go localiza o repositório que contém um determinado caminho de módulo solicitando uma URL HTTPS correspondente e lendo os metadados incorporados na resposta HTML (consulte Recursos [go help importpath](https://go.dev/cmd/go/#hdr-Remote_import_paths)). Muitos serviços de hospedagem já fornecem esses metadados para repositórios contendo código Go, portanto, a maneira mais fácil de disponibilizar seu módulo para uso de outras pessoas é geralmente fazer com que o caminho do módulo corresponda à URL do repositório.

## Importando pacotes do seu módulo

Vamos escrever um pacote com mais strings e usá-lo no programa hello. Primeiro, crie um diretório para o pacote chamado $HOME/hello/morestrings e, em seguida, um arquivo chamado reverse.go nesse diretório com o seguinte conteúdo:

Primeiramente crie o diretório dentro do diretório hello:

```console
mkdir -p ~/go/src/hello/morestrings
cd ~/go/src/hello/morestrings
vi reverse.go
```

Inclua no arquivo reverse.go o seguine conteúdo:

```go
// O pacote morestrings implementa funções adicionais para manipular strings
// UTF-8 codificadas, além do que é fornecido no pacote padrão "strings".

package morestrings

// ReverseRunes retorna sua string de argumento invertida em termos de runas, da esquerda para a direita.

func ReverseRunes(s string) string {
    r := []rune(s)
    for i, j := 0, len(r)-1; i < len(r)/2; i, j = i+1, j-1 {
        r[i], r[j] = r[j], r[i]
    }
    return string(r)
}
```

Como nossa fução ReverseRunes começa com letra maiúscula, ela é exportada e pode ser usada em outros pacotes que importam nosso morestrings pacote.

Vamos testar se o pacote é compilado com go build:

```console
cd ~/go/src/hello/morestrings
go build
```

Isso não produzirá um arquivo de saída. Em vez disso, ele salva o pacote compilado no cache de compilação local.

Depois de confirmar que o pacote morestrings foi compilado, vamos usá-lo no programa hello. Para fazer isso, modifique seu arquivo original $HOME/go/src/hello/hello.go para usar o pacote morestrings:

```console
cd ~/go/src/hello
vi ~/go/src/hello/hello.go
```

Modifique o arquivo para o conteúdo abaixo:

```go
package main

import (
    "fmt"

    "go/src/hello/morestrings"
)

func main() {
    fmt.Println(morestrings.ReverseRunes("!somaV ,álO"))
}
```

Instale o programa hello:

```console
go install go/src/hello
```

Ao executar a nova versão do programa, você deverá ver uma mensagem nova e invertida:

```console
hello
```

Vai aparecer na tela a seguinte informação:

```console
Olá, Vamos!
```

## Importando pacotes de módulos remotos

Um caminho de importação pode descrever como obter o código-fonte do pacote usando um sistema de controle de revisão como Git ou Mercurial. A ferramenta go usa essa propriedade para buscar pacotes automaticamente de repositórios remotos. Por exemplo, para usar github.com/google/go-cmp/cmp em seu programa:

```go
package main

import (
    "fmt"

    "go/src/hello/morestrings"
    "github.com/google/go-cmp/cmp"
)

func main() {
    fmt.Println(morestrings.ReverseRunes("!somaV ,álO"))
    fmt.Println(cmp.Diff("Olá Mundo!", "Olá Vamos!"))
}
```

Agora que você depende de um módulo externo, você precisa baixar esse módulo e registrar sua versão em seu arquivo go.mod. O comando go mod tidy adiciona requisitos de módulo ausentes para pacotes importados e remove requisitos de módulos que não são mais usados.

Execute o comando abaixo:

```console
go mod tidy
```

Vai aparecer na tela a seguinte informação:

```console
go: finding module for package github.com/google/go-cmp/cmp
go: downloading github.com/google/go-cmp v0.6.0
go: found github.com/google/go-cmp/cmp in github.com/google/go-cmp v0.6.0
```

Agora execute o comando de instalação:

```console
go install go/src/hello
```

Agora execute nosso programa para ver o resultado:

```console
hello
```

Vai aparecer na tela a seguinte informação:

```console
Olá, Vamos!
  string(
- 	"Olá Mundo!",
+ 	"Olá Vamos!",
  )
```

Agora vamos ver o arquivo go.mod para saber o conteúdo dele:

```console
cat go.mod
```

Vai aparecer na tela a seguinte informação:

```console
module go/src/hello

go 1.21.3

require github.com/google/go-cmp v0.6.0
```

As dependências do módulo são baixadas automaticamente para o subdiretório pkg/mod do diretório indicado pela variável de ambiente GOPATH. O conteúdo baixado para uma determinada versão de um módulo é compartilhado entre todos os outros módulos que requerem essa versão, portanto, o comando go marca esses arquivos e diretórios como somente leitura. Para remover todos os módulos baixados, você pode passar a opção -modcache para limpar:

```console
go clean -modcache
```

## Teste

Go possui uma estrutura de teste leve composta pelo comando go test e pelo pacote testing.

Você escreve um teste criando um arquivo com um nome que termina em _test.go que contém funções nomeadas TestXXXcom assinatura func (t *testing.T). A estrutura de teste executa cada uma dessas funções; se a função chamar uma função de falha como t.Error ou t.Fail, o teste será considerado como tendo falhado.

Adicione um teste ao pacote morestrings criando o arquivo ~/go/src/hello/morestrings/reverse_test.go que contém o seguinte código Go.

```console
cd ~/go/src/hello/morestrings
vi ~/go/src/hello/morestrings/reverse_test.go
```

Inclua no arquivo reverse_test.go o seguine conteúdo:

```go
package morestrings

import "testing"

func TestReverseRunes(t *testing.T) {
    cases := []struct {
        in, want string
    }{
        {"Hello, world", "dlrow ,olleH"},
        {"Hello, 世界", "界世 ,olleH"},
        {"", ""},
    }
    for _, c := range cases {
        got := ReverseRunes(c.in)
        if got != c.want {
            t.Errorf("ReverseRunes(%q) == %q, want %q", c.in, got, c.want)
        }
    }
}
```

Em seguida, execute o teste com go test:

```console
cd ~/go/src/hello/morestrings
go test
```

Vai aparecer na tela a seguinte informação:

```console
PASS
ok  	go/src/hello/morestrings	0.002s
```

Execute [go help teste](https://go.dev/cmd/go/#hdr-Test_packages) e consulte a [documentação do pacote de testes](https://go.dev/pkg/testing/) para obter mais detalhes.

## Qual é o próximo

Inscreva-se na lista de discussão [golang-announce](https://groups.google.com/group/golang-announce) para ser notificado quando uma nova versão estável do Go for lançada.

Consulte [Effective Go](https://go.dev/doc/effective_go.html) para obter dicas sobre como escrever código Go claro e idiomático.

Faça [um tour pelo Go](https://go.dev/tour/) para aprender sobre a forma adequada da linguagem Go.

Visite a [página de documentação](https://go.dev/doc/#articles) para obter um conjunto de artigos detalhados sobre a linguagem Go e suas bibliotecas e ferramentas.

## Conseguindo ajuda

Para obter ajuda em tempo real, pergunte aos gophers prestativos no [servidor Slack de gophers](https://gophers.slack.com/messages/general/) administrado pela comunidade ([pegue um convite aqui](https://invite.slack.golangbridge.org/)).

A lista de discussão oficial para discussão da linguagem Go é [Go Nuts](https://groups.google.com/group/golang-nuts).

Relate bugs usando o [rastreador de problemas Go](https://go.dev/issue).
