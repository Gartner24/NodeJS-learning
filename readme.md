# Node.js

## Node.js Modules

### Node.js process module

The process module is a global object that provides information about, and control over, the current Node.js process. As a global, it is always available to Node.js applications without using require().

- Example: process.argv

result:

```bash
[
  '/usr/local/bin/node',
  '/Users/mjr/work/node/process-args.js',
  'one',
  'two=three',
  'four'
]
```

### Node.js os module

The os module provides operating system-related utility methods and properties. It can be accessed using:

```js
const os = require('os');
```

- Example: os.uptime()

result:

```bash
The system uptime is 6154 seconds.
```

### Node.js fs module

The fs module enables interacting with the file system in a way modeled on standard POSIX functions.

- Example: fs.readFile('file.txt', callback)

result:

```bash
Text file content
```

### Node.js events module

The events module provides a way of working with events.

- Example:

```js
emisorProductos.on('compra', (total) => {
  console.log(`Se realizo una compra por $${total}.`); 
});

emisorProductos.emit('compra', 500);
```

result:

```bash
se realizo una compra por $500.
```

### Node.js url module

The url module provides utilities for URL resolution and parsing.

- Example:

```js
// Crear un objeto URL a partir de una cadena de caracteres.
const miUrl = new URL('https://www.ejemplo.org/cursos/programacion?ordenar=vistas&nivel=1');

// Nombre del host
console.log(miUrl.hostname);

// Path (camino)
console.log(miUrl.pathname);

// Protocolo
console.log(miUrl.protocol);

// Parametros Query

// Retorna un objeto URLSearchParams que representa los parámetros
// query (de búsqueda) de la URL. 
console.log(miUrl.searchParams);
console.log(typeof miUrl.searchParams);

console.log(miUrl.searchParams.get('ordenarPor'));
console.log(miUrl.searchParams.get('nivel'));


// ---------------------------
// Extra: crear una URL
// ---------------------------

const nuevaURL = new URL('https://www.ejemplo.org');

nuevaURL.pathname = '/cursos/programacion';
nuevaURL.search = '?ordenar=vistas&nivel=1';

console.log(nuevaURL.toString());
```

## Promises

A Promise is an object representing the eventual completion or failure of an asynchronous operation.

```js
// ------------------------
// Ejemplo: Pedido de Pizza
// ------------------------

// Tenemos una tienda de pizzas y 
// nuestro sistema de pedidos es asincrono. 
// Puede tomar unos segundos procesar el pedido
// y la operacion tambien puede fallar algunas veces.
const estatusPedido = () => {
  // Math.random() genera un numero aleatorio entre 0 y 1.
  const estatus = Math.random() < 0.8;
  // Para ver el estatus en el terminal. 
  // Incluido solamente para probar la aplicacion.
  console.log(estatus);
  return estatus;
};

const miPedidoDePizza = new Promise((resolve, reject) => {
  // setTimeout() simula el tiempo que tardaria la operacion
  // de procesar la compra y agregarla a una base de datos.
  setTimeout(() => {
    if (estatusPedido()) {
      resolve('¡Pedido exitoso! Su pizza esta en camino.');
    } else {
      reject('Ocurrio un error. Por favor intente nuevamente.');
    }
  }, 3000);
});

const manejarPedido = (mensajeDeConfirmacion) => {
  console.log(mensajeDeConfirmacion);
};

const rechazarPedido = (mensajeDeError) => {
  console.log(mensajeDeError);
};

miPedidoDePizza.then(manejarPedido, rechazarPedido);

// Sintaxis alternativa: encadenar .then()
miPedidoDePizza
  .then((mensajeDeConfirmacion) => {
    console.log(mensajeDeConfirmacion);
  })
  .then(null, (mensajeDeError) => {
    console.log(mensajeDeError);
  });

// Example with catch()

miPedidoDePizza
  .then((mensajeDeConfirmacion) => {
    console.log(mensajeDeConfirmacion);
  })
  .catch((mensajeDeError) => {
    console.log(mensajeDeError);
  });
```

## Async/Await

The async function declaration defines an asynchronous function, which returns an AsyncFunction object. An asynchronous function is a function which operates asynchronously via the event loop, using an implicit Promise to return its result. But the syntax and structure of your code using async functions is much more like using standard synchronous functions.

```js
function ordenarProducto(producto) {
  return new Promise((resolve, reject) => {
    console.log(`Ordenando: ${producto} de freeCodeCamp.`);
    setTimeout(() => {
      if (producto === 'taza') {
        resolve('Ordenando una taza con el logo de freeCodeCamp...')
      } else {
        reject('Este producto no esta disponible actualmente.');
      }
    }, 2000);
  });
}

function procesarPedido(respuesta) {
  return new Promise((resolve, reject) => {
    console.log('Procesando respuesta...');
    console.log(`La respuesta fue: ${respuesta}`);
    setTimeout(() => {
      resolve('Gracias por tu compra. Disfruta tu producto de freeCodeCamp.');
    }, 4000);
  });
}

// Realizar procesos asincronos en orden 
// al esperar que uno se complete antes que el otro inicie.

ordenarProducto('taza')
  .then(respuesta => {
    console.log('Respuesta recibida');
    console.log(respuesta);
    return procesarPedido(respuesta);
  })
  .then(respuestaProcesada => {
    console.log(respuestaProcesada);
  })
  .catch(err => {
    console.log(err);
  });

// Con async await hacemos lo mismo de forma mas concisa.
// Una funcion asincrona retorna una promesa.

async function realizarPedido(producto) {
  try {
    const respuesta = await ordenarProducto(producto);
    console.log('Respuesta recibida');
    console.log(respuesta);
    const respuestaProcesada = await procesarPedido(respuesta);
    console.log(respuestaProcesada);
  } catch (err) {
    console.log(err);
  }
}

realizarPedido('taza');
// realizarPedido('lapiz');
```

## Node.js First server

```js
// Incluir el modulo http
const http = require('http');

// Crear un servidor que envie 'Hola, mundo'
// al cliente que realice una solicitud.
const servidor = http.createServer((req, res) => {

  // El objeto req (request = solicitud)
  console.log('===> Objeto req (solicitud)');
  // console.log(req);
  console.log(req.url);
  console.log(req.method);
  console.log(req.headers);

  // El objeto res (response = respuesta)
  console.log('===> Objeto res (respuesta)');
  console.log(res);

  console.log(res.statusCode); // 200 OK
  res.statusCode = 404;
  console.log(res.statusCode); // 404 Not Found
  
  res.setHeader('content-type', 'application/json');
  console.log(res.getHeaders());

  res.end('Hola, mundo');
});

// El servidor escucha en el puerto 3000 y cuando
// comienza escuchar muestra este mensaje en el terminal.
const PUERTO = 3000;

servidor.listen(PUERTO, () => {
  console.log(`El servidor esta escuchando en el puerto ${PUERTO}...`);
});
```

## Node.js routing

```js

const http = require('http');
const {infoCursos} = require('./cursos');

const server = http.createServer((req, res) => {
  const metodo = req.method;

  switch(metodo) {
    case 'GET':
      return manejarSolicitudGET(req, res);
    case 'POST':
      return manejarSolicitudPOST(req, res);
    default:
      res.statusCode = 501;
      res.end(`El metodo no puede ser manejado por el servidor: ${metodo}`);
  }
});

function manejarSolicitudGET(req, res) {
  const camino = req.url;

  if (camino === '/') {
    return res.end('Bienvenidos a mi primer servidor y API creados con Node.js.');
  } else if (camino === '/cursos') {
    return res.end(JSON.stringify(infoCursos));
  } else if (camino === '/cursos/programacion') {
    return res.end(JSON.stringify(infoCursos.programacion));
  }

  res.statusCode = 404;
  return res.end('El recurso solicitado no existe...');
}

function manejarSolicitudPOST(req, res) {
  const path = req.url;

  if (path === '/cursos/programacion') {

    let cuerpo = '';

    req.on('data', contenido => {
      cuerpo += contenido.toString();
    });

    req.on('end', () => {
      console.log(cuerpo);
      console.log(typeof cuerpo);

      // Convertir a un objeto de JavaScript.
      cuerpo = JSON.parse(cuerpo);

      console.log(typeof cuerpo);
      console.log(cuerpo.titulo);

      res.end('El servidor recibio una solicitud POST para /cursos/programacion');
    });

    // return res.end('El servidor recibio una solicitud POST para /cursos/programacion');
  }
}

const PUERTO = 3000;

server.listen(PUERTO, () => {
  console.log(`El servidor esta escuchando en el puerto ${PUERTO}...`);
});
```

## Node.js Express

```js
/*
* Curso de Node.js y Express.
* Creado para freeCodeCamp en Español.
* Por: Estefania Cassingena Navone. 
*/

const express = require('express');
const app = express();

const {infoCursos} = require('./cursos.js');

// Routing

app.get('/', (req, res) => {
  res.send('Mi primer servidor con Express. Cursos 💻.');        
});

app.get('/api/cursos', (req, res) => {
  res.send(JSON.stringify(infoCursos));
});

// Programacion

app.get('/api/cursos/programacion', (req, res) => {
  res.send(JSON.stringify(programacion));
});
  
app.get('/api/cursos/programacion/:lenguaje', (req, res) => {
  const lenguaje = req.params.lenguaje;
  const resultados = infoCursos.programacion.filter(curso => curso.lenguaje === lenguaje);

  if (resultados.length === 0) {
    return res.status(404).send(`No se encontraron cursos de ${lenguaje}`);
  }

  // Ver los parametros query.
  console.log(req.query.ordenar);

  // Ordenar por número de vistas
  if (req.query.ordenar === 'vistas') {
    res.send(JSON.stringify(resultados.sort((a, b) => a.vistas - b.vistas)));
  } else {
    res.send(JSON.stringify(resultados));
  }
});
  
app.get('/api/cursos/programacion/:lenguaje/:nivel', (req, res) => {
  const lenguaje = req.params.lenguaje;
  const nivel = req.params.nivel;
  
  const resultados = programacion.filter(curso => curso.lenguaje === lenguaje && curso.nivel === nivel);
  
  if (resultados.length === 0) {
    return res.status(404).send(`No se encontraron cursos de ${lenguaje} de nivel ${nivel}`);
  }

  res.send(JSON.stringify(resultados));
});

// Matematicas

app.get('/api/cursos/matematicas', (req, res) => {
  res.send(JSON.stringify(matematicas));
});
  
app.get('/api/cursos/matematicas/:tema', (req, res) => {
  const tema = req.params.tema;
  const resultados = matematicas.filter(curso => curso.tema === tema);
  
  if (resultados.length === 0) {
    return res.status(404).send(`No se encontraron cursos de ${tema}`);
  }
  res.send(JSON.stringify(resultados));
});

const PUERTO = process.env.PORT || 3000;

app.listen(PUERTO, () => {
  console.log(`El servidor esta escuchando en el puerto ${PUERTO}...`);      
});
```

## Node.js Express routing in separate files

app.js
```js
const express = require('express');
const app = express();

const {infoCursos} = require('./datos/cursos.js');

// Routers
const routerProgramacion = require('./routers/programacion.js');
app.use('/api/cursos/programacion', routerProgramacion);

const routerMatematicas = require('./routers/matematicas.js');
app.use('/api/cursos/matematicas', routerMatematicas);

// Routing
app.get('/', (req, res) => {
  res.send('Mi primer servidor con Express. Cursos 💻.');        
});

app.get('/api/cursos', (req, res) => {
  res.send(JSON.stringify(infoCursos));
});

const PUERTO = process.env.PORT || 3000;

app.listen(PUERTO, () => {
  console.log(`El servidor esta escuchando en el puerto ${PUERTO}...`);      
});
```

programacion.js
```js

const express = require('express');

const {programacion} = require('../datos/cursos.js').infoCursos;

const routerProgramacion = express.Router();

routerProgramacion.get('/', (req, res) => {
  res.send(JSON.stringify(programacion));
});
  
routerProgramacion.get('/:lenguaje', (req, res) => {
  const lenguaje = req.params.lenguaje;
  const resultados = programacion.filter(curso => curso.lenguaje === lenguaje);
  
  if (resultados.length === 0) {
    return res.status(404).send(`No se encontraron cursos de ${lenguaje}.`);
  } 
  
  if (req.query.ordenar === 'vistas') {
    return res.send(JSON.stringify(resultados.sort((a, b) => b.vistas - a.vistas)));
  }
  
  res.send(JSON.stringify(resultados));
});
  
routerProgramacion.get('/:lenguaje/:nivel', (req, res) => {
  const lenguaje = req.params.lenguaje;
  const nivel = req.params.nivel;
  
  const resultados = programacion.filter(curso => curso.lenguaje === lenguaje && curso.nivel === nivel);
  
  if (resultados.length === 0) {
    return res.status(404).send(`No se encontraron cursos de ${lenguaje} de nivel ${nivel}`);
  }
  res.send(JSON.stringify(resultados));
});

module.exports = routerProgramacion;
```

matematicas.js
```js

const express = require('express');

const {matematicas} = require('../datos/cursos.js').infoCursos;

const routerMatematicas = express.Router();

routerMatematicas.use(express.json());

routerMatematicas.get('/', (req, res) => {
  res.send(JSON.stringify(matematicas));
});
  
routerMatematicas.get('/:tema', (req, res) => {
  const tema = req.params.tema;
  const resultados = matematicas.filter(curso => curso.tema === tema);
  
  if (resultados.length === 0) {
    return res.status(404).send(`No se encontraron cursos de ${tema}`);
  }
  res.send(JSON.stringify(resultados));
});

module.exports = routerMatematicas;
```
