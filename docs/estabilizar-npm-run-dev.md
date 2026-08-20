# Estabilizar `npm run dev`

Para que o comando `npm run dev` seja autossuficiente para:

- Iniciar o container do Postgres.
- Aguardar até que o container esteja pronto e aceitando conexões.
- Executar as migrations.
- Iniciar o Next.js e deixá-lo pronto para receber requisições dos usuários.

Precisamos "estabilizar" esse comando. Antes disso, vamos definir um nome para o container do Postgres.

No arquivo `infra/compose.yaml`, logo abaixo da linha `database:`, adicione a seguinte declaração:

```yaml
container_name: "postgres-dev"
```

Agora crie o arquivo `infra/scripts/wait-for-postgres.js`. Esse script será responsável por verificar se o Postgres foi iniciado e está pronto para aceitar conexões:

```js
const { exec } = require("node:child_process");

function checkPostgres() {
  exec("docker exec postgres-dev pg_isready --host localhost", handleReturn);

  function handleReturn(error, stdout) {
    if (stdout.search("accepting connections") === -1) {
      process.stdout.write(".");
      checkPostgres();
      return;
    }
    console.log("\n🟢 Postgres está pronto e aceitando conexões!\n");
  }
}

process.stdout.write("\n\n🔴 Aguardando Postgres aceitar conexãoes");
checkPostgres();
```

No arquivo `package.json` remova a linha:

```json
"migration:up": "node-pg-migrate -m infra/migrations --envPath .env.development up"
```

e adicione as seguintes linhas:

```json
"migration:up": "node-pg-migrate -m infra/migrations --envPath .env.development up",
"wait-for-postgres": "node infra/scripts/wait-for-postgres.js"
```

Ainda no arquivo `package.json` remova a linha:

```json
"dev": "npm run services:up && next dev",
```

e adicione a seguinte linha:

```json
"dev": "npm run services:up && npm run wait-for-postgres && npm run migration:up && next dev",
```

Faça o commit das alterações:

```bash
git add -A
git commit -m 'add `wait-for-postgres.js` script'
git push
```

---

[← Voltar ao sumário](README.md)
