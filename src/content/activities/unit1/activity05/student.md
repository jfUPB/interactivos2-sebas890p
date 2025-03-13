#### Actividad 5 


Proyecto 1:

Utilice el siguiente proyecto de generative design:

http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_0_03


Descripcion: en este proyecto podemos crear poligonos de distntas figuras geometricas cada vez que damos un click, sin embargo estos poligonos siempre salen de forma centrada y del mismo color, por lo tanto estas son las variaciones que le quiero implementar 

El codigo queda asi con los cambios implementados:

``` Java
'use strict';

var strokeColor;
var mode = 1; // Modo 1: Polígonos en el centro Modo 2: Polígonos en la posición del mouse

function setup() {
  createCanvas(720, 720);
  colorMode(HSB, 360, 100, 100, 100);
  noFill();
  strokeWeight(2);
}

function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    push();

    
    if (mode === 1) {
      translate(width / 2, height / 2);
    } else {
      translate(mouseX, mouseY);
    }

   
    var circleResolution = int(map(mouseY, 0, height, 3, 20));
    var radius = mode === 1 ? abs(mouseX - width / 2) : abs(mouseX - pmouseX) * 2;
    var angle = TAU / circleResolution;

    // Color
    strokeColor = color(map(mouseX, 0, width, 0, 360), 100, 100, 50);
    stroke(strokeColor);
    
    
    if (mode === 2) {
      strokeWeight(map(mouseX, 0, width, 1, 5));
    }

   
    beginShape();
    for (var i = 0; i <= circleResolution; i++) {
      var x = cos(angle * i) * radius;
      var y = sin(angle * i) * radius;
      vertex(x, y);
    }
    endShape();

    pop();
  }
}

function keyReleased() {
  if (keyCode == DELETE || keyCode == BACKSPACE) background(0, 0, 100);
  if (key == 's' || key == 'S') saveCanvas('polygon_art', 'png');
  if (key == '1') strokeColor = color(0, 10);
  if (key == '2') strokeColor = color(192, 100, 64, 10);
  if (key == '3') strokeColor = color(52, 100, 71, 10);
  if (key == 'm' || key == 'M') mode = mode === 1 ? 2 : 1; // Cambia entre los dos modos
}
```

Aplicacion potencial:

Este proyecto se puede utilzar para una gran catidad de proyectos distintos, tomando la idea de los poligonos se pueden crear un sin fin de figuras al momento de presionar un click o yendonos mas alla se puede implementar para sensores de movimientos y dibujar al aire libre con la mano para volverlo mas inmersivo. 



Proyecto 2:

Este fue el proyecto que tome:

http://www.generative-gestaltung.de/2/sketches/?01_P/P_1_1_2_01


Se trata sobre un circulo formado a partir de segmentos de triangulos que tiene dos funciones, la primera es que dependiendo de la ubicacion del mouse se cambia de color y la segunda es que presionando del 1 al 5 se cambia el numero de segmentos de triangulos, lo variacion que hice fue cambiar el tamaño de cada semento de triangulos y asi formar otra figura, el codigo es:

```Js

'use strict';

var segmentCount = 360;
var radius = 300;

function setup() {
  createCanvas(800, 800);
  noStroke();
}

function draw() {
  colorMode(HSB, 360, width, height);
  background(360, 0, height);

  var angleStep = 360 / segmentCount;

  beginShape(TRIANGLE_FAN);
  vertex(width / 10, height / 50);

  for (var angle = 0; angle <= 360; angle += angleStep) {
    var vx = width / 2 + cos(radians(angle)) * radius;
    var vy = height / 2 + sin(radians(angle)) * radius;
    vertex(vx, vy);
    fill(angle, mouseX, mouseY);
  }

  endShape();
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  switch (key) {
  case '1':
    segmentCount = 360;
    break;
  case '2':
    segmentCount = 45;
    break;
  case '3':
    segmentCount = 24;
    break;
  case '4':
    segmentCount = 12;
    break;
  case '5':
    segmentCount = 6;
    break;
  }
}
```

Aplicacion potencial:

En el area del entretenimientos se puede utilizar la funcion de modos del proyecto al igual que la forma de recrear algo por segmentos, estas dos funciones permiten la creaciones de muchas experiencias, por ejemplo, con los modos se le puede dar a escoger al usuario el modo que mas prefiera para que asi sienta unico o propio el resultado.



Proyecto 3:

Utilice el siguiente proyecto:

http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_1_2_03


Este proyecto tiene un funcionamiento sencillo, cuenta con un canvas estatico que cambia de posicion y tamaño mediante se mueva el mouse, lo que hice fue cambiar el color del canvas para que se vea un poco mas dinamico:

``` Js

'use strict';

var tileCount = 20;

var moduleColor;
var moduleAlpha = 180;
var maxDistance = 500;

function setup() {
  createCanvas(600, 600);
  noFill();
  strokeWeight(3);
  moduleColor = color(50, 100, 0, moduleAlpha);
}

function draw() {
  clear();

  stroke(moduleColor);

  for (var gridY = 0; gridY < width; gridY += 25) {
    for (var gridX = 0; gridX < height; gridX += 25) {
      var diameter = dist(mouseX, mouseY, gridX, gridY);
      diameter = diameter / maxDistance * 40;
      push();
      translate(gridX, gridY, diameter * 5);
      rect(0, 0, diameter, diameter); // also nice: ellipse(...)
      pop();
    }
  }
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}
```

Este proyecto me intereso porque de algo sencillo como un canvas duplicado y el cursos del mouse se pueden crear proyectos dinamicos donde el usuario sienta que tiene el control de el. 






