# Relatório WordPress + MariaDB

## Integrantes:
- Felipe Parreiras Dias
- José Marconi 

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



