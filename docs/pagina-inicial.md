# Página inicial

Crie o diretório `pages` na raiz do projeto e, dentro dele, crie o arquivo `index.js`, conforme a estrutura abaixo:

```text
pages/
└── index.js
```

No arquivo `index.js`, adicione o seguinte código:

```js
function Home() {
  return <h1>Em Construção</h1>;
}

export default Home;
```

Em seguida, no arquivo `package.json`, remova o seguinte script:

```json
"test": "echo \"Error: no test specified\" && exit 1"
```

Agora, ainda no arquivo `package.json`, adicione o seguinte script:

```json
"dev": "next dev"
```

Crie o arquivo `.gitignore` na raiz do projeto e adicione os seguintes arquivos e diretórios:

```text
node_modules
.next
```

O arquivo `.gitignore` informa ao Git quais arquivos e diretórios não devem ser rastreados pelo controle de versão. Nesse caso, estamos ignorando as dependências instaladas em `node_modules` e os arquivos gerados pelo Next.js no diretório `.next`.

Salve as alterações e adicione o arquivo `.gitignore` à área de staging:

```bash
git add .gitignore
```

Em seguida, crie o commit:

```bash
git commit -m "add .gitignore file"
```

E por fim empurre as alterações para o repositório de origem:

```bash
git push
```

Caso queira consultar este é o link do commit no GitHub:

[Commit fafb305](https://github.com/thiagogracini/tabnews-clone/commit/fafb3057cfdf31bba7e86abbf3a5d233f2240577)

Agora vamos realizar o commit dos demais arquivos:

```bash
git add .nvmrc
git commit -m 'add file `.nvmrc`'
git push
```

```bash
git add package.json package-lock.json
git commit -m 'add manifest files'
git push
```

```bash
git add pages
git commit -m 'add initial page'
git push
```

---

[← Voltar ao sumário](README.md)
