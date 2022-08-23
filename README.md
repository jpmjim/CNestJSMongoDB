# CNestJSMongoDB
Curso de NestJS: Persistencia de Datos con MongoDB

### Instalación de Platzi Store y presentación
  
  **¿Cuál es el proyecto para NestJS:Persistencia de Datos con MongoDB?**

  Te recomendamos tomar estos cursos antes de iniciar este. Allí, se desarrolló una API Rest para el manejo de un catálogo de productos y se modularizó la aplicación con buenas prácticas de desarrollo. En este curso, se conectará la aplicación con una base de datos MongoDB para persistir los datos.

  Prepárate para dar tus primeros pasos con una base de datos NoSQL junto con NestJS.

## Configuración de Docker para MongoDB
  **Tu productividad como desarrollador/a de software se incrementará** gracias a **Docker**. No importa si eres desarrollador/a, backend o front-end. Hoy en día, trabajar con Docker es vital para ser un buen profesional del software.

  ### Cuáles son los beneficios de Docker
  Con Docker podrás utilizar la tecnología que quieras en simples pasos, sin preocuparte por instalarla en tu computadora. No tendrás que “llenar” tu ordenador con programas que tal vez solo necesitas por un rato.

  Es así como Docker simplifica la instalación de un motor de base de datos, de un lenguaje de programación para hacer algunas pruebas o de un software en particular para un propósito dado.

  ### Cómo trabajan Docker y MongoDB
  Veamos cómo puedes emplear Docker para levantar una base de datos MongoDB.

  **1. Configuración Docker**

  Comienza creando un archivo al cual, por defecto, se lo denomina <code>docker-compose.yml</code>.

  **NOTA**: Los archivos de Docker utilizan la extensión <code>.yml</code>. Tal vez tengas que instalar una extensión en tu editor de código para visualizar estos archivos correctamente.

  - [YAML para Visual Code](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)
  - [Crear contenedores de bases de Datos en Docker](https://dev.to/marlosrodriguez/crear-contenedores-de-bases-de-datos-en-docker-bdg)

  **2. Configuración MongoDB con Docker**

  Agrégale el siguiente contenido al archivo que te permitirá levantar un contenedor de Docker con MongoDB en su interior.
  ```yaml
  # docker-compose.yml
  version: '3'
  services:
    my-mongo:
      image: mongo:4.4.4
      environment:
        - MONGO_INITDB_DATABASE=nestjs_mongo
        - MONGO_INITDB_ROOT_USERNAME=mongo
        - MONGO_INITDB_ROOT_PASSWORD=secret
      ports:
        - '27017:27017'
      volumes:
        - ./mongo_data:/data/db
  ```
  Cómo leer cada línea del archivo

  Entendamos qué es cada línea de este archivo:

  - version: la versión de Docker a utilizar.
  - services: un mismo <code>docker.compose.yml</code> podrá tener N cantidad de contenedores Docker que se relacionan entre sí. En este ejemplo, solo tendremos un contenedor llamado <code>my-mongo</code>.
  - image: el nombre de la imagen base a utilizar para levantar el contenedor.
  - environment: variables de entorno que el contenedor necesita. La imagen de Docker usada recibe por defecto las variables <code>MONGO_INITDB_DATABASE</code>, <code>MONGO_INITDB_ROOT_USERNAME</code> y <code>MONGO_INITDB_ROOT_PASSWORD</code> para preconfigurar el usuario de la base de datos.
  - ports: el puerto que el contenedor utilizará. <puerto_host>:<puerto_contenedor>. MongoDB usa por defecto el puerto 27017 y podremos acceder al mismo a través del puerto 27017 de la máquina local.
  - volumes: Docker, al destruir un contenedor, no mantendrá los datos y se perderán. Usamos esta configuración para generar un directorio y persistir los datos en la computadora local.

  **3. Ejecutar contenedor**

  Es momento de levantar el contenedor con el simple comando <code>docker-compose up -d</code>. En pocos segundos podrás corroborar si el contenedor quedó levantado en tu computador con el comando <code>docker ps</code>. Debería estar ejecutándose, en el puerto 27017 una base de datos MongoDB.

  **4. Otros comandos útiles**
  Puedes detener el contenedor que está corriendo con el comando <code>docker-compose down</code> o actualizarlo con el comando <code>docker-compose up -d --build</code> en el caso de que hayas realizado modificaciones en el archivo **docker-compose.yml**.

  ### Código de ejemplo para configuración de Docker
  ```yaml
  # docker-compose.yml  
  version: '3.3'
  services:
    mongo:
      image: mongo:4.4
      environment:
        MONGO_INITDB_ROOT_USERNAME: root
        MONGO_INITDB_ROOT_PASSWORD: root
      ports:
        - 27017:27017
      volumes:
      - ./mongo_data:/data/db
  ```
  ```bash
  # .gitignore
  /mongo_data
  ```
  ```bash
  docker-compose up -d mongo
  docker-compose ps
  docker-compose down
  ```

## Exploración de la base de datos con Mongo Compass
  Al trabajar con un motor de base de datos, siempre es muy práctico disponer de una **interfaz gráfica para visualizar nuestros datos** y ejecutar consultas más cómodamente.

  ### UI para MongoDB
  [Mongo Compass](https://www.mongodb.com/try/download/compass  ) es el software por excelencia para la visualización de bases de datos MongoDB, oficial y desarrollado por Mongo. Te permitirá conectarte a cualquier base de datos, sea local o remota, para visualizar las colecciones y los documentos en tu base.

  **String de conexión a base de datos**

  MongoDB utiliza una sintaxis especial para establecer la conexión a una base de datos. Utiliza un string con la siguiente estructura:
  ```bash
  mongodb://<USER>:<PASS>@<HOST>:<PORT>/<DBNAME>?authSource=admin
  ```
  Debe completar los datos del usuario, del host y puerto, y el nombre de la base de datos, seguido de algunos parámetros opcionales de configuración. Si la información es correcta, se establecerá la conexión con la base de datos MongoDB que puedes estar corriendo en Docker o en un servidor remoto.

  **Por ejemplo:**

  ```bash
  mongodb://mongo:secret@localhost:27017/nestjs_mongo?authSource=admin
  ```
  Recuerda que, para conectarte a tu base de datos MongoDB que está corriendo en Docker, las variables de entorno que has configurado en el docker.compose.yml son los mismos datos que tienes que utilizar para construir el string de conexión.
  ```yaml
  # docker-compose.yml
  ...
  environment:
    - MONGO_INITDB_DATABASE=nestjs_mongo
    - MONGO_INITDB_ROOT_USERNAME=mongo
    - MONGO_INITDB_ROOT_PASSWORD=secret
  ```
  Mongo Compass será tu mejor aliado a la hora de diseñar y usar bases de datos MongoDB.
