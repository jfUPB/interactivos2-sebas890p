#### Actividad 13



![image](https://github.com/user-attachments/assets/da1a2d5c-47fa-4af7-9b06-a597a1662689)





```Js
let osc; // Oscilador de sonido
let waveType = 'sine'; // Tipo de onda inicial
let freq = 440; // Frecuencia inicial en Hz
let amp = 0.5; // Amplitud inicial

function setup() {
  createCanvas(400, 200);
  textAlign(CENTER, CENTER);
  textSize(16);

  osc = new p5.Oscillator();
  osc.setType(waveType);
  osc.freq(freq);
  osc.amp(amp);
  osc.start();
}

function draw() {
  background(30);
  fill(255);
  text("Tipo de onda: " + waveType, width / 2, height / 3);
  text("Frecuencia: " + freq + " Hz", width / 2, height / 2);
  text("Amplitud: " + amp, width / 2, (height / 3) * 2);
  text("Presiona 1-4 para cambiar la onda", width / 2, height - 40);
  text("↑ ↓ para cambiar la frecuencia", width / 2, height - 20);
}

// Cambia parámetros con el teclado
function keyPressed() {
  if (key === '1') waveType = 'sine';      // Onda senoidal
  if (key === '2') waveType = 'triangle';  // Onda triangular
  if (key === '3') waveType = 'square';    // Onda cuadrada
  if (key === '4') waveType = 'sawtooth';  // Onda diente de sierra

  if (keyCode === UP_ARROW) freq += 20;    // Aumentar frecuencia
  if (keyCode === DOWN_ARROW) freq -= 20;  // Disminuir frecuencia

  osc.setType(waveType);
  osc.freq(freq);
}
```

























