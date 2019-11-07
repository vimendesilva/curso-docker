# curso-docker

**Instalação no Windows**

Requisitos
- Arquitetura 64 bits
- Versão *Pro*, *Enterprise* ou *Education*.
- Virtualização habilitada

Seguimos o passo a passo do instalador para aceitar a licença, autorizamos o instalador e seguimos com a instalação. Ao clicar em *Finish*, precisamos encerrar a sessão do Windows e iniciá-la novamente. Ao fazer o login, precisamos habilitar o Hyper-V, clicando em Ok, para que o computador seja reiniciado.

> Hyper-V é uma tecnologia que permite a virtualização do hardware em um computador físico. Em outras palavras, é possível criar e gerenciar computadores virtuais e seus recursos, onde cada máquina virtual (VM) é considerada um sistema isolado.

O Docker pode demorar um pouco para inicializar, mas quando a mensagem Docker *is running* for exibida, significa que ele foi instalado com sucesso e já podemos utilizá-lo.

*Hello World*

No terminal, podemos executar os seguintes comandos:
- `docker version` (verifica sua versão)
- `docker run hello-world` (executa o clássico *Hello World*)

Caso você tenha tido algum problema em instalar o Docker em sua máquina, você também tem a opção de utilizar o [Play With Docker](https://labs.play-with-docker.com/). Nele você poderá utilizar os comandos vistos daqui em diante e testar as diversas funcionalidades que o Docker proporciona. Ao acessar o site basta clicar em +Add New Instance e começar a utilizá-lo como estivesse usando sua máquina normalmente.

**Trabalhando com as imagens**

Anteriormente, rodamos a imagem *hello-world* no container. 
Podemos executar a imagem do Ubuntu utilizando o comando `docker run ubuntu`

O container foi criado, o que acontece é que a imagem do Ubuntu não executa nada, por isso nenhuma mensagem foi exibida.
Podemos verificar isso vendo os containers que estão sendo executados no momento, executando o seguinte comando: `docker ps`

Para ver todos containers, inclusive os parados, adicionamos a flag -a ao comando acima: `docker ps -a`

Então, quando executamos o container do Ubuntu, precisamos passar para ele um comando que rode dentro dele, por exemplo:
`docker run ubuntu echo "Ola Mundo"`

