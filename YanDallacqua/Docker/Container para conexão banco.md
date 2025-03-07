
## MySql

```
docker run -d --name meu-mysql -e MYSQL_ROOT_PASSWORD=senha_segura -e MYSQL_DATABASE=meu_banco -e MYSQL_USER=usuario -e MYSQL_PASSWORD=senha_usuario -p 3306:3306 mysql:latest
```

## PostgresSQL

```
docker run -d --name meu-postgres -e POSTGRES_USER=usuario -e POSTGRES_PASSWORD=senha_segura -e POSTGRES_DB=meu_banco -p 5432:5432 postgres:latest
```