# Arquivo `.env.development`

Renomeie o arquivo `.env` para `.env.development`:

```bash
git mv .env .env.development
```

No arquivo `infra/compose.yaml` altere a linha `- ../.env` para:

```yaml
- ../.env.development
```

Faça o commit das alterações:

```bash
git add -A
git commit -m 'move file `.env` to `.env.development`'
git push
```

---

[← Voltar ao sumário](README.md)
