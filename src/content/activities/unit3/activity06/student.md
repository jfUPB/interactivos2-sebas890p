#### Actividad 6


codigo del desktop 

``` js
let conn;
let camara;
let btnCapturar;
let btnTransmitir;
let selectorArchivo;
let imagenActual;

function setup() {
  createCanvas(400, 400);
  background(200);

  conn = io('http://localhost:3000');

  conn.on('connect', () => {
    console.log('Conexión establecida con el servidor');
  });

  camara = createCapture(VIDEO);
  camara.size(400, 300);
  camara.hide();

  btnCapturar = createButton('Capturar Imagen');
  btnCapturar.position(10, height + 10);
  btnCapturar.mousePressed(capturarImagen);

  btnTransmitir = createButton('Enviar Imagen');
  btnTransmitir.position(140, height + 10);
  btnTransmitir.mousePressed(transmitirImagen);
  btnTransmitir.hide();

  selectorArchivo = createFileInput(cargarImagenLocal);
  selectorArchivo.position(280, height + 10);
}

function capturarImagen() {
  imagenActual = camara.get();
  btnTransmitir.show();
}

function cargarImagenLocal(archivo) {
  if (archivo.type === 'image') {
    loadImage(archivo.data, (img) => {
      imagenActual = img;
      transmitirImagen();
    });
  }
}

function transmitirImagen() {
  if (imagenActual) {
    const datos = imagenActual.canvas.toDataURL();
    conn.emit('image', datos);
    console.log('Imagen transmitida al servidor');
    btnTransmitir.hide();
  }
}

function draw() {
  background(200);
  image(camara, 0, 0);

  if (imagenActual) {
    image(imagenActual, 50, 320, 100, 75);
  }
}
```
mobile:

```js
let socketCon;
let imagenRecibida;

function setup() {
  createCanvas(400, 400);
  background(240);

  socketCon = io('http://localhost:3000');

  socketCon.on('connect', () => {
    console.log('Cliente móvil conectado');
  });

  socketCon.on('image', (contenido) => {
    console.log('Nueva imagen recibida');
    
    if (imagenRecibida) {
      imagenRecibida.remove();
    }

    imagenRecibida = createImg(contenido, 'foto');
    imagenRecibida.hide();

    setTimeout(() => {
      redraw();
    }, 100);
  });

  noLoop();
}

function draw() {
  background(240);

  if (imagenRecibida) {
    image(imagenRecibida, 50, 50, 300, 300);
  }
}
```


¿Cómo se comunican los clientes con el servidor?

Los clientes (desktop y mobile) se conectan al servidor mediante WebSockets usando la librería socket.io.
Esto ocurre con la línea:


conn = io('http://localhost:3000'); // en desktop  
socketCon = io('http://localhost:3000'); // en mobile


Esta conexión permite intercambiar mensajes en tiempo real sin necesidad de recargar la página.


¿Cómo se comunican los clientes entre sí?

Los clientes no se comunican directamente entre sí.
En cambio, el cliente desktop envía un mensaje al servidor, y este lo redistribuye (con socket.broadcast.emit(...)) a los demás clientes conectados, como el cliente mobile.

Por ejemplo:


conn.emit('image', datos); // desde el cliente desktop

Y en el servidor:

socket.broadcast.emit('image', datos); // reenviado a otros clientes

¿Qué tipo de mensajes se envían?

Evento image: que transporta una imagen capturada o cargada por el usuario desde el cliente desktop hasta el servidor, y luego al cliente mobile.

¿Qué tipo de datos se envían?

El dato enviado es una cadena de texto codificada en base64 que representa una imagen.

Ejemplo del formato:


data:image/png;base64,iVBORw0KGgoAAAANSUhEUgA..

Esto se obtiene con:

imagenActual.canvas.toDataURL();

¿Qué tipo de eventos se generan?

connect: cuando el cliente se conecta exitosamente al servidor.

image: cuando se envía o recibe una imagen.

Cómo es el flujo de datos entre los clientes y el servidor?

El cliente desktop toma una foto desde la cámara o selecciona una imagen local.

La imagen se codifica como una cadena base64.

Se envía al servidor con conn.emit('image', datos).

El servidor recibe ese evento y reenvía la imagen a los demás 


¿Cómo es el flujo de datos entre los clientes?

La comunicación sigue este flujo indirecto a través del servidor:


Cliente Desktop ──(emit 'image')──▶ Servidor ──(broadcast 'image')──▶ Cliente Mobile

El cliente desktop actúa como emisor, y el cliente mobile como receptor. No hay contacto directo entre ellos.
