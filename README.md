# docker-trail

# Docker

## Definição

### O que é o Docker

Docker é uma plataforma para desenvolvimento, provisionamento e execução de aplicações usando tecnologia de containers.
Exemplo: Servidor Web, Uma ferramenta de monitorização, um servidor de banco de dados e etc.

### Infraestrutura sem uso de container

* [Infraestrutura de Servidores](https://gist.github.com/renatoapcosta/18fe9208226819c8215f103de2fed773)

### Virtualização baseada em container

A virtualização baseada em container usa o kernel do sistema operacional hospedeiro para executar varias aplicações que podem rodar em qualquer infra estrutura suportada.

Um container é um isolamente de recurso.

![docker](https://user-images.githubusercontent.com/6649193/40151285-5ca7bc06-5955-11e8-80bf-df06fb5a44ac.png)


![docker_compoe](https://user-images.githubusercontent.com/6649193/40151346-c74948ea-5955-11e8-8a18-4142eeff6af7.png)

![container_hipervisor](https://user-images.githubusercontent.com/6649193/40151394-1776428c-5956-11e8-8395-177917e409d0.png)

### Por que usar Docker ?

| Desenvolvedor        | Sysadmin           | Usuário    |
| ------------- |:-------------:| -----:|
| Desenvolve uma vez executa em qualquer lugar      | Configura uma vez executa em qualquer lugar | Tudo que é executado por linha de comando|
| Sem preocupação de dependências ou pacotes     | Elimina inconsistências na entrega das aplicações ou serviços     |  Instala software em um ambiente isolado |
| Diversos ambientes de teste | Ciclo de trabalho mais eficiente e ágil      | Evita conflito de multiplas dependencias |


![porque_usar](https://user-images.githubusercontent.com/6649193/40151670-c4c7c496-5957-11e8-85fd-a8db21458beb.png)

## Conceitos e Fundamentos

Arquitetura Docker

![arquitetura_docker](https://user-images.githubusercontent.com/6649193/40151853-995cee8e-5958-11e8-84aa-34da00004837.png)

* Docker Engine ⇒ "daemon" que permite que os containers sejam construídos, enviados e executados
* Docker Client ⇒ recebe as entradas do usuário (CLI) e as envia para a Engine
* Docker Registry ⇒ software que gerencia o armazenamento das IMAGES
* Images ⇒ Template usado para criar containers
* Containers ⇒ Isolamento da aplicação e de recursos. Baseado nas IMAGES


| IMAGES                                        | CONTAINERS                                      | 
| --------------------------------------------- |:-----------------------------------------------:| 
| Template usado para criar containers;         | Isolamento da aplicaçnao e de recursos;         | 
| Pode ser construído pela comunidade ou outros;| Contém o necessário para executar uma aplicação |   
| Armazenado no Docker HUB ou Registry local    | Baseado nas Images                              |    


![arquitetura_resumo](https://user-images.githubusercontent.com/6649193/40152128-427ab126-595a-11e8-931f-3b374abcc01c.png)


## Imagens e Containers

### Introdução ao uso de Imagens

Listando images baixadas

      docker images 
        
      docker images -a
      
As imagens são especificadas por TAGs
        
        repositorio:tag
       
TAG pode ser compreendida como VERSÃO
Uma mesma imagem pode ter diversas TAGs.
A ultima versão de uma images possui a TAG chamada latest.
É aconselhado usar uma tag definitiva, diferente da latest.

Uma imagem possui um ID, que é um hash.

### Docker Hub - Repositório de imagens

O [Docker Hub](https://hub.docker.com/) é um repositório de imagens.

No Docker Hub, as empresas disponibiliza as versões dockernizada de suas aplicações.

As imagens oficiais possui um nome único. Exemplo: ubuntu, mysql, wordpress.

Outras pessoas da comunidade postas suas imagens com o seu nome_repositorio/nome_imagem.

O Docker Hub pertime que qualquer pessoa ou empresa tenha uma imagem privada, sem custo.

Toda imagem, pelo menos as oficiais, possui todos os detalhes de uso informado no Docker Hub.

### Introdução ao uso de containers

Imagens não são containers, mas dão base consistente para que haja um container.

Não há container sem uma imagem.

Para baixar uma imagem hello-world fornecida pelo Docker Hub

      docker pull hello-world
      
Neste caso está imagem vai ser baixada a versão latest, pois não foi informada nenhuma versão.

Para criar um container dessa imagem

      docker run hello-word
 
Para executar um container em segundo plano ( use -d )

      docker run -d ubuntu:14.04 ping 127.0.0.1 -c 50
 
Para executar um container em primeiro plano e iniciar uma iteração com ele. ( use exit para sair )

      docker run -it ubuntu:14.04
 
Listar containers

        docker ps

        docker ps -a

## Administração Básica de Containers e Imagens

#### Iniciando e parando um containers

``` 
docker start container_ID

docker stop container_ID

docker kill container_ID
```

### Dando nome para um container
 
      docker run -it --name nomecontainer debian /bin/bash
      
#### Entrando em um containers em execução

Podemos iniciar outro processo do container para executar uma outra tarefa no container já iniciado.

        docker exec -it container_ID /bin/bash

        ps aux | wc-l
 
### Capturando os logs de um container

 A opção -f acompanhas as ultimas mensagens de forma iterativa.
 
        docker logs container_ID

        docker logs -f container_ID 
 
### Exibindo informações detalhadas

Retorna um JSON das informações do container

        docker inspect container_ID
 
### Exibindo estatisticas de processamento do container

Quanto de memória, processos etc.

        docker stats container_ID

### Copiar um arquivo do host para o container

Copiar do host para o a raiz do container

       docker cp arquivo_host.txt container_ID:/

Copiar do container para o host

      docker cp container_ID:/arquivo_container.txt .

#### Remover um containers

Um container só pode ser removido se o mesmo estiver parado.

        docker rm container_ID

        docker rm $(docker ps -qa)

        docker rm `docker ps -qa`
 
 
#### Remover images

        docker rmi hashimage|nomedaimage

#### Criando um container

        docker run

        -- name Nome do container
        -p 8888:8080  Onde ( porta externa : porta interna no container )        
        --link  Indica outro container


## Containers como Microserviço

* Containers: empacotados, leves e projetados para serem executados em qualquer lugar;

* Multiplos containers: podem ser implantados em uma única VM;

Um microserviço é uma aplicação com uma única função.

* Ambiente de execução mais enxutos;
* Melhor isolamento de recursos;
* Inicialização e execução mais rápidas;

## Container Networking

Docker0 ( bridge )

* "Ponte" entre o host e os containers;
* Criada no momento da instalação da Engine Docker;
* Aleatoriamente é criada uma subnet PRIVADA;

Quando criamos um container, é criado uma VIRTUAL ETHERNET ( VETH )

* Camada que permite determinado container ter sua própria "eth";
* Cada container recebe automaticamente um endereço IP do segmento da bridge0;
* Interface virtual é criada no host;

Nenhum container é acessado diretamente pelo seu endereço de IP entregue pela bridge0.

![network](https://user-images.githubusercontent.com/6649193/40155040-ebc39880-5967-11e8-8f2c-aa61fe8d5d4e.png)

      docker exec -it container_ID ip a
      
## Acesso externo ao container através do mapeamento de portas

O serviço do container precisa ser acessado externamente. 
Usamos o mapeamento de portas para o acesso.

![portas](https://user-images.githubusercontent.com/6649193/40155183-a7a10c2c-5968-11e8-913b-df79f19c654b.png)

O mapeamento de porta pode ser feito de forma manual ou automática.

1 porta do host : 1 porta do container

Parametros opcionais no docker run:
      -p  (manual)
      -P (automatico)

Exemplo:

      docker run -d -p 8080:80 httpd
      
      docker run -d -P httpd
      

## Criação de imagens personalizadas

Por padrão o docker não efetiva os comandos executados em um container.

Vamos personalizar uma imagem.

![criacao_image](https://user-images.githubusercontent.com/6649193/40155455-34c14abc-596a-11e8-8172-327cf33908b0.png)


Vamos acessar o container e modifica - lo.

      
      docker run -d -p 8080:80 httpd
      
      docker exec -it container_ID bash
      
      cd /usr/local/apache2/htdocs
      
      echo "Hello Container Modificado" >> index.html
      
      exit
      
 Vamos remover o container e verificar que as modificações serão perdidas.
 
      docker stop container_ID
      
      docker rm container_ID
      
      docker run -d -p 8080:80 httpd
      
 
### Criando um container personalizado e persistindo em uma nova imagem

        docker run -it --name container_base ubuntu:14.04 bash

        # apt-get update && apt-get install -y apache2
        # echo "Hello Container Modificado" >> index.html
        # exit

        docker commit -m "Message" nomecontainer nomeimagem_novocontainer

        docker run -it nomeimagem_novocontainer

        Obs: nomeimagem_novocontainer pode ser ubuntu/apache especializando o nome atual
      

#### Criar um container test e remover quando finalizar

        docker run --rm -it ubuntu bash

        # apt-get update && apt-get install -y apache2

        # exit

## Dockerfile

Um Dockerfile é um arquivo de configuração que contém instrução para a criação de uma imagem Docker.

Usa da instrução para automatizar o processo de criação de imagens.

![dockerfile](https://user-images.githubusercontent.com/6649193/40156098-ba8e982c-596d-11e8-83f9-9191917e2177.png)

Esse metodo é melhor que entrar em um container, modificar e comitar, pois o processo se torna mais robusto.

Dockerfile Instruções

       FROM - indica a imagem base
       MAINTAINER - informa o autor da imagem
       RUN - Indica o que será executado para a criaçnao da imagem
       EXPOSE - informa qual porta o container irá escutar
       CMD - define um comando padrão que será executado quando o container for criado
       ENTRYPOINT - Precede a instrução CMD
       ADD - Copia um arquivo do host, mesmo sendo uma URL, para dentro da imagem
       COPY - Copia um arquivo do host para dentro da imagem
       
Para criar a imagem usamos docker build

      docker build -t renatoapcosta/nome_imagem_nova .
      
 O ponto final indica onde se encontra o arquivo Dockerfile
 
 Exemplo Dockerfile:
 
      FROM debian
      MAINTAINER Renato Costa<email@gmail.com>
      RUN apt-get update
      RUN apt-get install -y nginx
      ADD index.html /var/www/html/
      EXPOSE 80
      ENTRYPOINT ["/usr/sbin/nginx"]
      CMD ["-g", "daemon off;"]
      
Na pasta que está o Dockerfile, colocar um arquivo index.html.

      docker build -t renatoapcosta/nginx:1.0 .

## Volumes Docker

Um container é um elemento volatil.

Um volume é uma pasta do container que é designado (mapeado) para persistir seus dados, independente do ciclo de vida do container.

É fazer um canal de comunição entre o host e o container.

Volumes:

* Permite manter os dados, mesmo quando o container é deletado;
* É mapeado num diretório do host, em outro container ou em um volume;
* Volumes são mapeados quando um container é criado;

### Volume entre o Host e o Container

Uma pasta do host é mapeado como volume.
O volume é salvo no proprio host.
Mesmo quando se apagar o container, os dados fica no host.
Ponto negativo dessa configuração é a escalabilidade.

      docker run -d -name web_pasta -v /home/user/site:/usr/local/apache2/htdocs -p 8080:80 httpd

### Container como Volume

Nesse configuração criamos um container para receber os dados.

![volume-container](https://user-images.githubusercontent.com/6649193/40157206-37d48354-5974-11e8-9b4a-ed735bb453f4.png)

Criamos o container de volume com base em uma imagem de sistema operacional.

      docker create -v /usr/local/apache2/htdocs --name datacontainer ubuntu:14.04
      
      docker run -d --name web_volumes --volumes-from datacontainer -p 8080:80 httpd
      

### Shared Storage Volume

É um tipo mais recente.

![volume-shared](https://user-images.githubusercontent.com/6649193/40157320-be719320-5974-11e8-994f-fa18bd7b2359.png)

Esse é o mais adequado e mais seguro de mapeamento de volumes.

      docker volume create --name datastore
      
      docker volume ls
      
     
      
      docker run -it --name cont_volume -v datastore:/tmp ubuntu:14.04

Para apagar o container

      docker volume rm datastore 

## Comunicação entre containers

Estabelecer conexões entre containers para oferecer serviços mais completos.

Comunicação entre container é usado para, de maneira segura, transferir dados de um para outro.

### Linking

Esse é um metódo obsoleto, mas o mais facíl.

Basta acrescentar um parametro no docker run, --link nomecontainer a ser linkado.

* Container de origem tem acesso aos dados do container de destino;
* Linking permite conexão entre containers através dos seus respectivos nomes;

Exemplo, configurar um mysql com

    docker pull mysql:5.7

    docker run -it --name mysql_db -e MYSQL_ROOT_PASSWORD=root -e MYSQL_USER=root -e MYSQL_PASSWORD=root -p 3306:3306 -d mysql:5.7
    
    docker run -d -p 8080:80 --name php --link mysql_db:db -v /home/user/site:/var/www/html php:5.6-apache
    
    docker ps -l
    
    docker exec php docker-php-ext-install mysqli

    docker exec -it mysql_db bash

    docker exec -it mysql_db mysql -uroot -p

    mysql -h 127.0.0.1 -u root -p

### Docker Network

Usando o docker network

![docker_network](https://user-images.githubusercontent.com/6649193/40158472-193a0386-597b-11e8-8903-a44d8bcd9a94.png)
    
* Sucessor do método "linking";
* Usuários pode criar diversas redes próprias;
* Permite a comunicação entre containers através dos seus respectivos nomes;

      docker network create redeA
      
      docker network ls
      
      docker run -itd --name ubuntu1 --network=redeA ubuntu:14.04

      docker run -itd --name ubuntu2 --network=redeA ubuntu:14.04
      
      docker exec -it ubuntu1 bash
      
      # ping ubuntu2
      
      docker network disconnect redeA ubuntu1
      
      docker network disconnect redeA ubuntu2
      
      docker network rm redeA
      
      docker network ls
      
## Docker sem sudo

Adicionando o usuario corrente no grupo para usar o docker sem sudo.

`sudo gpasswd -a $USER docker`

## Imagem para o Docker Hub

Crie uma conta no [Docker Hub](https://hub.docker.com/)

Para fazer o login

      docker login
      
 Para renomear uma imagem
 
      docker tag nomeold/imagemold nomelogin/imagem:versao
      
      docker push nomelogin/imagem:versao


## Docker Compose

 * [Docker Compose](https://gist.github.com/renatoapcosta/86d90664f723296c744500bb4acbbafe)
