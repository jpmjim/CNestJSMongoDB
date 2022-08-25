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

## Instalando y conectando MongoDB Driver
  Ya sabes como hacer la configuración de Docker, Ahora, para conectar NestJS y MongoDB es necesario realizar la instalación de algunas dependencias desde NPM que nos ayudarán a lograrlo.

  ### Cómo instalar drivers MongoDB
  Con el comando <code>npm install mongodb --save</code>[instalarás el driver oficial para trabajar con NodeJS y MongoDB](https://www.npmjs.com/package/mongodb). Esta dependencia puedes utilizarla siempre que quieras, ya sea que estés trabajando con NestJS o no.

  **NOTA:** 
  
  Adicional a la instalación del driver, al trabajar con TypeScript es necesario instalar el tipado de la dependencia con el comando <code>npm i @types/mongodb --save-dev</code> para que nos ayude a trabajar con el driver y evitar errores.

  Siempre usa dependencias oficiales cuando se trata de conexiones a bases de datos. Posterior a eso, podrás instalar otras dependencias que te ayudarán a mapear los datos, pero siempre se apoyan en el driver principal para establecer la conexión y realizar las consultas.

  ### Código de ejemplo para instalación de MongoDB driver
  ```bash
  npm i mongodb --save
  npm i @types/mongodb --save-dev
  ```
  ```typescript
  # src/app.module.ts
  import { MongoClient } from 'mongodb';

  const uri = 'mongodb://root:root@localhost:27017/?authSource=admin&readPreference=primary';

  const client = new MongoClient(uri);
  async function run() {
    await client.connect();
    const database = client.db('platzi-store');
    const taskCollection = database.collection('tasks');
    const tasks = await taskCollection.find().toArray();
    console.log(tasks);
  }
  run();
  ```

## Conexión como inyectable
  Veamos una forma de realizar la conexión a una base de datos MongoDB con el driver oficial.

  ### Estableciendo la conexión asíncrona
  Conectarse a una base de datos es un procedimiento asíncrono. Este puede ejecutarse de manera global al inicializar el proyecto NestJS y, posteriormente, gracias a las características de NestJS, inyectar la conexión en cualquier servicio para hacer consultas.

  - [Mongo en NestJS](https://docs.nestjs.com/techniques/mongodb)

  **Paso 1: establecer la conexión de forma global**

  Creamos un módulo al que denominaremos **DatabaseModule**, que contiene la configuración de forma global para establecer la conexión a una base de datos, a la vez que inyecta un servicio denominado **“MONGO”**. Este puede ser utilizado por cualquier otro servicio que requiere la conexión.
  ```typescript
  // src/database/database.module.ts
  import { MongoClient } from 'mongodb';

  @Global()
  @Module({
    providers: [
      {
        provide: 'MONGO',
        useFactory: async () => {
          const uri =
            'mongodb://mongo:secret@localhost:27017/nestjs_mongo?authSource=admin';
          const client = new MongoClient(uri);
          await client.connect();
          const database = client.db('platzi-store');
          return database;
        },
      },
    ],
    exports: ['API_KEY', 'MONGO'],
  })
  export class DatabaseModule {}
  ```

  **Paso 2: inyección del servicio**

  Inyectamos el servicio que lleva el nombre de **“MONGO”** en cualquier otro servicio que requiere su utilización.
  ```typescript
  // src/app.service.ts
  import { Db } from 'mongodb';

  @Injectable()
  export class AppService {
    constructor(@Inject('MONGO') private database: Db,) {}
  }
  ```
  De esta manera, solo usando el driver oficial de MongoDB, puedes crear un servicio reutilizable para establecer la conexión a tu base de datos.

  ### Código de ejemplo de: conexión como inyectable
  ```typescript
  // src/database/database.module.ts
  import { MongoClient } from 'mongodb'; // 👈 Import MongoClient 

  @Global()
  @Module({
    providers: [
      ...
      {
        provide: 'MONGO',
        useFactory: async () => { // 👈 Inject w/ useFactory
          const uri =
            'mongodb://root:root@localhost:27017/?authSource=admin&readPreference=primary';
          const client = new MongoClient(uri);
          await client.connect();
          const database = client.db('platzi-store');
          return database;
        },
      },
    ],
    exports: ['API_KEY', 'MONGO'],  // 👈 add in exports
  })
  ```
  ```typescript
  // src/app.service.ts
  import { Injectable, Inject } from '@nestjs/common';
  import { Db } from 'mongodb'; // 👈 Import DB Type

  @Injectable()
  export class AppService {
    constructor(
      // @Inject('API_KEY') private apiKey: string,
      @Inject('TASKS') private tasks: any[],
      @Inject('MONGO') private database: Db,
      @Inject(config.KEY) private configService: ConfigType<typeof config>,
    ) {}

    getHello(): string {
      const apiKey = this.configService.apiKey;
      const name = this.configService.database.name;
      return `Hello World! ${apiKey} ${name}`;
    }
    getTasks() { }  // 👈 Create new method
  }
  ```

## Ejecutando un query
  La parte más importante de conectarse a una base de datos es la obtención de las mismas para su posterior uso.

  ### Cómo realizar consultas a la base
  Teniendo establecida la conexión a la base de datos, puedes ejecutar consultas de manera muy sencilla en tus servicios.
  ```typescript
  // src/app.service.ts
  import { Db } from 'mongodb';

  @Injectable()
  export class AppService {

    constructor(@Inject('MONGO') private database: Db,) {}
    
    getProducts() {
      const productCollection = this.database.collection('products');
      return productCollection.find().toArray();
    }
  }
  ```
  Puedes utilizar estas consultas en tus controladores para la creación de endpoints.
  ```typescript
  // src/app.controller.ts
  import { AppService } from './app.service';

  @Controller()
  export class AppController {

    constructor(private readonly appService: AppService) {}

    @Get('/products')
    getProducts() {
      return this.appService.getProducts();
    }
  }
  ```
  Así, tienes ya disponible la creación de todo un CRUD con persistencia en base de datos MongoDB para que juegues con tu aplicación.

  ### Código de ejemplo para ejecutar una query
  ```typescript
  // src/app.service.ts
  ...
  @Injectable()
  export class AppService {
    ...

    getTasks() { // 👈 Query
      const tasksCollection = this.database.collection('tasks');
      return tasksCollection.find().toArray();
    }
  }
  ```
  ```typescript
  // src/app.controller.ts
  import { AppService } from './app.service';

  @Controller()
  export class AppController {
    constructor(private readonly appService: AppService) {}
    ...

    @Get('/tasks/') // 👈 New endpoint
    getTasks() {  
      return this.appService.getTasks();
    }
  }
  ```

## Usando variables de ambiente en Mongo
  Trabajar con **variables de entorno** será siempre la forma más correcta y segura de pasarle a nuestra aplicación los datos sensibles de conexión a bases de datos o claves secretas.

  ### Pasaje de variables de entorno en NestJS
  Veamos cómo se realiza la configuración de variables de entorno en NestJS.

  **Paso 1: instalación de NestJS Config**

  Asegúrate de instalar la dependencia <code>npm i --save @nestjs/config</code>. Esta te permitirá crear en la raíz de tu proyecto el archivo <code>.env</code>, que contendrá las variables de entorno que tu aplicación necesita.
  ```bash
  # .env
  MONGO_BBDD=nestjs_mongo
  MONGO_CONF=mongodb
  MONGO_HOST=localhost:27017
  MONGO_PASS=secret
  MONGO_USER=mongo
  ```

  **Paso 2: importación de las variables de entorno**

  Importa el ConfigModule en el módulo principal de tu aplicación para leer correctamente el archivo <code>.env</code>.
  ```typescript
  // app.module.ts
  import { ConfigModule } from '@nestjs/config';

  @Module({
    imports: [
      ConfigModule.forRoot(),
    ]
  })
  export class AppModule {}
  ```

  **Paso 3: utilización de las variables de entorno**

  De esta manera, ya tienes disponible en tu aplicación para utilizar las variables de entorno que hayas definido en el archivo <code>.env</code> a través del objeto global de NodeJS <code>process</code> de la siguiente manera:

  Tu cadena de conexión de MongoDB:
  ```bash
  mongodb://mongo:secret@localhost:27017/nestjs_mongo
  ```
  Podría quedar de la siguiente manera:
  ```bash
  `${process.env.MONGO_CONF}://${process.env.MONGO_USER}:${process.env.MONGO_PASS}@${process.env.MONGO_HOST}/${process.env.MONGO_BBDD}`,
  ```
  Recuerda no versionar en el repositorio de tu proyecto el archivo <code>.env</code> que contiene datos sensibles como contraseñas o accesos privados. Tu aplicación está lista para conectarse a múltiples ambientes de desarrollo a través de variables de ambiente.

  ### Código de ejemplo para variables de ambiente en Mongo
  ```bash
  // .env, .stag.env, .prod.env
  MONGO_INITDB_ROOT_USERNAME=root
  MONGO_INITDB_ROOT_PASSWORD=root
  MONGO_DB=platzi-store
  MONGO_PORT=27017
  MONGO_HOST=localhost
  MONGO_CONNECTION=mongodb
  ```
  ```typescript
  // src/config.ts
  import { registerAs } from '@nestjs/config';

  export default registerAs('config', () => {
    return {
      ...
      mongo: { // 👈 
        dbName: process.env.MONGO_DB,
        user: process.env.MONGO_INITDB_ROOT_USERNAME,
        password: process.env.MONGO_INITDB_ROOT_PASSWORD,
        port: parseInt(process.env.MONGO_PORT, 10),
        host: process.env.MONGO_HOST,
        connection: process.env.MONGO_CONNECTION,
      },
    };
  });
  ```
  ```typescript
  // src/database/database.module.ts
  import { ConfigType } from '@nestjs/config';

  import config from '../config'; // 👈 import config


  @Global()
  @Module({
    providers: [
      ...
      {
        provide: 'MONGO',
        useFactory: async (configService: ConfigType<typeof config>) => {
          const {
            connection,
            user,
            password,
            host,
            port,
            dbName,
          } = configService.mongo; // 👈 get mongo config
          const uri = `${connection}://${user}:${password}@${host}:${port}/?authSource=admin&readPreference=primary`;
          const client = new MongoClient(uri);
          await client.connect();
          const database = client.db(dbName);
          return database;
        },
        inject: [config.KEY], // 👈 Inject config
      },
    ],
    exports: ['API_KEY', 'MONGO'],
  })
  export class DatabaseModule {}
  ```

## ¿Qué es Mongoose? Instalación y configuración
  Utilizar el driver oficial de MongoDB para NestJS es una buena manera de trabajar y relacionar estos dos mundos. Pero existe una forma mucho más profesional y amigable que te ayudará a trabajar más rápido y cometer menos errores.

  ### ¿Qué es Mongoose como ODM?
  [Mongoose](https://mongoosejs.com/) es un **ODM** (Object Data Modeling) que permite realizar un mapeo de cada colección de tu base de datos MongoDB a través de esquemas. Estos te ayudarán a acceder a los datos, realizar consultas complejas y estandarizar la estructura de los mismos.

  En MongoDB, al ser NoSQL, puedes guardar lo que quieras, en el orden que quieras y con la estructura que quieras. Esto es una muy mala práctica que tienes que evitar ya que traerá serios problemas en un futuro no muy lejano en tu proyecto. **Los ODM llegan para solucionar esto**.

  ### Cómo instalar Mongoose
  Te dejamos esta serie de pasos para utilizar Mongoose.
  - [MongoDB (Mongoose) en Nestjs](https://docs.nestjs.com/recipes/mongodb#mongodb-mongoose)
  - [Mongo](https://docs.nestjs.com/techniques/mongodb)

  **Paso 1: instalación de Mongoose**

  Además de la instalación de Mongoose, NestJS posee su propia librería que te ayudará a crear los esquemas, inyectar los servicios y ejecutar las consultas a tu base de datos.
  ```bash
  npm install --save @nestjs/mongoose mongoose
  ```

  **Paso 2: importación y configuración de Mongoose**

  Importa el módulo **MongooseModule** y pásale la cadena de conexión utilizando, o no, variables de entorno.
  ```typescript
  import { MongooseModule } from '@nestjs/mongoose';
  @Module({
    imports: [
      MongooseModule.forRoot( `${process.env.MONGO_CONF}://${process.env.MONGO_USER}:${process.env.MONGO_PASS}@${process.env.MONGO_HOST}/${process.env.MONGO_BBDD}`
      )
    ]
  })
  export class AppModule { }
  ```
  De esta manera, habrás realizado la conexión de tu base de datos a través de Mongoose, en lugar de utilizar el driver oficial.

  ```typescript
  // src/database/database.module.ts
  import { MongooseModule } from '@nestjs/mongoose'; // 👈 Import

  @Global()
  @Module({
    imports: [  // 👈
      MongooseModule.forRootAsync({ // 👈 Implement Module
        useFactory: (configService: ConfigType<typeof config>) => {
          const {
            connection,
            user,
            password,
            host,
            port,
            dbName,
          } = configService.mongo;
          return {
            uri: `${connection}://${host}:${port}`,
            user,
            pass: password,
            dbName,
          };
        },
        inject: [config.KEY],
      }),
    ],
    providers: [
      {
        provide: 'API_KEY',
        inject: [config.KEY],
      },
    ],
    exports: ['API_KEY', 'MONGO', MongooseModule],  // 👈 add in exports
  })
  export class DatabaseModule {}
  ```

## Implementando Mongoose en Módulos
  Además de realizar la conexión a la base de datos, Mongoose permite el mapeo de información para estandarizar su estructura en cada colección de MongoDB.

  - [Mongoose SchemaTypes](https://mongoosejs.com/docs/schematypes.html#)JS
  - [NestJS Mongoose](https://docs.nestjs.com/techniques/mongodb)

  ### Creando entidades con Mongoose
  Crear entidades para darle forma a tus datos es tarea simple gracias a Mongoose.
  
  **Paso 1: creación de la entidad y sus propiedades**

  Suponiendo que necesitas una colección en Mongo para almacenar productos, comienza creando un archivo llamado <code>product.entity.ts</code> en el módulo de productos de tu aplicación.
  ```typescript
  // modules/products/product.entity.ts
  import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
  import { Document } from 'mongoose';

  @Schema()
  export class Product extends Document {
    @Prop({ required: true })
    name: string;

    @Prop()
    description: string;

    @Prop({ type: Number })
    price: number;

    @Prop({ type: Number })
    stock: number;

    @Prop()
    image: string;
  }

  export const ProductSchema = SchemaFactory.createForClass(Product);
  ```
  Observa el decorador <code>@Prop()</code> para mapear cada atributo de la clase Product que extiende de Document e indicarle a Mongoose que se trata de una propiedad del documento. Exportando ProductSchema que, gracias a SchemaFactory que es el responsable de crear y realizar el mapeo de datos, podrás realizar las posteriores consultas desde los servicios.

  **Paso 2: importación de la entidad**

  Ahora solo tienes que importar la entidad en el módulo al que pertenece de la siguiente manera:
  ```typescript
  // modules/products/products.module.ts
  import { MongooseModule } from '@nestjs/mongoose';
  import { Product, ProductSchema } from './entities/product.entity';

  @Module({
    imports: [
      MongooseModule.forFeature([
        {
          name: Product.name,
          schema: ProductSchema,
        },
      ])
    ]
  })
  export class ProductsModule {}
  ```
  Importa **MongooseModule** y tienes que asignarle un nombre a la colección e inyectarle el schema que utilizará.

  De esta forma, estarás creando la colección <code>products</code> en tu base de datos MongoDB y ya tienes mapeada en tu aplicación la estructura de cada documento que contendrá para evitar errores.

  ### Código de ejemplo para implementar Mongoose
  ```typescript
  // src/products/entities/product.entity.ts
  import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
  import { Document } from 'mongoose';

  @Schema()
  export class Product extends Document {
    @Prop({ required: true })
    name: string;

    @Prop()
    description: string;

    @Prop({ type: Number })
    price: number;

    @Prop({ type: Number })
    stock: number;

    @Prop()
    image: string;
  }

  export const ProductSchema = SchemaFactory.createForClass(Product);
  ```
  ```typescript
  // src/products/products.module.ts
  ...
  import { MongooseModule } from '@nestjs/mongoose';
  import { Product, ProductSchema } from './entities/product.entity';

  @Module({
    imports: [
      MongooseModule.forFeature([
        {
          name: Product.name,
          schema: ProductSchema,
        },
      ]),
    ],
    ...
  })
  export class ProductsModule {}
  ```

## Conectando Mongo a los servicios
  Establecida la conexión a la base de datos con Mongoose y creadas las entidades que mapean la información, es momento de realizar las consultas a la base de datos desde los servicios.

  - [Mongo Indexes](https://www.mongodb.com/docs/manual/indexes/)

  ### Ejecutando consultas con Mongoose
  Aquí tienes una serie de pasos que te ayudarán durante este proceso.

  **Paso 1: importación del esquema en los servicios**

  Comienza inyectando el esquema creado en el servicio que será el responsable de realizar las consultas.
  ```typescript
  // modules/products/products.service.ts
  import { InjectModel } from '@nestjs/mongoose';
  import { Model } from 'mongoose';

  @Injectable()
  export class ProductsService {

    constructor(@InjectModel(Product.name) private productModel: Model<Product>) {}

    findAll() {
      return this.productModel.find().exec();
    }

    findOne(id: string) {
      return this.productModel.findById(id).exec();
    }
  }
  ```
  Utilizando **InjectModel**, inyectas el esquema de productos en el servicio de productos.

  **Paso 2: importación del servicio en los controladores**

  Los servicios son los responsables de realizar las consultas a la base de datos, pero los controladores son quienes determinan cuándo hay que realizar esas consultas.
  ```typescript
  // module/products/products.controller.ts
  @Controller('products')
  export class ProductsController {

    @Get()
    async getAll() {
      return await this.productsService.findAll();
    }

    @Get(':productId')
    async getOne(@Param('productId') productId: string) {
      return await this.productsService.findOne(productId);
    }
  }
  ```
  Crea tantos endpoints como necesites para responder a la necesidad de obtención de los datos a través de GET.

  Así, ya tienes completada tu conexión a la base de datos y obtención de datos en tu API a través de Mongoose y sus esquemas.
  ```typescript
  // src/products/services/products.service.ts
  import { InjectModel } from '@nestjs/mongoose';
  import { Model } from 'mongoose';
  ...
  @Injectable()
  export class ProductsService {
    constructor(
      @InjectModel(Product.name) private productModel: Model<Product>, // 👈
    ) {}
    ...
    findAll() { // 👈
      return this.productModel.find().exec();
    }
    async findOne(id: string) {  // 👈
      const product = await this.productModel.findById(id).exec();
      if (!product) {
        throw new NotFoundException(`Product #${id} not found`);
      }
      return product;
    }
    ...
  }
  ```
  ```typescript
  // src/products/controllers/products.controller.ts
  @Controller('products')
  export class ProductsController {
    ...
    @Get(':productId')
    getOne(@Param('productId') productId: string) {   // 👈
      return this.productsService.findOne(productId);
    }
  }
  ```
  ```typescript
  // src/users/services/users.service.ts
  @Injectable()
  export class UsersService {
    ...
    async getOrderByUser(id: number) {   // 👈
      const user = this.findOne(id);
      return {
        date: new Date(),
        user,
        products: await this.productsService.findAll(),   // 👈 implement await
      };
    }
  }
  ```

