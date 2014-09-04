## Configuración básica de MySQL en Sails.js

A continuación les presento la forma en que configuré mi Sails.js para trabajar con MySQL, hasta este momento, la documentación en Sails.js es muy limitada por lo que un proceso "sencillo" de pronto se torna complicado, es por eso la razón de este mini-tutorial.

### Intalación de Node.js + Sails.js
Descarga e instala Nodejs 

http://nodejs.org/ 

En una terminal de tu sistema operativo, escribre lo siguiente:

```javascript
> npm install –g sails
```

El argumento -g te instala sails de forma global. Ahora te posicionas en la carpeta donde deseas ubicar tu proyecto y ejecutas lo siguiente:
```javascript
> sails new mySQLProject
```

Accede al directorio creado:
```javascript
> cd mySQLProject
```

Ejecutas sails:
```javascript
> sails lift
```

### Creación de API Sails

Para este ejemplo imaginemos que tenemos una sencilla tabla **users** en nuestro host MySQL:

```SQL
CREATE TABLE `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(30) DEFAULT NULL,
  `pass` varchar(30) DEFAULT NULL,
  `complete_name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`users_id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;
```
Para este ejemplo crearemos una API sails con el nombre user

```javascript
> sails generate api user
```
Con esta instrucción conseguimos crear un **controlador** y un **modelo**, ambos llamados User. Ahora dirigete al modelo User.js dentro de la carpeta api/models/ y escribe lo siguiente (dentro de module.exports={...):

```javascript
module.exports = {
	connection: 'someMysqlServer',
	tableName: 'users',
  	attributes: {
  		id:'int',
  		name: 'string',
  		pass:'string',
  		complete_name: 'string'
  	},
  	autoPK:false,
  	autoCreatedAt:false,
  	autoUpdatedAt:false
};
```

Ahora en connections.js necesitamos modificar lo siguiente:

```javascript
  someMysqlServer: {
    adapter: 'sails-mysql',
    host: 'host',
    user: 'tuUsuario',
    password: 'tuContraseña',
    database: 'tuBaseDeDatos'
  },
```
Por último en models.js modificamos la siguiente línea

```javascript
connection: 'localDiskDb', //Por defecto Sails guarda la información en disco
```
Por esta:
```javascript
connection: 'someMysqlServer', //Le indicamos a sails que nos conectaremos con MySQL
```

Una vez finalizados estos cambios ejecuta nuevamente:

```javascript
> sails lift
```

Con esto en teoría conseguirás una **API Rest** hacia tu tabla users.
```
http://localhost:1337/user/
```
Para mayor información revisa : https://github.com/fraygit/SailsJs-Mysql-Association-Tutorial/




