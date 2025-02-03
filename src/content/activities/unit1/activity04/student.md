#### Actividad 4 


1. ejemplo 

Tome el siguiente ejemplo de la pagina de openprocessing:

https://openprocessing.org/sketch/880007


Descripcion: el proyecto se trata de un canva negro en el cual aprecen particular circulares por donde se mueva el cursor del mouse, entre mas rapido se mueva aumentan el tamaño de las particulas y la cantidad, como 
reflejando fuegos artificiales:

![image](https://github.com/user-attachments/assets/d9c396ff-38c7-4a40-8e55-67d66ddda306)





Modificacion: realice una modificacion en la mezcla de colores para que tuviera un efecto mas suave y se puedan apreciar mejor los distintos colores y el otro cambio fue en el tamaño de las particulas para que tenga mas 
variabilidad y duracion. 


![image](https://github.com/user-attachments/assets/adb83238-7313-4c01-8df6-cadde7b62b55)



``` Js
var MAX_PARTICLES = 1700;
var COLORS = ['#31CFAD', '#ADDF8C', '#FF6500', '#FF0063', '#520042', '#DAF7A6'];

// ARRAYS
var particles = [];
var pool = [];

// VARIABLES
var wander1 = 0.5;
var wander2 = 2.0;
var drag1 = 0.9;
var drag2 = 0.99;
var force1 = 2;
var force2 = 8;
var theta1 = -0.5;
var theta2 = 0.5;
var size1 = 80;  // Modificado (Antes 100)
var size2 = 250; // Modificado (Antes 180)
var sizeScalar = 0.98; // Modificado (Antes 0.95)

// ----------------------------------------
// Particle Functions
// ----------------------------------------

function Particle(x, y, size) {
    this.alive = true;
    this.size = size || 10;
    this.wander = 0.15;
    this.theta = random(TWO_PI);
    this.drag = 0.92;
    this.color = '#fff';
    this.location = createVector(x || 0.0, y || 0.0);
    this.velocity = createVector(0.0, 0.0);
}

Particle.prototype.move = function () {
    this.location.add(this.velocity);
    this.velocity.mult(this.drag);
    this.theta += random(theta1, theta2) * this.wander;
    this.velocity.x += sin(this.theta) * 0.1;
    this.velocity.y += cos(this.theta) * 0.1;
    this.size *= sizeScalar;
    this.alive = this.size > 1.0;
};

Particle.prototype.show = function () {
    fill(this.color);
    noStroke();
    ellipse(this.location.x, this.location.y, this.size, this.size);
};

function spawn(x, y) {
    var particle, theta, force;
    if (particles.length >= MAX_PARTICLES) {
        pool.push(particles.shift());
    }
    particle = new Particle(mouseX, mouseY, map(noise(frameCount * 0.001, mouseX * 0.01, mouseY * 0.01), 0, 1, size1, size2));
    particle.wander = random(wander1, wander2);
    particle.color = random(COLORS);
    particle.drag = random(drag1, drag2);
    theta = random(TWO_PI);
    force = random(force1, force2);
    particle.velocity.x = sin(theta) * force;
    particle.velocity.y = cos(theta) * force;
    particles.push(particle);
}

function update() {
    var i, particle;
    for (i = particles.length - 1; i >= 0; i--) {
        particle = particles[i];
        if (particle.alive) {
            particle.move();
        } else {
            pool.push(particles.splice(i, 1)[0]);
        }
    }
}

function moved() {
    var particle, max, i;
    max = random(1, 4);
    for (i = 0; i < max; i++) {
        spawn(mouseX, mouseY);
    }
}

// ----------------------------------------
// Runtime
// ----------------------------------------

function setup() {
    createCanvas(windowWidth, windowHeight);
    cursor("none");
    background(0);
}

function draw() {
    update();
    drawingContext.globalCompositeOperation = 'normal';
    background(0, 32);
    drawingContext.globalCompositeOperation = 'screen'; // Modificado (Antes 'lighter')

    for (var i = particles.length - 1; i >= 0; i--) {
        particles[i].show();
    }
}

function mouseMoved() {
    moved();
}

function touchMoved() {
    moved();
}
```

Refelxion: Del codigo aprendi a usar funciones basicas para diseño generativo para el manejo de los tamaños de particulas y los colores



2. Ejemplo:

https://openprocessing.org/sketch/2516551

Descripcion: este proyecto se trata de particulas que tienen una direccion constante formando una obra de colores con fondo negro, cada vez que se presiona el click del mouse aparecen nuevas particulas pero con distintos
colores, sin embargo siempre van hacia la misma direccion


![image](https://github.com/user-attachments/assets/f64a8958-0eb8-455e-9e16-c73bd5a71a9f)



Modificaciones: con este proyecto opte por modificar la direccion de las particulas y los colores de tal forma que se ve asi:

![image](https://github.com/user-attachments/assets/f28d904a-6253-48e0-bf44-35d6d9f5ce21)



``` Js
let particles1 = []; // Arreglo de partículas
let num = 10000; // Número de partículas
let directionMultiplier = 1; // Controla la dirección de las partículas

function setup() {
    createCanvas((4 / 3) * windowHeight, windowHeight);
    background(0);

    for (let i = 0; i < num; i++) {
        particles1.push(createVector(random(width), random(height))); // Posición inicial aleatoria
    }
}

function draw() {
    colorMode(HSB);
    blendMode(ADD);
    stroke((frameCount * 0.5) % 255, 255, 255, 50); // Cambio de color gradual y opacidad reducida

    for (let i = 0; i < num; i++) {
        let p = particles1[i];
        point(p.x, p.y);

        // Movimiento modificado con dirección variable según el clic
        p.x += directionMultiplier * (noise(p.x / 100, p.y / 100, frameCount * 0.001) - 0.5) * 40;
        p.y += directionMultiplier * (noise(p.x / 100, p.y / 100, frameCount * 0.001) - 0.5) * 20;
    }
}

// Al hacer clic, cambia la dirección de las partículas
function mousePressed() {
    directionMultiplier *= -1; // Invierte la dirección de movimiento

    for (let i = 0; i < num; i++) {
        let p = particles1[i];
        point(p.x, p.y);

        // Cambia la posición bruscamente para dar un efecto de explosión o remolino
        p.x += directionMultiplier * (noise(p.x / 100, p.y / 100, 30000) - 0.2) * 200;
        p.y += directionMultiplier * (noise(p.x / 100, p.y / 100, 30000) - 0.3) * 3;
    }
}

// Guarda la imagen al presionar una tecla
function keyPressed() {
    save("img_" + month() + '-' + day() + '_' + hour() + '-' + minute() + '-' + second() + ".jpg");
}
```

Reflexion: de este proyecto pude reflexionar de lo importante que es variar la direccion de las particular para que se sienta aun mas interactivo y tenga un resultado mejor.














