
Usamos services, para regras de negocio da api, isolando esses processos da controller
- Controller geralmente cuida mais da parte da entrada do dado do usuário. ex: O dados não seu nulo
- Service geralmente cuida da regra de negocio. ex: O processo de update, a parte de atualizar e se os dados na atualização forem vazios e so um deles estiverem preenchidos e atualiza apenas esse campo, enquanto o resto vai ser oq ja estava no banco, com o proposito de não deixar algum campo vazio.

