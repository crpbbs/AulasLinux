# Como usar o Docker Compose

Docker Compose é uma ferramenta que ajuda a definir e compartilhar aplicativos com vários contêineres. Com o Compose, você pode criar um arquivo YAML para definir os serviços e, com um único comando, pode aumentar ou diminuir tudo.

A grande vantagem de usar o Compose é que você pode definir a pilha de aplicativos em um arquivo, mantê-la na raiz do repositório do projeto (agora com versão controlada) e permitir facilmente que outra pessoa contribua com o seu projeto. Alguém precisaria apenas clonar seu repositório e iniciar o aplicativo usando o Compose. Na verdade, você pode ver alguns projetos no GitHub/GitLab fazendo exatamente isso agora.

## Crie o arquivo Compose

No diretório getting-started-app, crie um arquivo chamado compose.yaml.

```console
cd ~/getting-started-app
touch compose.yaml
tree
```

Observe como ficou seu diretório:

![Aula5-9-Docker-34.png](imagens/Aula5-9-Docker-34.png)

## Definir o serviço de aplicativo

Anteriormente, você usou o comando a seguir para iniciar o serviço de aplicativo.

```console
docker run -dp 0.0.0.0:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
```

Agora você definirá esse serviço no arquivo compose.yaml.

1. Abra compose.yaml em um editor de texto ou código e comece definindo o nome e a imagem do primeiro serviço (ou contêiner) que deseja executar como parte de sua aplicação. O nome se tornará automaticamente um alias de rede, o que será útil ao definir seu serviço MySQL.

```console
cd ~/getting-started-app
vi compose.yaml
```

```console
services:
  app:
    image: node:18-alpine
```

2. Normalmente, você verá a diretiva command próximo à diretiva image, embora não haja nenhuma exigência sobre isso. Adicione command ao seu arquivo compose.yaml.

```console
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
```

3. Agora coloque a diretiva ports e informe - 0.0.0.0:3000:3000.

```console
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 0.0.0.0:3000:3000
```

4. Agora coloque a diretiva working_dir com a opção /app e a diretiva volumes com a opção - ./:/app.

    Uma vantagem das definições de volume do Docker Compose é que você pode usar caminhos relativos do diretório atual.

```console
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 0.0.0.0:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
```

5. Finalmente, você precisa migrar as definições de variáveis ​​de ambiente usando a chave environment.

```console
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 0.0.0.0:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
```

### Definir o serviço MySQL

Agora é hora de definir o serviço MySQL. O comando que você usou para esse contêiner foi o seguinte:

```console
docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.0
```

1. Primeiro defina o novo serviço e nomeie-o para mysql que obtenha automaticamente o alias da rede. Especifique também a imagem a ser usada.

```console
services:
  app:
    # A definição do serviço do app.
  mysql:
    image: mysql:8.0
```

2. A seguir, defina o mapeamento de volume. Quando você executou o contêiner com docker run, o Docker criou o volume nomeado automaticamente. No entanto, isso não acontece ao executar com o Compose. Você precisa definir o volume na seção volumes: de nível superior e depois especificar o ponto de montagem na configuração do serviço. Simplesmente fornecendo apenas o nome do volume, as opções padrão são usadas.

```console
services:
  app:
    # A definição do serviço do app.
  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql

volumes:
  todo-mysql-data:
```

3. Finalmente, você precisa especificar as variáveis ​​de ambiente.

```console
services:
  app:
    # A definição do serviço do app.
  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

Neste ponto, seu arquivo compose.yaml está completo e deve ficar assim. Muito atenção à identação. O sistema é muito rígido com relação a identação, são 2 espaços conforme descrito abaixo:

```console
services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 0.0.0.0:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
```

## Execute a pilha de aplicativos

Agora que você tem seu arquivo compose.yaml, pode iniciar seu aplicativo.

1. Certifique-se de que nenhuma outra cópia dos contêineres esteja em execução primeiro. Use docker ps para listar os contêineres e docker rm -f <ids> para removê-los. Pode-se usar também o docker rmi <ID> para remover as imagens, para saber os IDS utilize o comando docker images.

2. Inicie a pilha de aplicativos usando o comando docker compose up. Adicione a opção -d para executar tudo em segundo plano.

    ```console
    cd ~/getting-started-app
    docker compose up -d
    ```

    Ao executar o comando anterior, você deverá ver uma saída semelhante a esta:

    ![Aula5-9-Docker-35.png](imagens/Aula5-9-Docker-35.png)

    Você notará que o Docker Compose criou o volume e também uma rede. Por padrão, o Docker Compose cria automaticamente uma rede especificamente para a pilha de aplicativos (é por isso que você não definiu uma no arquivo Compose).

3. Veja os logs usando o comando docker compose logs -f. Você verá os logs de cada um dos serviços intercalados em um único fluxo. Isso é extremamente útil quando você deseja observar problemas relacionados ao tempo. A opção -f segue o log, portanto fornecerá uma saída ao vivo à medida que é gerada.

    Se você já executou o comando, verá uma saída semelhante a esta:

    O nome do serviço é exibido no início da linha (geralmente colorido) para ajudar a distinguir as mensagens. Se quiser visualizar os logs de um serviço específico, você pode adicionar o nome do serviço ao final do comando logs (por exemplo, docker compose logs -f app).

4. Neste ponto, você poderá abrir seu aplicativo em seu navegador em http://localhost:3000 e vê-lo funcionando.