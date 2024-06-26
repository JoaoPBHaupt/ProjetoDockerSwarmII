
# Configuração do Projeto Docker WordPress

## Passo 1: Clonar o Repositório

Primeiramente, clone o repositório do projeto e entre no diretório criado:

```sh
git clone https://github.com/JoaoPBHaupt/ProjetoDockerSwarmII.git && cd ProjetoDockerSwarmII
```

## Passo 2: Inicializar o Docker Swarm e Subir os Serviços

Utilize a CLI do Docker para iniciar um swarm e implantar os serviços definidos no arquivo `dockerSwarm.yml`:

```sh
docker swarm init
docker stack deploy -c dockerSwarm.yml <nome_stack>
```

Substitua `<nome_stack>` pelo nome que você deseja dar ao seu stack.

## Passo 3: Configuração do WordPress

Acesse o WordPress através do seu navegador na URL `http://localhost:8080` e siga as instruções para criar uma conta.

### Instalação do Plugin Redis

1. No painel administrativo do WordPress, navegue até a seção de plugins.
2. Instale o plugin do Redis.

### Modificação do Arquivo `wp-config.php`

Precisamos editar o arquivo `wp-config.php` no contêiner do WordPress para configurar o Redis:

1. Liste os contêineres em execução para encontrar o ID do contêiner do WordPress:

    ```sh
    docker ps
    ```

2. Acesse o contêiner do WordPress com o comando abaixo, substituindo `<id_container_wordpress>` pelo ID obtido anteriormente:

    ```sh
    docker exec -it <id_container_wordpress> bash
    ```

3. Instale um editor de texto (nano) no contêiner:

    ```sh
    apt update && apt install nano
    ```

4. Edite o arquivo `wp-config.php` utilizando o nano:

    ```sh
    nano wp-config.php
    ```

5. Adicione as seguintes linhas ao final do arquivo:

    ```php
    define('WP_CACHE', true);
    define('WP_REDIS_HOST', 'redis');
    define('WP_REDIS_PORT', 6379);
    ```

6. Salve as alterações pressionando `CTRL+O` e depois `Enter`. Saia do nano pressionando `CTRL+X`.

## Serviços Disponíveis

Após configurar o WordPress e o Redis, os serviços estarão disponíveis nas seguintes URLs:

- **WordPress**: `http://localhost:8080`
- **Grafana**: `http://localhost:3000` (login: `admin`, senha: `admin`)
- **Prometheus**: `http://localhost:9090`
