# Estabilizar `npm test`

O comando `npm test` será utilizado durante os testes do CI e, assim como o comando `npm run dev`, deve ser capaz de subir toda a infraestrutura necessária (banco de dados), executar as migrations e iniciar o serviço do Next.js.

Dito isso, instale os seguintes módulos:

```bash
npm install concurrently@8.2.2 -D
```

```bash
npm install async-retry@1.3.3
```

Agora, no arquivo `package.json`, remova o script `test`:

```json
"test": "jest --runInBand",
```

e adicione o seguinte script:

```json
"test": "npm run services:up && npm run wait-for-postgres && concurrently -n next,jest --hide next -k -s command-jest 'next dev' 'jest --runInBand'",
```

Teoricamente, essas alterações já resolveriam o problema. No entanto, durante a execução do script `test`, o serviço do Next.js é iniciado em paralelo com o Jest. Sendo assim, dependendo do ambiente, pode levar alguns segundos até que o serviço do Next.js esteja pronto para receber requisições, enquanto o Jest pode iniciar imediatamente a execução da bateria de testes.

Portanto, é necessário um **orquestrador** para fazer com que o Jest somente execute a bateria de testes quando o serviço do Next.js estiver pronto.

Dito isso, crie o arquivo `tests/orchestrator.js` e adicione o seguinte código:

```js
import retry from "async-retry";

async function waitForAllServices() {
  await waitForWebServer();

  async function waitForWebServer() {
    return retry(fetchStatusPage, {
      retries: 100,
    });

    async function fetchStatusPage() {
      const response = await fetch("http://localhost:3000/api/v1/status");
      const responseBody = await response.json();
    }
  }
}

export default {
  waitForAllServices,
};
```

Além disso, o Jest possui um **timeout** padrão de 5000 ms. Precisamos aumentá-lo para 60000 ms.

No arquivo `jest.config.js`, logo após a linha `moduleDirectories: ["node_modules", "<rootDir>"],`, adicione a seguinte propriedade:

```js
testTimeout: 60000,
```

Agora, nos arquivos de testes, precisamos importar o `orchestrator.js` e fazer o Jest aguardar até o término da execução da função `waitForAllServices`.

No arquivo `tests/integration/api/v1/status/get.test.js`, adicione no topo do arquivo o seguinte trecho de código:

```js
import orchestrator from "tests/orchestrator.js";

beforeAll(async () => {
  await orchestrator.waitForAllServices();
});
```

No arquivo `tests/integration/api/v1/migrations/get.test.js`, faça as seguintes alterações:

- Remova o seguinte trecho de código:

```js
beforeAll(cleanDatabase);

async function cleanDatabase() {
  await database.query("drop schema public cascade; create schema public;");
}

database.query("SELECT 1+1 as sum;");
```

- No lugar do trecho removido, adicione o seguinte código:

```js
import orchestrator from "tests/orchestrator.js";

beforeAll(async () => {
  await orchestrator.waitForAllServices();
  await database.query("drop schema public cascade; create schema public;");
});
```

No arquivo `tests/integration/api/v1/migrations/post.test.js`, faça as seguintes alterações:

- Remova o seguinte trecho de código:

```js
beforeAll(cleanDatabase);

async function cleanDatabase() {
  await database.query("drop schema public cascade; create schema public;");
}
```

- No lugar do trecho removido, adicione o seguinte código:

```js
import orchestrator from "tests/orchestrator.js";

beforeAll(async () => {
  await orchestrator.waitForAllServices();
  await database.query("drop schema public cascade; create schema public;");
});
```

Faça o commit das alterações:

```bash
git add -A
git commit -m 'make `npm test` more robust with `async-retry` and `orchestrator.js`'
git push
```

---

[← Voltar ao sumário](README.md)
