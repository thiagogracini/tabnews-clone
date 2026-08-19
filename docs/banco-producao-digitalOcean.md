# Banco de Produção na DigitalOcean

Para se conectar ao banco de dados da DigitalOcean, é necessário utilizar o certificado digital fornecido por ela. Sendo assim, é necessário tornar o módulo `database.js` mais robusto, para que seja capaz de lidar com certificados digitais. Para isso, faça as seguintes alterações:

Altere a linha `ssl: process.env.NODE_ENV === "development" ? false : true` para:

```js
ssl: getSSLValues();
```

Em seguida, crie, no final do arquivo, a função `getSSLValues`:

```js
function getSSLValues() {
  if (process.env.POSTGRES_CA) {
    return {
      ca: process.env.POSTGRES_CA,
    };
  }

  return process.env.NODE_ENV === "development" ? false : true;
}
```

Agora, faça o commit das alterações:

```bash
git add -A
git commit -m 'add a more robust ssl configuration method'
git push
```

---

[← Voltar ao sumário](README.md)
