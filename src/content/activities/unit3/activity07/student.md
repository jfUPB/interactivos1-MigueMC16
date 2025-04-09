### Código en p5.js

```js
// Estados
const STATE_INIT = 0;
const STATE_CONFIG = 1;
const STATE_COUNT = 2;
const STATE_WIN = 3;
const STATE_LOOSE = 4;

let currentState = STATE_INIT;
let countdown = 20000; // milisegundos
let lastTime = 0;
let password = ["A", "B", "A"];
let solution = [];

let eventA = false;
let eventB = false;
let eventT = false;
let eventS = false;

let musicPlaying = false;

function setup() {
  createCanvas(400, 400);
  textSize(32);
  textAlign(CENTER, CENTER);
}

function draw() {
  background(220);
  switch (currentState) {
    case STATE_INIT:
      countdown = 20000;
      solution = [];
      musicPlaying = false;
      currentState = STATE_CONFIG;
      break;

    case STATE_CONFIG:
      fill(0);
      text("Configura: " + int(countdown / 1000), width / 2, height / 2);

      if (eventA) {
        countdown = min(countdown + 1000, 60000);
        eventA = false;
      }
      if (eventB) {
        countdown = max(countdown - 1000, 1000);
        eventB = false;
      }
      if (eventT) {
        currentState = STATE_COUNT;
        lastTime = millis();
        eventT = false;
      }
      break;

    case STATE_COUNT:
      fill(0);
      text("Cuenta: " + int(countdown / 1000), width / 2, height / 2 - 40);
      text("Ingresado: " + solution.join(", "), width / 2, height / 2 + 40);

      // Cuenta regresiva cada segundo
      if (millis() - lastTime >= 1000) {
        countdown -= 1000;
        lastTime = millis();
      }

      // Entrada de clave
      if (eventA && solution.length < password.length) {
        solution.push("A");
        eventA = false;
      }
      if (eventB && solution.length < password.length) {
        solution.push("B");
        eventB = false;
      }

      // Comprobación
      if (solution.length === password.length) {
        if (arraysEqual(solution, password)) {
          currentState = STATE_WIN;
        } else {
          solution = [];
        }
      }

      if (countdown <= 0) {
        currentState = STATE_LOOSE;
      }

      if (eventS) {
        currentState = STATE_INIT;
        eventS = false;
      }
      break;

    case STATE_WIN:
      fill(0, 150, 0);
      text("¡Ganaste!", width / 2, height / 2);

      if (!musicPlaying) {
        // Aquí puedes reproducir un sonido si deseas
        console.log("🎵 Música de victoria 🎵");
        musicPlaying = true;
      }

      if (eventS) {
        currentState = STATE_INIT;
        eventS = false;
      }
      break;

    case STATE_LOOSE:
      fill(150, 0, 0);
      text("Perdiste", width / 2, height / 2);

      if (!musicPlaying) {
        // Aquí puedes reproducir un sonido si deseas
        console.log("🎵 Música triste 🎵");
        musicPlaying = true;
      }

      if (eventS) {
        currentState = STATE_INIT;
        eventS = false;
      }
      break;
  }
}

// Función para detectar teclas presionadas (eventos)
function keyPressed() {
  if (key === 'a' || key === 'A') eventA = true;
  if (key === 'b' || key === 'B') eventB = true;
  if (key === 't' || key === 'T') eventT = true;
  if (key === 's' || key === 'S') eventS = true;
}

// Comparación de arrays (clave vs. ingresado)
function arraysEqual(a1, a2) {
  if (a1.length !== a2.length) return false;
  for (let i = 0; i < a1.length; i++) {
    if (a1[i] !== a2[i]) return false;
  }
  return true;
}

```
