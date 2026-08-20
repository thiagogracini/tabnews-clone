# Reproduzindo e corrigindo o Bug em Homologação

O endpoint `/api/v1/migrations` possui um bug que ocorre quando uma requisição falha ou quando um usuário realiza uma requisição utilizando qualquer método HTTP diferente de `GET` ou `POST`. Nesses casos, a conexão com o banco de dados pode ser aberta, mas não é encerrada, permanecendo ativa.

Dito isso, vamos refatorar o arquivo `api/v1/migrations/index.js`. Como boa parte do arquivo sofreu alterações, não compensa modificar manualmente cada linha. Portanto, substitua todo o conteúdo do arquivo pelo código abaixo:

```js
import migrationRunner from "node-pg-migrate";
import { join } from "node:path";
import database from "infra/database.js";

export default async function migrations(request, response) {
  const allowedMethods = ["GET", "POST"];
  if (!allowedMethods.includes(request.method)) {
    return response.status(405).json({
      error: `Method "${request.method}" Not Allowed`,
    });
  }

  let dbClient;

  try {
    dbClient = await database.getNewClient();

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
      return response.status(200).json(pendingMigrations);
    }

    if (request.method === "POST") {
      const migratedMigrations = await migrationRunner({
        ...defaultMigrationOptions,
        dryRun: false,
      });

      if (migratedMigrations.length > 0) {
        return response.status(201).json(migratedMigrations);
      }

      return response.status(200).json(migratedMigrations);
    }
  } catch (error) {
    console.error(error);
    throw error;
  } finally {
    await dbClient.end();
  }
}
```

Agora faça o commit das alterações:

```bash
git add -A
git commit -m 'fix `/migrations` endpoint bug'
git push
```

---

[← Voltar ao sumário](README.md)
