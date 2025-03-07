### Comandos básicos

- Criar container ubunto
```
docker run -it ubunto //Para criar um conteiner onde temos uma máquina ubunto

```

- Atualizar os pacotes
```
apt-get update
```

- Instalar algum pacote (ex. VIM)
```
apt-get install vim
```

Para sair de um container use `exit` onde ele sera parado instantaneamente 

- Criar um container e deixar rodando pelos fundos(background)
```
docker run -d node
```

- Para um container em segundo plano
```
docker stop <tag ou nome do container>
```

- Iniciar um container ja criado
```
docker start <tag ou nome do container>
```

- Acessar um container em segundo plano 
```
docker exec -it <tag ou nome do container> /bin/sh
```




$$

$$





