#### Actividad 3

Describe la aplicación que propones.

Observando y analizando los ejemplos de la activdad pasada me llamo la atencion uno llamado multiple user with data
porque me inspiro para una aplicacion que funcione como el juego de adivina quien soy, teniendo la posibilidad
de compartir datos y video en tiempo real, se puede hacer un juego donde un usuario recree algo por la camara
y el otro mediante texto vaya escribiendo lo que piensa que esta recreando 



Describe cómo la aplicación propuesta hace uso de la biblioteca p5LiveMedia.

Inicialmente haria uso de ella con la conexion entre usuarios para trasmitir video y datos 
en tiempo real, tambien se podria utilizar para el manejo de conexiones, verificar si un
usuario se conecto o se desconecto



Describe cómo la aplicación propuesta se relaciona con el proyecto de curso.

No tiene una relacion directa con el tema del diseño generativo pero si se relaciona con la
interaccion entre usuarios siendo en tiempo real 



Escribe un tutorial que permita replicar la aplicación propuesta.

creando un nuevo proyecto en p5.js y incliyendo los archivos p5LiveMedia en el HTML. Luego, en el archivo sketch.js, definí las variables necesarias para gestionar las conexiones, los videos, el chat y los nombres de los usuarios. En la función setup(), capturé el video de mi cámara y lo configuré para que se ocultara, mientras creaba los campos de entrada para el nombre y los mensajes. Después, establecí la conexión con otros usuarios usando p5LiveMedia, manejando eventos para recibir nuevos streams, desconexiones y mensajes entrantes. En la función draw(), mostré los videos de todos los participantes con sus nombres y agregué una sección para mostrar el chat en la parte inferior. También creé funciones que permitieran enviar mi nombre y mis mensajes a los demás participantes, y manejé la recepción de esos datos para actualizar el chat y mostrar los nombres correctamente. Finalmente, abrí varias pestañas con la aplicación para comprobar que todo funcionara bien





Adiciona el código y enlaces necesarios para replicar la aplicación propuesta.

enlace de la aplicacion para replicar:

https://editor.p5js.org/dano/sketches/HRvsj7kDk


Codigo de la aplicacion nueva que permite encviar mensajes de texto:

```Js
let allConnections = [];
let vidWidth = 160;
let vidHeight = 120;
let p5live;
let nameField, messageField;
let chatBox = "";

function setup() {
  createCanvas(480, 360);

  myVideo = createCapture(VIDEO, gotMineConnectOthers);
  myVideo.size(vidWidth, vidHeight);
  myVideo.hide();
  allConnections['Me'] = {
    'video': myVideo,
    'name': "Me",
    'x': random(width),
    'y': random(height)
  }

  nameField = createInput("No Name");
  nameField.changed(enteredName);
  
  messageField = createInput("");
  messageField.position(nameField.x, nameField.y + 30);
  messageField.changed(sendMessage);
}

function gotMineConnectOthers(myStream) {
  p5live = new p5LiveMedia(this, "CAPTURE", myStream, "arbitraryDataRoomName");
  p5live.on('stream', gotOtherStream);
  p5live.on('disconnect', lostOtherStream);
  p5live.on('data', gotData);
}

function draw() {
  background(220);
  stroke(255);
  for (var id in allConnections) {
    let thisConnectJSON = allConnections[id];
    let x = thisConnectJSON.x;
    let y = thisConnectJSON.y;
    image(thisConnectJSON.video, x, y, vidWidth, vidHeight);
    stroke(0);
    text(thisConnectJSON.name, x, y + 20);
  }

  // Draw chat box
  fill(0);
  text(chatBox, 10, height - 40);
}

// We got a new stream!
function gotOtherStream(stream, id) {
  otherVideo = stream;
  otherVideo.size(vidWidth, vidHeight);
  allConnections[id] = {
    'video': otherVideo,
    'name': id,
    'x': 0,
    'y': 0
  }
  otherVideo.hide();
  mouseDragged() //send them your location
  enteredName() //send them your name
}

function lostOtherStream(id) {
  print("lost connection " + id)
  delete allConnections[id];
}

function mouseDragged() {
  allConnections['Me'].x = mouseX;
  allConnections['Me'].y = mouseY;
  let dataToSend = {
    dataType: 'location',
    x: mouseX,
    y: mouseY
  };
  p5live.send(JSON.stringify(dataToSend));
}

function enteredName() {
  allConnections['Me'].name = nameField.value();
  let dataToSend = {
    dataType: 'name',
    name: nameField.value()
  };
  p5live.send(JSON.stringify(dataToSend));
}

function sendMessage() {
  let message = messageField.value();
  let dataToSend = {
    dataType: 'message',
    name: nameField.value(),
    message: message
  };
  p5live.send(JSON.stringify(dataToSend));
  chatBox += "Me: " + message + "\n";
  messageField.value("");
}

function gotData(data, id) {
  let d = JSON.parse(data);
  if (d.dataType == 'name') {
    allConnections[id].name = d.name;
  } else if (d.dataType == 'location') {
    allConnections[id].x = d.x;
    allConnections[id].y = d.y;
  } else if (d.dataType == 'message') {
    chatBox += d.name + ": " + d.message + "\n";
  }
}
```

![Captura de pantalla 2025-03-16 200600](https://github.com/user-attachments/assets/d8b6483d-b46a-40b1-86d0-ff1620324ee6)








