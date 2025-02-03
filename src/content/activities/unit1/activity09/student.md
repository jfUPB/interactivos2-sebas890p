#### Actividad 9





En este programa se maneja:

random() para elegir la posición, el tamaño y el tipo de forma aleatoriamente.

Uso de noise() (Perlin Noise) para generar colores más armónicos en lugar de cambios abruptos.

noLoop() y redraw() permiten generar una composición nueva cada vez que el usuario hace clic en la pantalla.








```js
function setup() {
  createCanvas(windowWidth, windowHeight);
  noLoop(); // Se ejecuta una vez para generar una composición única
}

function draw() {
  background(20);
  let numShapes = 80; // Número de formas a generar

  for (let i = 0; i < numShapes; i++) {
    let x = random(width);
    let y = random(height);
    let size = random(30, 100);
    let shapeType = floor(random(3)); // 0: círculo, 1: cuadrado, 2: triángulo

    let r = noise(x * 0.01, y * 0.01) * 255;
    let g = noise(x * 0.02, y * 0.02) * 255;
    let b = noise(x * 0.03, y * 0.03) * 255;

    fill(r, g, b, 150);
    noStroke();

    if (shapeType === 0) {
      ellipse(x, y, size, size);
    } else if (shapeType === 1) {
      rect(x, y, size, size);
    } else {
      let x1 = x, y1 = y - size / 2;
      let x2 = x - size / 2, y2 = y + size / 2;
      let x3 = x + size / 2, y3 = y + size / 2;
      triangle(x1, y1, x2, y2, x3, y3);
    }
  }
}

function mousePressed() {
  redraw(); // Redibujar cuando se haga clic en la pantalla
}
```


![image](https://github.com/user-attachments/assets/5a23ab15-1e6f-44ac-9d7d-455947943d9b)


![image](https://github.com/user-attachments/assets/ce3dba27-8a90-4c8d-a557-ec260ff8ac03)

