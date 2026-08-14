# Versionamento da API e endpoint `/status`

Para realizar o versionamento da nossa API, utilizaremos a versão diretamente no caminho da URL. Por exemplo:

```text
https://tabnews-clone.thiagogracini.com.br/api/v1/status
```

Nesse caso, `v1` indica que estamos utilizando a versão 1 da API.

Sendo assim, dentro do diretório `pages`, crie a seguinte estrutura de diretórios e o arquivo `index.js`:

```text
pages/
└── api/
    └── v1/
        └── status/
            └── index.js
```

No arquivo `pages/api/v1/status/index.js`, adicione o seguinte código:

```js
function status(request, response) {
  response.status(200).json({ chave: "são acima da média" });
}

export default status;
```

Agora, na raiz do projeto, crie a seguinte estrutura de diretórios e o arquivo `get.test.js`:

```text
tests/
└── integration/
    └── api/
        └── v1/
            └── status/
                └── get.test.js
```

No arquivo `tests/integration/api/v1/status/get.test.js`, adicione o seguinte código:

```js
test("GET to /api/v1/status should return 200", async () => {
  const response = await fetch("http://localhost:3000/api/v1/status");
  expect(response.status).toBe(200);
});
```

Dito isso, faça o commit das alterações:

```bash
git add -A
git commit -m 'add `api/v1/status` endpoint'
git push
```

---

[← Voltar ao sumário](README.md)
