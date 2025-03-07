## MySQL

appsettings.json

```
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost;Database=verify; User Id=root;Password=root;"
},
```

Program.cs

```
// Configurar o DbContext com a connection string
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseMySql(
        builder.Configuration.GetConnectionString("DefaultConnection"),
        ServerVersion.AutoDetect(builder.Configuration.GetConnectionString("DefaultConnection"))
    )
);
```

---
## PostgresSQL

appsettings.json

```
"ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Database=Product;Username=postgres;Password=0000;"
  }
```

Program.cs

```
// Configurar o DbContext com a connection string
builder.Services.AddDbContext<AplicationDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```
