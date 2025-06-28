# Relatório WordPress + MariaDB

## Integrantes:
- Felipe Parreiras Dias
- José Marconi Almeida

## Explicação sobre a funcionalidade do serviço

### 🔧 WordPress + MariaDB com Docker: o que é e por que usar

WordPress é a plataforma de CMS (Content Management System) mais popular do mundo. Com ela, é possível criar e gerenciar sites, blogs, lojas virtuais e até portais corporativos, tudo através de uma interface gráfica simples e intuitiva. Porém, para funcionar corretamente, ele precisa de um banco de dados relacional onde armazena posts, páginas, usuários, plugins e configurações. É aí que entra o MariaDB, um sistema de banco de dados leve, robusto e compatível com MySQL.

### 🐳 Uso com Docker: múltiplos containers interligados

Com o Docker, cada serviço (WordPress e MariaDB) é isolado em seu próprio container, o que traz diversas vantagens:

- Facilidade de configuração: basta um docker-compose.yml com as imagens e variáveis.
- Padronização de ambiente: funciona da mesma forma em qualquer máquina ou servidor.
- Escalabilidade e manutenção separadas: você pode atualizar o banco ou o WordPress separadamente.
- Rede interna do Docker: o WordPress se comunica com o banco de dados via hostname (db), sem precisar expor o banco na internet.

Esse tipo de arquitetura permite ao estudante ou profissional aprender sobre dependência de serviços, rede entre containers, variáveis de ambiente, healthchecks e persistência de dados com volumes.

### 🌍 Onde isso é aplicado

- Em ambientes de desenvolvimento local, para testar plugins e temas WordPress rapidamente.
- Em ambientes de produção, com ajustes de segurança e persistência.
- Em hospedagens em nuvem, como AWS, Azure ou servidores VPS.
- Em aulas e treinamentos de DevOps, Docker e sistemas distribuídos, como um exemplo didático completo.

## Instruções de instalação do Docker e Docker Compose

### Instalação: 

1- Comandos iniciais sudo para instalação:
```bash
sudo apt update #recomendável atualizar
sudo apt install -y ca-certificates curl gnupg #certificações que podem ser necessárias para a instalação
```

2- Adiciona a chave GPG oficial
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

```

3-  Adiciona o repositório oficial do Docker
```bash
echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

4- Atualiza os pacotes e instala Docker + Compose v2
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin
docker-compose-plugin
```

5- Testar instalação:
```bash
docker –version
docker compose –version
```
> ⚠️ **POSSÍVEIS PROBLEMAS**
> 
> Em caso de não funcionamento, faça a instalação do Docker via browser, baixando, através do instalador, o aplicativo.
> 
> 1- Instale o Docker Desktop:
>   - Baixe: https://www.docker.com/products/docker-desktop
>   - Siga o assistente de instalação.
>   - Durante a instalação, marque a opção “Enable integration with WSL 2”.
>     
> 2- Configure o WSL para usar o Docker Desktop:
>   - No Docker Desktop, vá em Settings > Resources > WSL Integration.
>   - Ative a integração com sua distro (ex: Ubuntu).
> 3- No terminal WSL, execute os comandos de teste de instalação

6- Teste de execução:
```bash
docker run hello-word
```

## Instruções de execução do WordPress com Docker Compose

Após certificar-se de que o Docker Compose foi instalado corretamente, pode-se passar para a etapa de instalação e execução do WordPress. 

1- Criar o arquivo `docker-compose.yml`.
```bash
mkdir docker-compose.yml
```

2- Abrir o arquivo com `nano` e utilizar o script(default):
O arquivo default está neste repositório salvo como `docker-compose.yml`
```bash
mkdir docker-compose.yml
```
```yml
services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wpdb
    depends_on:
      - db
  db:
    image: mariadb:latest
    environment:
      MYSQL_DATABASE: wpdb
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
      MYSQL_ROOT_PASSWORD: rootpass
    ports:
      - "3306:3306"
```

> ⚠️ **POSSÍVEIS PROBLEMAS**
>
> Caso a porta 8080 já esteja em uso, utilize outra porta para ter acesso via navegador, como a 8888. 

3- Subir os containers:
```bash
docker compose up   # (Para subir em primeiro plano)
docker compose up -d   # (para subir em segundo plano)
```
> O container deve estar em *up* para que a execução seja bem feita.
> Existem outros comandos envolvendo container para visualiação do programa:
> 
> 1- Para ver containers que estão em execução no momento:
> ```bash
> docker ps
> ```
> 2- Para reiniciar os containers:
> ```bash
> docker compose restart
> ```
> 3- Para parar a execução
> ```bash
> docker compose stop
> ```
> 4- Para parar e apagar os containers
> ```bash
> docker compose down
> ```
> 5- Para ver os Logs
> ```bash
> docker compose logs   #(logs gerais)
> docker compose logs wordpress   #(só logs do wordpress)
> docker compose logs db   #(só logs do banco de dados)
> ```
> 6- Para acessar o terminal direto de cada container:
> ```bash
> docker exec -it (id do container wordpress ou do banco) bash
> ```
> 7- Verificar variáveis de ambiente:
> ```bash
> docker compose config
> ```

## Como acessar o banco de dados (MySql MariaDB)
1- Acessar container do banco
```bash
docker exec -it (id do container wordpress ou do banco) bash
```

2- Dentro do terminal do container:
```bash
mariadb -u root -p
#(Senha do root para o banco (definido no script do arquivo yml, nesse caso: rootpass))
```

3- Comandos de SQL padrão:
  - 3.1 - Mostrar DATABASES:
    ```bash
    SHOW DATABASES
    ```
  - 3.2 - Mostrar Tabelas:
    ```bash
    SHOW TABLES
    ```
  - 3.3 - Seleciona um banco de dados para usar
    ```bash
    USE nome_do_banco
    ```
  - 3.4 - Mostra a estrutura (colunas, tipos, chaves) de uma tabela
    ```bash
    DESCRIBE nome_da_tabela
    ```

4- Backup do banco de dados:
```bash
docker run --rm --network trabalho_1_default mysql \
mysqldump -h db -u wpuser -pwppass wpdb > backup.sql
```
ou
```bash
mysqldump -h 127.0.0.1 -P 3306 -u wpuser -pwppass wpdb > backup.sql
```

5- Para verificar se deu certo:
```bash
ls -lh backup.sql
```
> Em caso de sucesso, deve retornar uma mensagem como:
> ```bash
> -rw-r--r-- 1 josemarconi josemarconi 1.3K Jun 25 10:31 backup.sql
> ```

Para ver as primeiras linhas do BD:
```sql
head backup.sql
```
Em caso de sucesso, deve retornar uma mensagem como:
```bash
-- MySQL dump 10.13 Distrib 9.3.0, for Linux (x86_64)
--
-- Host: db Database: wpdb
-- ------------------------------------------------------
-- Server version 11.8.2-MariaDB-ubu2404
/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS
*/;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!50503 SET NAMES utf8mb4 */;
```

## Referências
https://hub.docker.com/_/wordpress

https://hub.docker.com/_/mariadb
