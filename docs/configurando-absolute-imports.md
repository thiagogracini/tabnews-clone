# Configurar o "baseUrl" para "Absolute Imports"

Crie, na raiz do projeto, o arquivo `jsconfig.json` e adicione o seguinte trecho de código:

```json
{
  "compilerOptions": {
    "baseUrl": "."
  }
}
```

No arquivo `pages/api/v1/status/index.js`, remova a linha `import database from "../../../../infra/database.js";` e adicione a seguinte linha:

```js
import database from "infra/database.js";
```

Essas alterações farão com que o Next.js seja capaz de identificar o diretório raiz do projeto, permitindo que as importações sejam realizadas a partir dele.

Dito isso, faça o commit das alterações:

```bash
git add -A
git commit -m 'add file `jsconfig.json`'
git push
```

---

[← Voltar ao sumário](README.md)
