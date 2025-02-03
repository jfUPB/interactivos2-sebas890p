#### Actividad 10


ejemplo: http://www.generative-gestaltung.de/2/sketches/?01_P/P_2_1_2_01


Desconstrucción:

En lugar de usar líneas estáticas o con posiciones fijas, utilizamos un parámetro dinámico, el cual es el noise(i * 0.1), que varía la longitud de cada línea de forma suave y continua, emulando un patrón fluido y orgánico.
La relación entre el ángulo y la distancia de las líneas genera un patrón radial.

Reconstrucción:

A partir de esos elementos, se dibujan las líneas con posiciones iniciales y finales que dependen del angle y distance, pero las líneas no siguen un patrón regular. Al recomponer las líneas a partir de parámetros 
modificados (como el noise() aplicado), se genera un diseño visualmente más complejo.

```
let numLines = 150;  // Número de líneas generadas
let angleStep = 0.05; // Paso de ángulo
let lengthFactor = 6; // Factor que afecta la longitud de las líneas

function setup() {
  createCanvas(windowWidth, windowHeight);
  noLoop(); // Solo generar el patrón una vez
  stroke(255, 100); // Color de las líneas en escala de grises con opacidad
  strokeWeight(1);
  background(0);
}

function draw() {
  let centerX = width / 2;
  let centerY = height / 2;

  for (let i = 0; i < numLines; i++) {
    // Desconstrucción: Ángulo y distancia aleatorios dentro de un patrón
    let angle = i * angleStep;
    let distance = lengthFactor * noise(i * 0.1) * width / 2;

    // Reconstrucción: Generar líneas basadas en los parámetros aleatorios
    let x1 = centerX + cos(angle) * distance;
    let y1 = centerY + sin(angle) * distance;
    let x2 = centerX - cos(angle) * distance;
    let y2 = centerY - sin(angle) * distance;

    line(x1, y1, x2, y2); // Dibujar la línea
  }
}

function mousePressed() {
  redraw(); // Vuelve a dibujar el patrón al hacer clic
}
```



