# Configurar EditorConfig

Crie na raíz do projeto o arquivo `.editorconfig` e adicione o seguinte código:

```text
root = true

[*]
indent_style = space
indent_size = 2
```

Por padrão o VSCode não lê o editor config para aplicar as configurações definidas no arquivo, sendo assim é necessário instalar a extensão `EditorConfig`

Assim que instalar a extensão basta fazer o commit das alterações:

```bash
git add .editorconfig
git commit -m 'add file `.editorconfig`'
git push
```

---

[← Voltar ao sumário](README.md)
