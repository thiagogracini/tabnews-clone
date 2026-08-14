# Instalar um Test Runner

Instale o Jest, que será o framework de testes utilizado no projeto:

```bash
npm install jest@29.6.2 -D
```

No arquivo `package.json` na sessão scripts, logo após a linha `"lint:fix": "prettier --write ."` adicione os seguintes scripts:

```json
"test": "jest",
"test:watch": "jest --watch"
```

Faça o commit das alterações:

```bash
git add -A
git commit -m 'install `jest` and configure test scripts'
git push
```

---

[← Voltar ao sumário](README.md)
