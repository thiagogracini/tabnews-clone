# Migrations por Linha de Comando

Instale os módulos `dotenv` e `node-pg-migrate`:

```bash
npm install dotenv@16.4.4 node-pg-migrate@6.2.2
```

No arquivo `.env.development` adicione a seguinte variável de ambiente:

```text
DATABASE_URL=postgres://local_user:local_password@localhost:5432/local_db
```

No arquivo `package.json` na seção scripts adicione os seguintes scripts:

```json
"migration:create": "node-pg-migrate -m infra/migrations create",
"migration:up": "node-pg-migrate -m infra/migrations --envPath .env.development up"
```

Faça o commit das alterações:

```bash
git add .env.development package-lock.json package.json
git commit -m 'add migration scripts'
git push
```

---

[← Voltar ao sumário](README.md)
