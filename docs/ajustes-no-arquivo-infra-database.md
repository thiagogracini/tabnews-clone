# Ajustes no arquivo `infra/database.js`

No arquivo `infra/database.js`, faça as seguintes alterações:

- Remova a seguinte linha:

```js
await client.connect();
```

- Adicione a linha removida dentro do bloco `try`, antes da execução da query:

```js
try {
  await client.connect();
  const result = await client.query(queryObject);
  return result;
```

- No bloco `catch`, adicione a seguinte linha após `console.error(error);` para que o erro seja propagado:

```js
throw error;
```

Após as alterações, o bloco deverá ficar da seguinte forma:

```js
try {
  await client.connect();
  const result = await client.query(queryObject);
  return result;
} catch (error) {
  console.error(error);
  throw error;
} finally {
  await client.end();
}
```

Faça o commit das alterações:

```bash
git add -A
git commit -m 'make `database.js` `client.connect()` more robust to errors'
git push
```

---

[← Voltar ao sumário](README.md)
