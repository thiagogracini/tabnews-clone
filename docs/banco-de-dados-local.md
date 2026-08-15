# Subir o banco de dados local

Crie o arquivo `infra/compose.yaml`, conforme a estrutura abaixo:

```text
infra/
└── compose.yaml
```

No arquivo `compose.yaml`, adicione o seguinte conteúdo:

```yaml
services:
  database:
    image: "postgres:16.0-alpine3.18"
    environment:
      POSTGRES_PASSWORD: "local_password"
    ports:
      - "5432:5432"
```

Por fim, faça o commit das alterações.

```bash
git add -A
git commit -m 'add file `compose.yaml`'
git push
```

---

[← Voltar ao sumário](README.md)
