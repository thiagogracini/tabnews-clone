# Anexo A - Comandos úteis

## npm

`npm init` - Cria o arquivo `package.json`.

`npm install` - Instala todos os pacotes especificados no arquivo `package.json` e suas respectivas dependências.

`npm install pacote@versao` - Instala um pacote na versão especificada. Caso nenhuma versão seja informada, será instalada a versão mais recente do pacote.

`npm install pacote -D` - Instala um pacote como depedência de desenvolvimento.

## nvm

`nvm install` - Instala a versão do Node.js especificada no arquivo `.nvmrc`.

`nvm alias default lts/hydrogen` - Define a versão `lts/hydrogen` como a versão padrão do Node.js.

`nvm use` - Passa a utilizar a versão do Node.js especificada no arquivo `.nvmrc`.

## git

`git log` - Exibe o histórico de commits do repositório.

`git log --oneline` - Exibe o histórico de commits do repositório de forma resumida, mostrando cada commit em uma única linha com seu identificador abreviado e a respectiva mensagem.

`git log --stat` - Exibe o histórico de commits, incluindo um resumo dos arquivos alterados e a quantidade de linhas adicionadas e removidas em cada commit.

`git status` - Exibe o estado atual do repositório, mostrando arquivos modificados, adicionados, removidos e quais alterações estão ou não na área de staging.

`git add arquivo` - Adiciona as alterações de um arquivo à área de staging, deixando-as prontas para serem incluídas no próximo commit.

`git commit -m "mensagem do commit"` - Cria um novo commit com as alterações que estão na área de staging e associa a ele a mensagem informada.

`git commit -am "mensagem do commit"` - Adiciona automaticamente ao staging os arquivos já rastreados pelo Git que foram modificados ou removidos e, em seguida, realiza o commit. Arquivos novos ainda precisam ser adicionados com `git add`.

`git commit --amend` - Permite alterar o último commit realizado, seja para modificar sua mensagem ou para incluir novas alterações que não foram adicionadas anteriormente.

`git push` - Envia os commits do repositório local para o repositório remoto, que, no nosso caso, está hospedado no GitHub.

`git push --force` - Envia os commits do repositório local para o repositório remoto, forçando a atualização do histórico remoto mesmo quando ele diverge do histórico local. Deve ser utilizado com cuidado, pois pode sobrescrever commits existentes no repositório remoto.

`git pull` - Baixa os commits do repositório remoto e integra as alterações à branch local atual.

`git diff` - Exibe as alterações realizadas nos arquivos que ainda não foram adicionadas à área de staging, mostrando as diferenças em relação à última versão registrada pelo Git.

`git mv` - Comando utilizado para mover um arquivo do repositório para outro diretório ou para renomeá-lo.

## Docker

`docker ps` - Lista os containers que estão em execução.

`docker ps -a` - Lista todos os containers, incluindo os que estão em execução e os que já foram encerrados.

`docker compose up` - Lê o arquivo `compose.yaml` e cria e inicia os serviços definidos nele.

`docker compose up --detach` - Cria e inicia os serviços definidos no arquivo `compose.yaml` em segundo plano.

`docker compose up -d --force-recreate` - Recria os containers antes de iniciá-los, mesmo que suas configurações ou imagens não tenham sido alteradas, e os executa em segundo plano.

`docker compose -f infra/compose.yaml up` - Cria e inicia os serviços utilizando o arquivo `compose.yaml` localizado no diretório `infra`.

## Terminal

`clear` - Limpa o terminal

`Ctrl + L` - Limpa o terminal

---

[← Voltar ao sumário](README.md)
