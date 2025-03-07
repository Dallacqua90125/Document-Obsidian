### Criar container através de uma imagem

- Criar uma imagem 
```
docker build -t <nome da imagem> .
```

- Visualizar as imagens criadas
```
docker image ls
```

- Criar container com porta e nome usando uma imagem criada
```
docker run -d -p 80:80 --name meu-container <nome da imagem>
```

### Dockerfile
Para a criação de imagem, precisamos de um dockerfile no projeto ou contem alguns comando para serem executados para q rode o programa com o docker


### Dockerfile .NET
```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS base //define a imagem base, no caso .net

WORKDIR /app //define diretorio de trabalho

EXPOSE 80 // HTTP (sem segurança)
			// Ambos são as portas para acessar a aplicação
EXPOSE 443 // HTTPS (com segurança)


FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build //Usa uma nova imagem SDK para contruir a aplicação, onde sera compilado o projeto e rodado

COPY ["d1000.FrenteLoja/d1000.FrenteLoja/d1000.FrenteLoja.csproj", "d1000.FrenteLoja/"] //Copia o arquivo do csproj para o diretório correspondente dentro do container


RUN dotnet restore ./d1000.FrenteLoja/d1000.FrenteLoja.csproj //Executa o comando dotnet restore para baixar os pacotes NuGet

COPY . . //Copia todos os pacotes

RUN dotnet build "d1000.FrenteLoja/d1000.FrenteLoja/d1000.FrenteLoja.csproj" -c Release -o /app/build //Compila o projeto no modo de release e coloca os arquivos de compilação na pasta /app/build dentro do container

  

FROM build AS publish //Publicação do projeto

RUN dotnet publish "d1000.FrenteLoja/d1000.FrenteLoja/d1000.FrenteLoja.csproj" -c Release -o /app/publish

  

FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS final //Utiliza uma imagem base diferente, mais leve, que contém apenas o runtime do ASP.NET Core

WORKDIR /app

COPY --from=publish /app/publish .
 
ENTRYPOINT ["dotnet", "d1000.FrenteLoja.dll"]

```

### Dockerfile Angular 

```
# Etapa 1: Construir a aplicação Angular
FROM node:16 AS build

# Definir o diretório de trabalho
WORKDIR /app

# Copiar o package.json e package-lock.json para o container
COPY package*.json ./

# Instalar as dependências
RUN npm install

# Copiar o restante do código da aplicação
COPY . .

# Rodar o build da aplicação Angular
RUN npm run build --prod

# Etapa 2: Servir a aplicação usando um servidor HTTP estático (Nginx)
FROM nginx:alpine

# Copiar os arquivos de build da etapa anterior para o diretório de arquivos estáticos do Nginx
COPY --from=build /app/dist/ /usr/share/nginx/html

# Expor a porta 80
EXPOSE 80

# Rodar o Nginx
CMD ["nginx", "-g", "daemon off;"]

```