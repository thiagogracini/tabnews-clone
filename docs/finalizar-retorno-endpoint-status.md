# Finalizar retorno do endpoint `/status`

Substitua todo o conteúdo do arquivo `/pages/api/v1/status/index.js` pelo seguinte conteúdo:

```js
import database from "infra/database.js";

async function status(request, response) {
  const updatedAt = new Date().toISOString();

  const databaseVersionResult = await database.query("SHOW server_version;");
  const databaseVersionValue = databaseVersionResult.rows[0].server_version;

  const databaseMaxConnectionsResult = await database.query(
    "SHOw max_connections;",
  );
  const databaseMaxConnectionsValue =
    databaseMaxConnectionsResult.rows[0].max_connections;

  const databaseName = process.env.POSTGRES_DB;
  const databaseOpenedConnectionsResult = await database.query({
    text: "SELECT count(*)::int FROM pg_stat_activity WHERE datname = $1;",
    values: [databaseName],
  });
  const databaseOpenedConnectionsValue =
    databaseOpenedConnectionsResult.rows[0].count;

  response.status(200).json({
    updated_at: updatedAt,
    dependencies: {
      database: {
        version: databaseVersionValue,
        max_connections: parseInt(databaseMaxConnectionsValue),
        opened_connections: databaseOpenedConnectionsValue,
      },
    },
  });
}

export default status;
```

Substitua todo o conteúdo do arquivo `tests/integration/api/v1/status/get.test.js` pelo seguinte conteúdo:

```js
test("GET to /api/v1/status should return 200", async () => {
  const response = await fetch("http://localhost:3000/api/v1/status");
  expect(response.status).toBe(200);

  const responseBody = await response.json();

  const parseUpdatedAt = new Date(responseBody.updated_at).toISOString();
  expect(responseBody.updated_at).toEqual(parseUpdatedAt);

  expect(responseBody.dependencies.database.version).toEqual("16.0");
  expect(responseBody.dependencies.database.max_connections).toEqual(100);
  expect(responseBody.dependencies.database.opened_connections).toEqual(1);
});
```

No arquivo `infra/database.js`, faça as seguintes alterações:

- Remova as seguintes linhas:

```js
const result = await client.query(queryObject);
await client.end();
return result;
```

- Adicione as seguintes linhas no lugar das linhas removidas:

```js
try {
  const result = await client.query(queryObject);
  return result;
} catch (error) {
  console.error(error);
} finally {
  await client.end();
}
```

Primeiro, faça o commit das alterações no arquivo `infra/database.js`:

```bash
git add infra/database.js
git commit -m 'make `database.js` more robust to error handling'
git push
```

Depois, faça o commit das demais alterações:

```bash
git add -A
git commit -m 'finish `/api/v1/status` endpoint'
git push
```

---

[← Voltar ao sumário](README.md)
