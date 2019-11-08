# curso-docker

## **1. Instalação no Windows**

[Docker for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

Requisitos
- Arquitetura 64 bits
- Versão *Pro*, *Enterprise* ou *Education*.
- Virtualização habilitada

Seguimos o passo a passo do instalador para aceitar a licença, autorizamos o instalador e seguimos com a instalação. Ao clicar em *Finish*, precisamos encerrar a sessão do Windows e iniciá-la novamente. Ao fazer o login, precisamos habilitar o Hyper-V, clicando em Ok, para que o computador seja reiniciado.

> Hyper-V é uma tecnologia que permite a virtualização do hardware em um computador físico. Em outras palavras, é possível criar e gerenciar computadores virtuais e seus recursos, onde cada máquina virtual (VM) é considerada um sistema isolado.

O Docker pode demorar um pouco para inicializar, mas quando a mensagem Docker *is running* for exibida, significa que ele foi instalado com sucesso e já podemos utilizá-lo.

Para continuar os comandos, verifique se nas opções do docker, a opção 'Switch to Linux containers...' está habilitada.

*Hello World*

No terminal, podemos executar os seguintes comandos:
- `docker version` (verifica sua versão)
- `docker run hello-world` (executa o clássico *Hello World*)

Caso você tenha tido algum problema em instalar o Docker em sua máquina, você também tem a opção de utilizar o [Play With Docker](https://labs.play-with-docker.com/). Nele você poderá utilizar os comandos vistos daqui em diante e testar as diversas funcionalidades que o Docker proporciona. Ao acessar o site basta clicar em +Add New Instance e começar a utilizá-lo como estivesse usando sua máquina normalmente.

## **2. Trabalhando com as imagens**

Anteriormente, rodamos a imagem *hello-world* no container. 
Podemos executar a imagem do Ubuntu utilizando o comando `docker run ubuntu`

O container foi criado, o que acontece é que a imagem do Ubuntu não executa nada, por isso nenhuma mensagem foi exibida.
Podemos verificar isso vendo os containers que estão sendo executados no momento, executando o seguinte comando: `docker ps`

Para ver todos containers, inclusive os parados, adicionamos a flag -a ao comando acima: `docker ps -a`

Então, quando executamos o container do Ubuntu, precisamos passar para ele um comando que rode dentro dele, por exemplo:
`docker run ubuntu echo "Ola Mundo"`

Podemos fazer com que o terminal da nossa máquina seja integrado ao terminal de dentro do container, para ficar um terminal interativo, usando o comando `docker run -it ubuntu`.

Com isso, estamos trabalhando dentro do container. E dentro dele, podemos trabalhar como se estivéssemos trabalhando dentro do terminal de um Ubuntu, executando comandos como ls, cat, etc.

Agora, se abrirmos outro terminal e executar o comando docker ps, veremos o container que estamos executando. Podemos parar a sua execução, digitando no container o comando exit ou através do atalho CTRL + D.

- Executando novamente um container

Para iniciar um container que está parado, pegamos o id do container a ser iniciado, e passamos ao comando docker start: `docker start 05025384675e`
Para pausar o container, é usado o comando: `docker stop 05025384675e`

Para iniciar novamente o container, deixando o terminal interativo, é utilizado o comando docker start passando duas flags: -a, de *attach*, para integrar os terminais, e -i, de *interactive*, para interagirmos com o terminal, para podermos escrever nele: `docker start -a -i 05025384675e` 

## **2.1 Trabalhando com imagem site estático**

Utilizando o comando: `docker run -d -P dockersamples/static-site`, uma nova imagem nomeada 'static-site' pertencente ao usuário 'dockersamples' é baixada. A tag -P fará com que o Docker atribua uma porta aleatória do mundo externo, que no caso é a nossa máquina, para poder se comunicar com o que está dentro do container. E a tag -d fará com que o site rode em segundo plano, sem travar o terminal.

Ao executar o comando `docker ps`, é possível visualizar a porta (coluna PORTS) em que o site está rodando, para que possa ser acessada pelo navegador.

## **3. Usando Volumes:**

Vimos que os *containers*  funcionam como uma pequena camada de leitura e escrita, em cima das imagens, sendo somente para leitura. Quando removemos um container com o comando **docker rm**, a camada de leitura e escrita também é removida, o que faz com que os dados também sejam removidos. Por exemplo: pense em um banco de dados, toda vez que o container desse banco for removido, todos os nossos dados persistidos nele será perdido? Por isso precisamos ter um lugar para salvar esses dados, e esse lugar são os **Volumes**, já que é da natureza dos containers ser voláteis.

Quando criamos um volume de dados, criamos uma pasta dentro do container e estamos apontando essa pasta pra uma pequena pasta do **Docker Host**, ele é responsável por hostear a nossa máquina, onde a Docker Engine está rodando. E o que escrevermos dentro desta pasta estaremos escrevendo no Docker Host. Caso o container que criamos a pasta for removido, a pasta do Docker Host continua lá com os nossos dados.

Vamos praticar agora? No Terminal ou PowerShell, crie um container através do comando **docker run**, mas desta vez usando a flag **-v** para criar um volume, seguido do nome do mesmo:

        docker run -v "/var/www" ubuntu

Então criamos o volume **/var/www**, mas a que pasta no **Docker Host** ele faz referência? Para isso podemos inspecionar o container, executando o comando **docker inspect**, passando o seu **id** para o mesmo:

        docker inspect 8cf7b40ce226

Temos uma saída com várias informações, mas a que nos interessa é o **"Mounts"**:

        "Mounts": [
        {
        "Type": "volume",
        "Name": "5e1cbfd48d07284680552e56087c9d5196659600ccd6874bfa3831b51ddd0576",
        "Source": "/var/lib/docker/volumes/5e1cbfd48d07284680552e56087c9d5196659600ccd6874bfa3831b51ddd0576/_data",
        "Destination": "/var/www",
        "Driver": "local",
        "Mode": "",
        "RW": true,
        "Propagation": ""
        }
    ]
    
Nele, podemos ver que o **/var/www** será escrito na nossa máquina no diretório **/var/lib/docker/volumes/5e1cbfd48d07284680552e56087c9d5196659600ccd6874bfa3831b51ddd0576/_data**, endereço que foi gerado pelo Docker. Ou seja, tudo que escrevermos na pasta **/var/www**, estamos escrevendo na pasta do docker host da nossa máquina. Essa pasta gerada pelo Docker pode ser configurada, por exemplo se quisermos escrever dentro do Desktop da nossa máquina, devemos passá-lo antes do volume, separando-os com dois pontos. Vamos executar o container no modo iterativo:

        docker run -it -v "C:\Users\Alura\Desktop:/var/www" ubuntu
        root@abd0286c0083:/#
        
Então, quando escrevermos na pasta **/var/www**, estaremos escrevendo no Desktop da nossa máquina. Vamos fazer um teste?

        root@abd0286c0083:/# cd /var/www/
        root@abd0286c0083:/var/www# touch novo-arquivo.txt
        root@abd0286c0083:/var/www# echo "Este arquivo foi criado dentro de um volume" > novo-arquivo.txt
        
Quando acessarmos nosso Desktop, o arquivo estará lá com a mensagem escrita dentro. E ao remover o container, a sua camada de escrita é removida, mas os arquivos continuam no nosso Desktop. Podemos atrelar o volume com um lugar mais seguro, o que veremos daqui a pouco.

Percebemos que isso nos dá a possibilidade de implementar localmente um código de uma linguagem que não está instalada na nossa máquina, e colocá-lo para compilar e rodar dentro de um container. Se ele pussuir Node, Java, PHP, nosso ambiente de desenvolvimento será dentro de dele mesmo. Então vamos lá!

Iremos usar um exemplo escrito em **Node.js** que pode ser baixado [AQUI](https://github.com/vimendesilva/curso-docker/tree/master/volume-exemplo). Até podemos executar esse código na nossa máquina, mas temos que instalar o Node na versão certa em que o desenvolvedor implementou o código.  Após o download, abra o projeto e rode o comando: `npm install` para que a pasta node_modules esteja no projeto. Agora como fazemos para criar um container, que irá pegar e rodar esse código Node que está na nossa máquina? Vamos usar os volumes. Primeiro vamos rodar um código em Node.js, precisamos usar a sua imagem através do comando:

        docker run node
    
Além disso, precisamos criar um volume, que referencie à pasta do código no nosso Desktop:

        docker run -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" node
    
Agora, para iniciar o seu servidor, executamos o comando **npm start**. Para executar um comando dentro do container, podemos iniciá-lo no modo interativo ou passá-lo no final do **docker run**:

        docker run -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" node npm start
        

Por fim, esse servidor roda na porta 3000, então precisamos linkar essa porta a uma porta do nosso computador, no caso a 8080. O comando ficará assim:

        docker run -p 8080:3000 -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" node npm start
        
Executado o comando, recebemos um erro. Nele podemos verificar a seguinte linha:

        npm ERR! enoent ENOENT: no such file or directory, open '/package.json'
        

Isto é, o **package.json** não foi encontrado, mas ele está dentro da pasta do código. O que acontece é que o container não inicia já dentro da pasta **/var/www**, e sim em uma pasta determinada pelo próprio container. Por exemplo, se a imagem é baseada no Ubuntu, o container iniciar no root. Então devemos especificar que o comando npm start deve ser executado dentro da pasta **/var/www**. Para isso, vamos passar a flag -w (Working Directory), para dizer em qual diretório o comando deve ser executado, na pasta **/var/www**:

        docker run -p 8080:3000 -v "C:\Users\Alura\Desktop\volume-exemplo:/var/www" -w "/var/www" node npm start
        
Agora, ao acessar a porta 8080 no navegador, vemos uma página exibindo a mensagem Eu amo Docker!. E para testar que está mesmo funcionando, podemos editar o arquivo index.html localmente, salvá-lo e ao recarregar a página no navegador, a nova mensagem é exibida! Com isso concluimos que podemos criar um ambiente de desenvolvimento todo baseado em containers, já que todos utilizam, todos terão o mesmo ambiente de desenvolvimento.

## **4. Construindo nossas próprias imagens**

Conforme vimos anteriormente, a imagem é como se fosse uma receita de bolo. Portanto, para criarmos a nossa própria imagem, precisamos criar nossa receita de bolo, o **Dockerfile**. Então, no projeto que estamos usando, temos que criar o arquivo **Dockerfile**, que nada mais é do que um arquivo texto, ele pode ter qualquer nome, porém deve possuir a extensão .dockerfile, por exemplo node.dockerfile, mas vamos manter o nome padrão mesmo.

Geralmente, montamos nossas imagens a partir de uma imagem já existente. Podemos criar uma imagem do zero, mas a prática de utilizar uma imagem como base e adicionar nela o que quisermos é mais comum. Então vamos começar!

Para dizer a imagem-base que queremos, utilizamos a palavra FROM mais o nome da imagem. Como o nosso projeto precisa do Node.js, vamos usar sua imagem e indicar a versão que queremos, ou usar o **latest**, que é quem faz referência à versão mais recente da imagem. Caso não seja passada nenhuma versão, o Docker assumi que queremos o latest, mais vamos deixar isso explícito:

        From node:latest
        
Outra instrução que vem logo após é quem cuida ou quem criou a imagem, através do MAINTAINER:

       FROM node:latest
       MAINTAINER nome do criador
       
Agora, especificamos o que queremos na imagem. Queremos colocar nosso código dentro dela, então usamos o **COPY**. Vamos usar o **.** para copiar tudo o que está na pasta do arquivo **Dockerfile**, para **/var/www**.

        FROM node:latest
        MAINTAINER nome do criador
        COPY . /var/www

No projeto, já temos as suas dependências dentro da pasta **node_modules**, mas não queremos copiar essa pasta para dentro do container, pois elas podem estar desatualizadas, então queremos que a própria imagem instale as dependências para nós, rodando o comando **npm install**. Para executar um comando, utilizamos o **RUN**:

        FROM node:latest
        MAINTAINER nome do criador
        COPY . /var/www
        RUN npm install
        
Agora, **deletamos a pasta node_modules**, para ela não ser copiada para o container. Além disso, toda imagem possui um comando que é executado quando a mesma inicia, e o comando que utilizamos na aula anterior foi o **npm start**. Para isso, utilizamos o **ENTRYPOINT**, que executará o comando que quisermos assim que o container for carregado:

        FROM node:latest
        MAINTAINER nome do criador
        COPY . /var/www
        RUN npm install
        ENTRYPOINT npm start   ou   ["npm", "start"]
        

Falta colocarmos a porta em que a aplicação executará, a porta em que ela ficará exposta. Para isso, utilizamos o **EXPOSE**:

        FROM node:latest
        MAINTAINER nome do criador
        COPY . /var/www
        RUN npm install
        ENTRYPOINT ["npm", "start"]
        EXPOSE 3000
        
Por fim, falta dizermos onde os comandos rodarão, pois eles devem ser executados dentro da pasta **/var/www**. Então, através do **WORKDIR**, assim que copiarmos o projeto, dizemos em qual diretório iremos trabalhar;
        
        FROM node:latest
        MAINTAINER nome do criador
        COPY . /var/www
        WORKDIR /var/www
        RUN npm install
        ENTRYPOINT npm start
        EXPOSE 3000
        
Com isso, finalizamos o **Dockerfile**, baseado no comando que fizemos na aula anterior:

        docker run -p 8080:3000 -v "$(pwd):/var/www" -w "/var/www" node npm start
        
*Obs: *o comando **pwd** mostra o caminho da pasta que você está, usado como recurso de interpolação de comandos, através de um comando, chama-se outro.*

### **4.1 Criando a imagem**

Para criar a imagem, precisamos fazer o seu build através do comando **docker build**, usado para buildar uma imagem a partir de um **Dockerfile**. Para configurar, passamos o nome do Dockerfile através da flag **-f**:

        docker build -f Dockerfile
        
Além disso, passamos a tag da imagem, o seu nome, através da flag **-t**. Já vimos que para imagens não-oficiais, colocamos o nome no padrão **NOME_DO_USUARIO/NOME_DA_IMAGEM**:

        docker build -f Dockerfile -t nome_usuario/node
        
E agora dizemos onde está o **Dockerfile**. Como já estamos rodando o comando dentro da pasta **volume-exemplo**, vamos utilizar o ponto **(.)**;

        docker build -f Dockerfile -t nome_usuario/node .
        
Ao executar o comando, podemos perceber que cada instrução executada do nosso **Dockerfile** possui um id. Cada instrução gera uma nova camada, que fará parte da imagem final, que nada mais é do que a imagem-base com vários containers intermediários em cima, sendo que cada um desses containers representa um comando do **Dockerfile**. Assim, se um dia a imagem precisar ser alterada, somente o container referente à instrução modificada será alterado, com as outras partes intermediárias da imagem já prontas.

### **4.2 Criando um container a partir da nossa imagem**

Agora que já temos a imagem criada, podemos criar um container a partir dela:

        docker run -d -p 8080:3000 nome_usuario/node
        
Ao acessar o endereço da porta no navegador, vemos a página da nossa aplicação. No **Dockerfile**, também podemos criar variáveis de ambiente, utilizando o **ENV**. Por exemplo, para criar a variável **PORT**, para dizer em que porta a nossa aplicação irá rodar, fazemos:

        FROM node:latest
        MAINTAINER Douglas Quintanilha
        ENV PORT=3000
        COPY . /var/www
        WORKDIR /var/www
        RUN npm install
        ENTRYPOINT npm start
        *EXPOSE 3000*
        
E aí no próprio Dockerfile, podemos utilizar essa variável:
        
        FROM node:latest
        MAINTAINER Douglas Quintanilha
        ENV PORT=3000
        COPY . /var/www
        WORKDIR /var/www
        RUN npm install
        ENTRYPOINT npm start
        *EXPOSE $PORT*
        
E como modificamos o **Dockerfile**, precisamos construir a nossa imagem novamente e podemos perceber que dessa vez o comando é bem mais rápido, já que quase todas as camadas estão cacheadas pelo Docker. Agora que criamos a imagem, vamos disponibilizá-la para outras pessoas.

### **4.3 Subindo a imagem do Docker Hub**

Para disponibilizar a imagem para outras pessoas, precisamos enviá-la para o **Docker Hub** (serviço de registro em nuvem que permite ao desenvolvedor criar repositórios de imagens docker semelhante ao GitHub). O primeiro passo é criar a nossa conta. Com ela criada, executamos o comando **docker login** e digitamos o nosso login e senha que acabamos de criar. Após isso basta executar o comando **docker push**, passando para ele a imagem que queremos subir, por exemplo:

        docker push nome_usuario/node
        
Esse comando pode demorar um pouco, mas terminada a sua execução, podemos ver que várias mensagens **Mounted from library/node**, ou seja, o Docker já sabe que essas camadas podem ser reaproveitadas da imagem do **node**, então não tem o porquê dessas camadas subirem também, então só as camadas diferentes são enviadas para o **Docker Hub**.

Mais uma vantagem em se trabalhar com camadas, o **Layered File System**, pois até na hora de fazer o upload, só é feito das camadas diferentes, as outras são referenciadas da imagem-base que estamos utilizando, no caso a do **node**.

Por fim, ao acessar a nossa conta do **Docker Hub**, podemos ver que a imagem está lá. Para baixá-la, podemos utilizar o comando **docker pull**, ele somente baixa a imagem sem criar nenhum container acima dela:
    
    docker pull nome_usuario/node
    
Então, esse é um jeito simples de compartilharmos uma imagem com outras pessoas, através do **Docker Hub**. A imagem é disponibilizada em um repositório público, mas também podemos disponibilizar em repositórios privados, que no momento da criação do curso, cada usuário pode criar um repositório privado gratuitamente.

### **Referências Bibliográficas:**

*Curso de Docker: Criando containers sem dor de cabeça
https://cursos.alura.com.br/course/docker-e-docker-compose
Acessado em: 07/11/2019 às 15:25hrs.*
