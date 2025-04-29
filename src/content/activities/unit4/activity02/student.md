#### Actividad 2

Qué es p5LiveMedia?

Una libreria que utiliza sockets para conectar a dos personas en tiempo real y de forma directa sin pasar la informacion primero a un servidor si no que lo hace directo a cada cliente






¿Qué posibilidades ofrece p5LiveMedia?

Para el proyecto del curso ofrece muchas posibilidades para hacer pruebas con audio y video en tiempo real, como al momento de querer hacer algun diseño generativo usando la voz de algun cliente o puntos de reconocimientos 
faciales en un buen tiempo de fluides 



Replica varios ejemplos de p5LiveMedia que permitan entender sus posibilidades. Trata de seleccionar ejemplos para entender cómo se comparte audio, video, datos y el canvas.

```Js
// Ejemplo 1: Compartir video y audio
let myVideo, otherVideo, p5l;

function setup() {
  createCanvas(400, 400);
  let constraints = { audio: true, video: true };
  myVideo = createCapture(constraints, function (stream) {
    p5l = new p5LiveMedia(this, "CAPTURE", stream, "VideoRoom");
    p5l.on("stream", gotStream);
  });
  myVideo.elt.muted = true;
  myVideo.hide();
}

function draw() {
  background(220);
  image(myVideo, 0, 0, width / 2, height);
  if (otherVideo) {
    image(otherVideo, width / 2, 0, width / 2, height);
  }
}

function gotStream(stream, id) {
  otherVideo = createVideo(stream);
  otherVideo.hide();
}

// Ejemplo 2: Compartir datos (mouseX, mouseY)
let sharedData;

function setup() {
  createCanvas(400, 400);
  p5l = new p5LiveMedia(this, "DATA", null, "DataRoom");
  p5l.on("data", gotData);
}

function draw() {
  background(220);
  fill(0);
  ellipse(mouseX, mouseY, 20, 20);
  p5l.send({ x: mouseX, y: mouseY });
  if (sharedData) {
    fill(255, 0, 0);
    ellipse(sharedData.x, sharedData.y, 20, 20);
  }
}

function gotData(data, id) {
  sharedData = data;
}

// Ejemplo 3: Compartir el canvas
let sharedCanvas;

function setup() {
  createCanvas(400, 400);
  background(255);
  p5l = new p5LiveMedia(this, "CANVAS", null, "CanvasRoom");
  p5l.on("data", gotCanvasData);
}

function draw() {
  if (mouseIsPressed) {
    stroke(0);
    line(mouseX, mouseY, pmouseX, pmouseY);
    p5l.send({ x: mouseX, y: mouseY, px: pmouseX, py: pmouseY });
  }
}

function gotCanvasData(data, id) {
  stroke(255, 0, 0);
  line(data.x, data.y, data.px, data.py);
}
```





Documenta en la bitácora tus hallazgos y las consideraciones que debes tener en cuenta para implementar p5LiveMedia en tu aplicación.

La primera consideracion y mas importante es tener la libreria para poder importarla al proyecto, luego el manejo de las conexiones y tener claro cual es el id, tambien es importante el manejo de datos lo que se quiera enviar ya sea video audio o texto, estos son como los aspectos que considero mas importante al implementar p5livemedia.












