# Criar módulo "database.js"

Instale o pacote `pg`:

```bash
npm install pg@8.11.3
```

No arquivo `package.json`, altere a linha `"test:watch": "jest --watch"` para a declaração abaixo, para que, sempre que alguma alteração for realizada, todos os testes sejam executados.

```json
"test:watch": "jest --watchAll"
```

No diretório `infra`, crie o arquivo `database.js` e adicione o seguinte trecho de código:

```js
import { Client } from "pg";

async function query(queryObject) {
  const client = new Client({
    host: process.env.POSTGRES_HOST,
    port: process.env.POSTGRES_PORT,
    user: process.env.POSTGRES_USER,
    database: process.env.POSTGRES_DB,
    password: process.env.POSTGRES_PASSWORD,
  });
  await client.connect();
  const result = await client.query(queryObject);
  await client.end();
  return result;
}

export default {
  query: query,
};
```

Substitua todo o conteúdo do arquivo `pages/api/v1/status/index.js` pelo seguinte:

```js
import database from "../../../../infra/database.js";

async function status(request, response) {
  const result = await database.query("SELECT 1 + 1 as sum;");
  console.log(result.rows);
  response.status(200).json({ chave: "são acima da média" });
}

export default status;
```

Crie, na raiz do projeto, o arquivo `.env` e adicione as seguintes variáveis:

```text
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=local_user
POSTGRES_DB=local_db
POSTGRES_PASSWORD=local_password
```

No arquivo `infra/compose.yaml`, remova as seguintes linhas:

```yaml
environment:
  POSTGRES_PASSWORD: "local_password"
```

Em seguida, no arquivo `infra/compose.yaml`, adicione as seguintes linhas no lugar das que foram removidas:

```yaml
env_file:
  - ../.env
```

Dito isso faça o commit das alterações:

```bash
git add -A
git commit -m 'add files `database.js` and `.env`'
git push
---

[← Voltar ao sumário](README.md)
```
