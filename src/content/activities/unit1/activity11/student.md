
#### Actividad 11




```Js
let words = [
  "luz", "sombra", "cielo", "noche", "sol", 
  "mar", "viento", "estrella", "caminos", "sueños", 
  "misterio", "hoja", "agua", "silencio", "universo"
];

let poem = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(0);
  noLoop(); // Solo generar el patrón una vez

  // Generar un poema aleatorio
  generatePoem();

  // Mostrar el poema
  textAlign(CENTER, CENTER);
  for (let i = 0; i < poem.length; i++) {
    let lineHeight = 40;
    let yPos = height / 2 + (i - poem.length / 2) * lineHeight;
    let size = random(20, 40);
    let style = random([ITALIC, BOLD, NORMAL]);

    textSize(size);
    textStyle(style); // Establecer el estilo
    fill(random(255), random(255), random(255)); // Colores aleatorios
    text(poem[i], width / 2, yPos);
  }
}

function generatePoem() {
  let numLines = floor(random(3, 6)); // Número de líneas aleatorias

  for (let i = 0; i < numLines; i++) {
    let lineLength = floor(random(4, 8)); // Número de palabras por línea
    let line = '';

    for (let j = 0; j < lineLength; j++) {
      let word = random(words); // Elegir palabra aleatoria del conjunto
      line += word + ' ';
    }

    poem.push(line.trim()); // Añadir la línea al poema
  }
}

function mousePressed() {
  poem = []; // Limpiar el poema
  generatePoem(); // Generar uno nuevo
  redraw(); // Redibujar el poema
}
```


![image](https://github.com/user-attachments/assets/a2c49b70-96ae-4295-aa42-29778f0ca881)

![image](https://github.com/user-attachments/assets/259f86f4-1413-439b-8086-85bbe2932d26)

