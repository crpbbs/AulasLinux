# Tutorial: Primeiros passos com Go

## Introcução

Instale o Go (se ainda não o fez).

Escreva um código simples "Olá, mundo".

Use o gocomando para executar seu código.

Use a ferramenta de descoberta de pacotes Go para encontrar pacotes que você pode usar em seu próprio código.

Chamar funções de um módulo externo.

>[!NOTE]
>Para outros tutoriais, consulte [Tutoriais](https://go.dev/doc/tutorial/index.html).

## Pré-requisitos

* Alguma experiência em programação. O código aqui é bem simples, mas ajuda saber algo sobre funções.
* Uma ferramenta para editar seu código. Qualquer editor de texto que você tiver funcionará bem. A maioria dos editores de texto tem um bom suporte para Go. Os mais populares são VSCode (gratuito), GoLand (pago) e Vim (gratuito).
* Um terminal de comando. Go funciona bem usando qualquer terminal no Linux e Mac, e no PowerShell ou cmd no Windows.

## Instalação do Go

Basta usar as etapas de [download e instalação](/Aula10-01-Como%20instalar%20e%20começar%20a%20usar%20o%20Go%20no%20Debian%2012.md).

## Escrever seu primeiro programa em Go

Comece com Olá, mundo.

1. Abra um prompt de comando e faça cd para seu diretório inicial.

    No Linux ou Mac:

    ```console
    cd
    ```

    No Windows:

    ```console
    CD %HOMEPATH%
    ```

2. Crie um diretório hello para seu primeiro código-fonte Go.

    Por exemplo, use os seguintes comandos:

    ```console
    mkdir hello
    CD hello
    ```

3. Habilite o rastreamento de dependência para seu código.

    Quando seu código importa pacotes contidos em outros módulos, você gerencia essas dependências por meio do próprio módulo do seu código. Esse módulo é definido por um arquivo go.mod que rastreia os módulos que fornecem esses pacotes. Esse arquivo go.mod faz parte do seu código, inclusive no mesmo diretório do código-fonte.

    Para ativar o rastreamento de dependência para seu código criando um arquivo go.mod, execute o comando go mod init, fornecendo a ele o nome do módulo em que seu código estará. O nome é o diretório do módulo.

    No desenvolvimento real, o diretório do módulo normalmente será o local do repositório onde seu código-fonte será mantido. Por exemplo, o caminho do módulo pode ser github.com/mymodule. Se você planeja publicar seu módulo para uso de outras pessoas, o caminho do módulo deve ser um local de onde as ferramentas Go possam fazer download do seu módulo. Para obter mais informações sobre como nomear um módulo com um caminho de módulo, consulte [Gerenciando dependências](https://go.dev/doc/modules/managing-dependencies#naming_module).

    Para os própositos deste tutorial, vamos utilizar o caminho padrão do Go. A variável GOPATH aponta para o diretório padrão do Go. Para saber qual o diretório do Go faça:

    ```console
    go env GOPATH
    ```

    Vai aparecer na tela a seguinte informação:

    ```console
    /home/NOME_DO_USUÁRIO/go
    ```

    Então para começarmos, vamos para o diretório padrão e depois criamos um diretório para o nosso código. Normalmente se usa o diretório scr para ir colocando nossos códigos fontes. Então faremos:

    ```console
    mkdir -p ~/go/src/hello
    cd ~/go/src/hello
    go mod init go/src/hello
    ```

    Vai aparecer na tela a seguinte informação:

    ```console
    go: creating new go.mod: module go/src/hello
    ```

4. Em seu editor de texto, crie um arquivo hello.go para escrever seu código.
   
   ```console
   code hello.go
   ```

5. Cole o código a seguir em seu arquivo hello.go e salve o arquivo.

    ```console
    package main

    import "fmt"

    func main() {
        fmt.Println("Hello, World!")
    }
    ```

    Este é o seu código Go. Neste código, você:

    Declara um pacote main (um pacote é uma forma de agrupar funções e é composto por todos os arquivos no mesmo diretório).

    Importa o [pacote popular fmt](https://pkg.go.dev/fmt/), que contém funções para formatação de texto, incluindo impressão no console. Este pacote é um dos pacotes de biblioteca padrão que você obteve quando instalou o Go.

    Implementa uma função main para imprimir uma mensagem no console. Uma função main é executada por padrão quando você executa o pacote main.

6. Execute seu código para ver a saudação.

    ```console
    go run .
    ```

    Vai aparecer na tela a seguinte informação:

    ```console
    Hello, World!
    ```

    O comando go run é um dos muitos comandos go que você usará para realizar tarefas com Go. Use o seguinte comando para obter uma lista dos outros comandos:

    ```console
    go help
    ```

## Código de chamada em um pacote externo

Quando você precisar que seu código faça algo que possa ter sido implementado por outra pessoa, você pode procurar um pacote que tenha funções que você possa usar em seu código.

1. Torne a sua mensagem impressa um pouco mais interessante com uma função de um módulo externo.

   1. Visite pkg.go.dev e procure um pacote de "cotação".
   2. Localize e clique no rsc.io/quotepacote nos resultados da pesquisa (se você vir rsc.io/quote/v3, ignore-o por enquanto).
   3. Na seção Documentação , em Índice , observe a lista de funções que você pode chamar do seu código. Você usará a Gofunção.
   4. No topo desta página, observe que o pacote quoteestá incluído no rsc.io/quotemódulo.

    Você pode usar o site pkg.go.dev para encontrar módulos publicados cujos pacotes possuem funções que você pode usar em seu próprio código. Os pacotes são publicados em módulos -- como rsc.io/quote-- onde outros podem usá-los. Os módulos são aprimorados com novas versões ao longo do tempo e você pode atualizar seu código para usar as versões aprimoradas.

2. No seu código Go, importe o rsc.io/quotepacote e adicione uma chamada à sua Gofunção.

    Depois de adicionar as linhas destacadas, seu código deverá incluir o seguinte:

    ```console
    package main

    import "fmt"

    import "rsc.io/quote"

    func main() {
        fmt.Println(quote.Go())
    }
    ```

3. Adicione novos requisitos e somas de módulo.

    Go adicionará o quotemódulo como um requisito, bem como um arquivo go.sum para uso na autenticação do módulo. Para obter mais informações, consulte Autenticando módulos na Referência de módulos Go.

    ```console
    $ go mod tidy
    go: finding module for package rsc.io/quote
    go: found rsc.io/quote in rsc.io/quote v1.5.2
    ```

4. Execute seu código para ver a mensagem gerada pela função que você está chamando.

    ```console
    $ go run .
    Don't communicate by sharing memory, share memory by communicating.
    ```

    Observe que seu código chama a Gofunção, imprimindo uma mensagem inteligente sobre comunicação.

    Quando você executou go mod tidy, ele localizou e baixou o rsc.io/quotemódulo que contém o pacote que você importou. Por padrão, ele baixou a versão mais recente – v1.5.2.

## Escreva mais código

Com esta introdução rápida, você instalou o Go e aprendeu alguns princípios básicos. Para escrever mais código com outro tutorial, dê uma olhada no módulo Create a Go .
