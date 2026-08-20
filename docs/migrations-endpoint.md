# Migrations Endpoint

Para a execução sucessiva das migrations por meio dos testes automatizados, é necessário que os testes sejam executados de forma serial. Por padrão, o Jest executa os testes de forma paralela, sendo assim precisamos alterar o modo de execução dos testes para serial. Para isso, no arquivo `package.json`, na seção `scripts`, faça as seguintes alterações:

- Remova as linhas:

```json
"test": "jest",
"test:watch": "jest --watchAll",
```

- Adicione as seguintes linhas:

```json
"test": "jest --runInBand",
"test:watch": "jest --watchAll --runInBand",
```

Para que o Jest seja capaz de limpar o banco de dados de forma automática, ele precisa conseguir se conectar ao banco. Como já temos o módulo `database.js`, responsável por realizar as conexões, podemos importá-lo dentro dos arquivos de teste.

No entanto, precisamos configurar o Jest para utilizar corretamente as configurações do Next.js e reconhecer as importações utilizadas pelo projeto. Para isso, crie na raiz do projeto o arquivo `jest.config.js` e adicione o seguinte código:

```js
const dotenv = require("dotenv");
dotenv.config({ path: ".env.development" });

const nextJest = require("next/jest");

const createJestConfig = nextJest({
  dir: ".",
});
const jestConfig = createJestConfig({
  moduleDirectories: ["node_modules", "<rootDir>"],
});

module.exports = jestConfig;
```

Como o Jest roda no ambiente de `test`, será necessário realizar as seguintes refatorações no arquivo `infra/database.js`:

- Remova de dentro da função `query` as seguintes linhas:

```js
const client = new Client({
  host: process.env.POSTGRES_HOST,
  port: process.env.POSTGRES_PORT,
  user: process.env.POSTGRES_USER,
  database: process.env.POSTGRES_DB,
  password: process.env.POSTGRES_PASSWORD,
  ssl: getSSLValues(),
});
```

- Em seguida, declare a variável `client` antes do bloco `try`:

```js
let client;
```

- Dentro do bloco `try`, remova a linha:

```js
await client.connect();
```

e adicione a seguinte linha:

```js
client = await getNewClient();
```

- Imediatamente antes da declaração `export default`, crie a função assíncrona `getNewClient`, responsável por criar uma nova instância do `Client`, realizar a conexão com o banco de dados e retornar o cliente conectado:

```js
async function getNewClient() {
  const client = new Client({
    host: process.env.POSTGRES_HOST,
    port: process.env.POSTGRES_PORT,
    user: process.env.POSTGRES_USER,
    database: process.env.POSTGRES_DB,
    password: process.env.POSTGRES_PASSWORD,
    ssl: getSSLValues(),
  });

  await client.connect();
  return client;
}
```

- No objeto exportado pelo módulo, remova a linha:

```js
query: query,
```

e adicione as seguintes linhas:

```js
  query,
  getNewClient,
```

- Na função `getSSLValues`, altere a verificação do ambiente de:

```js
return process.env.NODE_ENV === "development" ? false : true;
```

para:

```js
return process.env.NODE_ENV === "production" ? true : false;
```

Essa alteração é necessária porque, durante a execução dos testes, o valor de `NODE_ENV` será `test`. Dessa forma, a conexão local também será realizada sem SSL, enquanto o SSL continuará habilitado no ambiente de produção.

Crie a migration `test migration` dentro do diretório `infra/migrations` com o seguinte comando:

```bash
npm run migration:create test migration
```

Crie o arquivo `pages/api/v1/migrations/index.js` e adicione o seguinte código:

```js
import migrationRunner from "node-pg-migrate";
import { join } from "node:path";
import database from "infra/database.js";

export default async function migrations(request, response) {
  const dbClient = await database.getNewClient();

  const defaultMigrationOptions = {
    dbClient: dbClient,
    dryRun: true,
    dir: join("infra", "migrations"),
    direction: "up",
    verbose: true,
    migrationsTable: "pgmigrations",
  };

  if (request.method === "GET") {
    const pendingMigrations = await migrationRunner(defaultMigrationOptions);
    await dbClient.end();
    return response.status(200).json(pendingMigrations);
  }

  if (request.method === "POST") {
    const migratedMigrations = await migrationRunner({
      ...defaultMigrationOptions,
      dryRun: false,
    });

    await dbClient.end();

    if (migratedMigrations.length > 0) {
      return response.status(201).json(migratedMigrations);
    }

    return response.status(200).json(migratedMigrations);
  }

  return response.status(405);
}
```

Crie os arquivos de teste `tests/integration/api/v1/migrations/get.test.js` e `tests/integration/api/v1/migrations/post.test.js` com os seguintes trechos de código:

- Arquivo `tests/integration/api/v1/migrations/get.test.js`:

```js
import database from "infra/database.js";

beforeAll(cleanDatabase);

async function cleanDatabase() {
  await database.query("drop schema public cascade; create schema public;");
}

database.query("SELECT 1+1 as sum;");

test("GET to /api/v1/migrations should return 200", async () => {
  const response = await fetch("http://localhost:3000/api/v1/migrations");
  expect(response.status).toBe(200);

  const responseBody = await response.json();
  console.log(responseBody);

  expect(Array.isArray(responseBody)).toBe(true);
  expect(responseBody.length).toBeGreaterThan(0);
});
```

- Arquivo `tests/integration/api/v1/migrations/post.test.js`:

```js
import database from "infra/database.js";

beforeAll(cleanDatabase);

async function cleanDatabase() {
  await database.query("drop schema public cascade; create schema public;");
}

test("POST to /api/v1/migrations should return 200", async () => {
  const response1 = await fetch("http://localhost:3000/api/v1/migrations", {
    method: "POST",
  });
  expect(response1.status).toBe(201);

  const response1Body = await response1.json();
  console.log(response1Body);

  expect(Array.isArray(response1Body)).toBe(true);
  expect(response1Body.length).toBeGreaterThan(0);

  const response2 = await fetch("http://localhost:3000/api/v1/migrations", {
    method: "POST",
  });
  expect(response2.status).toBe(200);

  const response2Body = await response2.json();
  console.log(response2Body);

  expect(Array.isArray(response2Body)).toBe(true);
  expect(response2Body.length).toBe(0);
});
```

O Jest utiliza a connection string disponível na variável de ambiente `DATABASE_URL` para se conectar ao banco de dados. Essa variável já existe no nosso projeto, porém atualmente os valores de conexão estão repetidos diretamente nela.

Para permitir que a variável `DATABASE_URL` reutilize outras variáveis definidas no arquivo `.env.development`, vamos instalar o pacote `dotenv-expand`:

```bash
npm install dotenv-expand@11.0.6
```

Após instalar o pacote `dotenv-expand`, no arquivo `.env.development`, faça a seguinte alteração:

- Remova a linha:

```text
DATABASE_URL=postgres://local_user:local_password@localhost:5432/local_db
```

- Adicione a linha:

```text
DATABASE_URL=postgres://$POSTGRES_USER:$POSTGRES_PASSWORD@$POSTGRES_HOST:$POSTGRES_PORT/$POSTGRES_DB
```

Com o `dotenv-expand`, as referências às outras variáveis serão expandidas ao carregar o arquivo `.env.development`, evitando a duplicação dos dados de conexão dentro da variável `DATABASE_URL`.

Faça o commit das alterações:

```bash
git add -A
git commit -m 'add `api/v1/migrations` endpoint'
git push
```

---

[← Voltar ao sumário](README.md)
