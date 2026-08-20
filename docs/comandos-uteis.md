# Anexo A - Comandos úteis

## npm

`npm init` - Cria o arquivo `package.json`.

`npm install` - Instala todos os pacotes especificados no arquivo `package.json` e suas respectivas dependências.

`npm install pacote@versao` - Instala um pacote na versão especificada. Caso nenhuma versão seja informada, será instalada a versão mais recente do pacote.

`npm install pacote -D` - Instala um pacote como depedência de desenvolvimento.

`npm run test:watch -- migrations` - Executa, em modo watch, somente os testes cujo caminho corresponde ao padrão `migrations`.

`npm run test:watch -- migrations.post` - Executa, em modo watch, somente os testes cujo caminho corresponde ao padrão `migrations.post`.

## nvm

`nvm install` - Instala a versão do Node.js especificada no arquivo `.nvmrc`.

`nvm alias default lts/hydrogen` - Define a versão `lts/hydrogen` como a versão padrão do Node.js.

`nvm use` - Passa a utilizar a versão do Node.js especificada no arquivo `.nvmrc`.

## git

`git log` - Exibe o histórico de commits do repositório.

`git log --oneline` - Exibe o histórico de commits do repositório de forma resumida, mostrando cada commit em uma única linha com seu identificador abreviado e a respectiva mensagem.

`git log --stat` - Exibe o histórico de commits, incluindo um resumo dos arquivos alterados e a quantidade de linhas adicionadas e removidas em cada commit.

`git reflog` - Exibe o histórico das alterações realizadas nas referências locais do repositório, como movimentações do `HEAD` e das branches, sendo útil para localizar e recuperar commits que não aparecem mais no histórico atual.

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

`git restore arquivo` - Descarta as alterações realizadas em um arquivo e restaura seu conteúdo para o estado do último commit.

`git branch` - Exibe as branches existentes.

`git branch nome-da-branch` - Cria uma nova branch.

`git branch -d nome-da-branch` - Remove a branch informada.

`git branch -D nome-da-branch` - Força a remoção da branch informada.

`git checkout nome-da-branch` - Altera para a branch informada.

`git checkout -b nome-da-branch` - Cria uma nova branch e já altera o ponteiro head para ela.

`git switch nome-da-branch` - Altera para a branch informada.

`git merge branch-a-ser-mesclada` - Mescla as alterações da branch informada na branch atual.

## Docker

`docker ps` - Lista os containers que estão em execução.

`docker ps -a` - Lista todos os containers, incluindo os que estão em execução e os que já foram encerrados.

`docker compose up` - Lê o arquivo `compose.yaml` e cria e inicia os serviços definidos nele.

`docker compose up --detach` - Cria e inicia os serviços definidos no arquivo `compose.yaml` em segundo plano.

`docker compose up -d --force-recreate` - Recria os containers antes de iniciá-los, mesmo que suas configurações ou imagens não tenham sido alteradas, e os executa em segundo plano.

`docker compose -f infra/compose.yaml up` - Cria e inicia os serviços utilizando o arquivo `compose.yaml` localizado no diretório `infra`.

`docker system prune -a` - Remove containers parados, redes não utilizadas, imagens não utilizadas por containers e cache de build. Por padrão, não remove volumes.

## Terminal

`clear` - Limpa o terminal

`Ctrl + L` - Limpa o terminal

---

[← Voltar ao sumário](README.md)
