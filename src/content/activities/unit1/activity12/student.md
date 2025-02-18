#### Actividad 12


```Js
let img;

function preload() {
  img = loadImage("https://upload.wikimedia.org/wikipedia/commons/thumb/7/77/Google_Images_2015_logo.svg/800px-Google_Images_2015_logo.svg.png");
}

function setup() {
  createCanvas(img.width, img.height);
  noLoop(); // Solo aplicamos el efecto una vez
}

function draw() {
  background(0);
  img.loadPixels();

  for (let y = 0; y < img.height; y += 5) { // Espaciado para efectos más marcados
    for (let x = 0; x < img.width; x += 5) {
      let index = (x + y * img.width) * 4;
      
      // Obtener colores originales
      let r = img.pixels[index];
      let g = img.pixels[index + 1];
      let b = img.pixels[index + 2];
      let a = img.pixels[index + 3];

      // Efecto generativo: Cambiar colores y posiciones
      let newX = x + sin(y * 0.05) * 10; // Ondulación horizontal
      let newY = y + cos(x * 0.05) * 10; // Ondulación vertical

      // Manipular colores de manera aleatoria
      let newR = r * random(0.5, 1.5);
      let newG = g * random(0.5, 6.5);
      let newB = b * random(0.5, 1.5);
      let newA = a * random(0.5, 1.0);

      // Dibujar píxeles modificados como círculos
      fill(newR, newG, newB, newA);
      noStroke();
      ellipse(newX, newY, 50, 5);
    }
  }
}
```


![image](https://github.com/user-attachments/assets/0d747cf6-312e-4ac3-9466-2b447658ab38)



![image](https://github.com/user-attachments/assets/b9a5d7be-2c5c-45ea-9b9d-ddf0dd1eb867)














