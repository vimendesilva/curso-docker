**Lista de comandos utilizados**

`docker version` - exibe a versão do docker.

`docker run NOME_DA_IMAGEM` - cria um container com a respectiva imagem passada como parâmetro.

`docker ps` - exibe todos os containers em execução no momento.

`docker ps -a` - exibe todos os containers, independentemente de estarem em execução ou não.

`docker run -it NOME_DA_IMAGEM` - conecta o terminal que estamos utilizando com o do container.

`docker start ID_CONTAINER` - inicia o container com id em questão.

`docker stop ID_CONTAINER` - interrompe o container com id em questão.

`docker start -a -i ID_CONTAINER` - inicia o container com id em questão e integra os terminais, além de permitir interação entre ambos.

`docker rm ID_CONTAINER` - remove o container com id em questão.

`docker container prune` - remove todos os containers que estão parados.

`docker rmi NOME_DA_IMAGEM` - remove a imagem passada como parâmetro.

`docker run -d -P --name NOME dockersamples/static-site` - ao executar, dá um nome ao container.

`docker run -d -p 12345:80 dockersamples/static-site` - define uma porta específica para ser atribuída à porta 80 do container, neste caso 12345.

`docker run -d -e AUTHOR="Fulano" dockersamples/static-site` - define uma variável de ambiente AUTHOR com o valor Fulano no container criado.

`docker run -v "CAMINHO_VOLUME" NOME_DA_IMAGEM` - cria um volume no respectivo caminho do container.

`docker inspect ID_CONTAINER` - retorna diversas informações sobre o container.

`docker build -f Dockerfile` - cria uma imagem a partir de um Dockerfile.

`docker build -f CAMINHO_DOCKERFILE/Dockerfile -t NOME_USUARIO/NOME_IMAGEM` - constrói e nomeia uma imagem não-oficial informando o caminho para o Dockerfile.

`docker login` - inicia o processo de login no Docker Hub.

`docker push NOME_USUARIO/NOME_IMAGEM` - envia a imagem criada para o Docker Hub.

`docker pull NOME_USUARIO/NOME_IMAGEM` - baixa a imagem desejada do Docker Hub.
