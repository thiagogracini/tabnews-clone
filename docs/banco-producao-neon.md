# Banco de Produção no Neon

Crie uma conta no Neon Tech, depois crie um banco de dados e, por fim, configure as variáveis de ambiente do seu projeto na Vercel.

Como o banco de dados no Neon exige uma conexão SSL faça as seguintes alterações no arquivo `infra/database.js`

- No objeto `client`, adicione a propriedade `ssl`:

```js
ssl: process.env.NODE_ENV === "development" ? false : true,
```

Ao final da alteração, o objeto deverá estar assim:

```js
const client = new Client({
  host: process.env.POSTGRES_HOST,
  port: process.env.POSTGRES_PORT,
  user: process.env.POSTGRES_USER,
  database: process.env.POSTGRES_DB,
  password: process.env.POSTGRES_PASSWORD,
  ssl: process.env.NODE_ENV === "development" ? false : true,
});
```

Faça o commit das alterações:

```bash
git add -A
git commit -m 'handle database ssl connection'
git push
```

---

[← Voltar ao sumário](README.md)
