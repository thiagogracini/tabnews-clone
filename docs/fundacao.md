# Fundação

Instale o Node.js com o seguinte comando:

```bash
nvm install lts/hydrogen
```

Em seguida, defina a versão `lts/hydrogen` como a versão padrão do Node.js:

```bash
nvm alias default lts/hydrogen
```

Agora, vamos criar o arquivo `.nvmrc` na raíz do projeto. Esse arquivo é utilizado para indicar qual versão do Node.js deve ser utilizada no projeto. Dessa forma, podemos garantir que todos os desenvolvedores utilizem a mesma versão do Node.js durante o desenvolvimento.

Adicione o seguinte conteúdo dentro do arquivo:

```txt
lts/hydrogen
```

Depois, sempre que você estiver no diretório do projeto, poderá executar:

```bash
nvm use
```

O NVM lerá o arquivo `.nvmrc` e utilizará a versão especificada nele.

---

[← Voltar ao sumário](README.md)
