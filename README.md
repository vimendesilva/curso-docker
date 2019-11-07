# curso-docker

**Instalação no Windows**

[Docker for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

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

Podemos fazer com que o terminal da nossa máquina seja integrado ao terminal de dentro do container, para ficar um terminal interativo, usando o comando `docker run -it ubuntu`.

Com isso, estamos trabalhando dentro do container. E dentro dele, podemos trabalhar como se estivéssemos trabalhando dentro do terminal de um Ubuntu, executando comandos como ls, cat, etc.

Agora, se abrirmos outro terminal e executar o comando docker ps, veremos o container que estamos executando. Podemos parar a sua execução, digitando no container o comando exit ou através do atalho CTRL + D.

- Executando novamente um container

Para iniciar um container que está parado, pegamos o id do container a ser iniciado, e passamos ao comando docker start: `docker start 05025384675e`
Para pausar o container, é usado o comando: `docker stop 05025384675e`

Para iniciar novamente o container, deixando o terminal interativo, é utilizado o comando docker start passando duas flags: -a, de *attach*, para integrar os terminais, e -i, de *interactive*, para interagirmos com o terminal, para podermos escrever nele: `docker start -a -i 05025384675e` 

**Trabalhando com imagem site estático**

Utilizando o comando: `docker run -d -P dockersamples/static-site`, uma nova imagem nomeada 'static-site' pertencente ao usuário 'dockersamples' é baixada. A tag -P fará com que o Docker atribua uma porta aleatória do mundo externo, que no caso é a nossa máquina, para poder se comunicar com o que está dentro do container. E a tag -d fará com que o site rode em segundo plano, sem travar o terminal.

Ao executar o comando `docker ps`, é possível visualizar a porta (coluna PORTS) em que o site está rodando, para que possa ser acessada pelo navegador.

**Referência**: https://cursos.alura.com.br/
