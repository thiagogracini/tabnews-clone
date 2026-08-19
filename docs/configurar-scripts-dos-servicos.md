# Configurar scripts dos serviços

No arquivo `package.json`, na seção `scripts`, adicione os seguintes scripts logo após o script `dev`:

```json
"services:up": "docker compose -f infra/compose.yaml up -d",
"services:stop": "docker compose -f infra/compose.yaml stop",
"services:down": "docker compose -f infra/compose.yaml down",
```

Em seguida faça o commit das alterações:

```bash
git commit -am 'add services scripts'
git push
```

---

[← Voltar ao sumário](README.md)
