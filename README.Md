# Docker

**O que é?**
---
No dia a dia do desenvolvedor buscamos ferramentas que nos auxilie a ser mais produtivo e esse é o contexto em que o docker se encontra. Ele é uma plataforma open source desenvolvido pela Google com a linguagem de programação GO e permite criar containers, o qual agrupa parte de softwares de um sistema de arquivo completo que tem os recursos necessários para a sua execução.

#Dockerfile

O Dockerfile é um arquivo necessário com as configurações do que terá em um container criado por você. Nesse projeto simples, terá apenas uma imagem do node com suas dependências.

### Explicação do arquivo:


```
// Define qual versão da imagem você quer usar.
FROM node:alpine

// Indica qual o diretório dentro da máquina seus arquivos ficaram salvos. 
WORKDIR /usr/app

// Copia todos os arquivos que começam com package e terminam com json.
COPY package*.json ./

// Instala as dependências que estão no package.json.
RUN npm install

// Copia todo o resto dos arquivos.
COPY . .

// Indica a porta que o servidor expõe para a máquina acessar.
EXPOSE 3000

// Indica qual o comando que a aplicação precisa rodar para startar.
CMD ["npm", "start"]
```
**0BS** 
---
Quando usamos o RUN npm install, criamos a pasta node_modules e para evitar que ela seja copiada junto com os outros arquivos, a colocamos dentro de um arquivo ```.dockerignore```.

# Rodar e criar imagens docker em ambiente de desenvolvimento

**OBS:**
---
Lembre-se de entrar com  su - ${USER} em sua máquina.

- O seguinte comando permite que possa ser criado um novo container:

```
docker build -t name_image . 
```
- O -t permite que possa colocar o nome da image do container.

- O (.) indica que o arquivo Dockerfile está localizado na mesma pasta que está sendo rodado esse comando.

**Com as instalações concluídas, você precisa rodar o seguinte comando para usar a imagem:**

```
docker run -p port_docker:port_file -d name_image
```
- O -p indica que você vai selecionar uma porta para roda a imagem, você poder escolher qualquer porta no **port_docker**, exemplo: 

```
docker run -p 3000:port_file -d name_image
```

- O port_file é a porta que você colocou no seu index.js. A minha está com a porta 3000. Ficando o código assim:

```
docker run -p 3000:3000 -d name_image
```

- O -d pede que em seguida você coloque o nome da imagem, a qual você nomeou antes.

# Docker-compose:

**O que é?**
---
O docker-compose auxilia na criação de volumes, ou seja, em grandes cargas de containers.

### Explicação do arquivo:

Primeiramente, é preciso criar um arquivo docker-compose com a extensão .yml, logo em seguida digitar os seguintes comandos:

```
// Indica a versão do docker-compose.
version: "3"

// Define os serviços da aplicação.
services:

// Aqui você pode nomear como quiser.
  app:

  // Indica onde está o nosso arquivo Dockerfile, ou seja, na raiz do projeto.
    build: .

    // Define o comando que será execultado quando a aplicação subir.
    command: npm start

    //Indica o direcionamento de portas.
    ports:
      - 3000:3000

    //Define em qual pasta ficará as alterãções feitas no projeto.
    volumes:
      - .:/usr/app

```
# Roda o docker-compose em ambiente de desenvolvimento:

```
docker-compose up
```
