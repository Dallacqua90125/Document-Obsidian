### O que é

- CORS (Cross-Origin Resource Sharing) é um mecanismo de segurança do navegador que impede ou permite que uma aplicação web faça requisições para um domínio diferente daquele de onde a página foi carregada.
### Quando precisa ativar o CORS?

- Se você estiver desenvolvendo uma API que será consumida por um frontend hospedado em um domínio diferente, precisa configurar o CORS para permitir essas requisições.

> [!NOTE]
> 
> **Exemplo:**  
> Se sua API está rodando em `https://api.meuservico.com` e seu frontend em `https://meusite.com`, o navegador bloqueará as requisições da sua aplicação frontend para a API a menos que o CORS esteja configurado corretamente.

### Como ativar o CORS (ASP.NET)

program.cs
```
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAll",
        policy => policy.AllowAnyOrigin()
                        .AllowAnyMethod()
                        .AllowAnyHeader());
});
```

```

app.UseCors("AllowAll"); // Adicionado no final do program

app.UseHttpsRedirection();

app.UseAuthentication();

app.UseAuthorization();

app.MapControllers();

app.Run();
```