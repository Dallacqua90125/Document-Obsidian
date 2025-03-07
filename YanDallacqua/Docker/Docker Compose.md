### Docker Componse

Docker Compose é uma ferramenta para definir e executar aplicativos multi-recipientes.

A ideia é você tem diversas tasks para rodar seu projeto e com ele isso fica mais fácil.

> [!NOTE]
> Ex: Você cria uma api em ASP.NET e linka com um banco de dados(nesse caso postgreSQL).
> Então quando você for rodar isso, invés de ter q rodar a api toda vez, você simplifica esse processo com um container do docker, dessa forma fica muito mais fácil de rodar esse seu projeto, e para utilizar também

E para subir esse container, se resume a um comando
```
docker compose up --build
```

nesse comando ele vai buildar uma imagem que vai ser o que está no docker file, e depois vai criar os containers de acordo com o docker compose

