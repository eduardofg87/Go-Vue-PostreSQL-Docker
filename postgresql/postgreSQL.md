# Criando um container para executar uma instância do PostgreSQL + pgAdmin4

### As imagens oficiais do PostgreSQL e do pgAdmin 4 se encontram no Docker Hub, podendo ser baixadas através do comando docker pull
`sudo docker pull postgres`
`sudo docker pull dpage/pgadmin4`

### Criando uma network para execução dos containers
`sudo docker network create --driver bridge postgres-network`

### Criando um container para executar uma instância do PostgreSQL
`sudo docker run --name teste-postgres --network=postgres-network -e "POSTGRES_PASSWORD=Postgres@2019" -p 5432:5432 -v ./database:/var/lib/postgresql/data -d postgres`

O atributo name especifica o nome do container a ser gerado (teste-postgres);
Para o atributo network foi definido o valor da rede criada na seção anterior (postgres-network);
No atributo POSTGRES_PASSWORD foi indicada a senha do administrador (para o usuário default postgres);
O atributo -p indica a porta (5432) em que se dará a comunicação com o PostgreSQL, a qual será mapeada para a porta (5432) deste SGBD dentro do container;
Através do atributo -v foi criado um volume, especificando assim o diretório no Ubuntu Desktop em que serão gravados os arquivos de dados (/home/renatogroffe/Desenvolvimento/PostgreSQL);
Quanto ao atributo -d, este parâmetro determina que o container em questão será executado como um serviço em background;
Temos indicada ainda a imagem utilizada como base para a geração do container (postgres).

### Criando um container para execução do pgAdmin 4

`sudo docker run --name teste-pgadmin --network=postgres-network -p 8081:80 -e "PGADMIN_DEFAULT_EMAIL=fake@email.com" -e "PGADMIN_DEFAULT_PASSWORD=PgAdmin@2019" -d dpage/pgadmin4`

### O atributo name indica o nome do container a ser criado (teste-pgadmin);
No atributo network foi atribuído o nome da rede utilizada na comunicação entre a instância do PostgreSQL e o pgAdmin (postgres-network);
O atributo -p especifica a porta (8081) em que acontecerá a comunicação com o pgAdmin 4, a qual será mapeada para a porta default (80) desta aplicação Web;
No atributo PGADMIN_DEFAULT_EMAIL foi informado o e-mail de acesso ao pgAdmin;
No atributo PGADMIN_DEFAULT_PASSWORD foi indicada ainda a senha de acesso ao pgAdmin 4;
Temos especificada também a imagem empregada na geração do container (dpage/pgadmin4).
O comando `docker ps` listará esse novo container

### Testes
#### Acessando a URL http://localhost:8081 aparecerá a tela para login no pgAdmin 4:
`login:fake@email.com`
`senha:PgAdmin2019!`

### Referências:
#### https://medium.com/@renato.groffe/postgresql-docker-executando-uma-inst%C3%A2ncia-e-o-pgadmin-4-a-partir-de-containers-ad783e85b1a4
#### https://hub.docker.com/_/postgres/
#### https://hub.docker.com/r/dpage/pgadmin4/