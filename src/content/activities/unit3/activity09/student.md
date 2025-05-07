### Vectores de Prueba

* Presionar A, intercalando entre microbit y el teclado del computador, hasta llegar a 60. El resultado esperado es que no pase de 60. Iniciar entonces la cuenta regresiva.
* Presionar B, intercalando entre microbit y el teclado del computador, hasta llegar a 60. El resultado esperado es que no pase de 1. Iniciar entonces la cuenta regresiva.
* Presionar A, ir a count y agitar el microbit. Debería volver al estado count. Hacer la misma prueba con el teclado.
* Ir a count desde config, ingresar contraseñas malas con el microbit, ir al estado loose y volver a config. Se espera  que el microbit siga ese flujo conforme a las acciones realizadas.
* Ir a count desde config, ingresar la contraseña correcta, ir al estado win y volver a config. Se espera  que el microbit siga ese flujo conforme a las acciones realizadas.
* Ir a count desde config, ingresar la contraseña correcta después de varias contraseñas erróneas, ir al estado win y volver a config. El resultado esperado es que el programa permita ingresar varias contraseñas antes de la correcta.
* Ir a count desde config, ingresar la contraseña correcta, ir al estado win y volver a config, y repetir el proceso. El resultado esperado es que en ambos casos el proceso sea idéntico.
* Agitar el microbit en config, enviar luego con teclado en config. El resultado esperado es que no pase nada.

### Resultados

* El VECTOR 1 pasó ✔️
* El VECTOR 2 pasó ✔️
* El VECTOR 3 pasó ✔️
* El VECTOR 4 pasó ✔️
* El VECTOR 5 pasó ✔️
* El VECTOR 6 pasó ✔️
* El VECTOR 7 pasó ✔️
* El VECTOR 8 pasó ✔️

No hay necesidad de pruebas de regresión. Aún así, haciendo más pruebas, vi que el código después de unas iteraciones dejaba de leer correctamente datos del microbit. 
Esto, me parece, por que en la función draw hace de todo: lee del microbit y ejecuta la máquina de estados. Para separar ambas cosas y poder hacer un uso óptimo del 
flujo del programa y no sobrecargar nada, se creó la función handleSerialInput() que maneje la lectura del serial. Además, pasé todo el código dentro del estado INIT
a una función aparte llamada resetGame() para hacerlo todo más modular. 

### Código Nuevo: 

```js
let port;
let connectBtn;

// Estados
const STATE_INIT = 0;
const STATE_CONFIG = 1;
const STATE_COUNT = 2;
const STATE_WIN = 3;
const STATE_LOOSE = 4;

let currentState = STATE_INIT;
let countdown = 20000;
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

  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);

  handleSerialInput();
  updateUI();

  switch (currentState) {
    case STATE_INIT:
      resetGame();
      currentState = STATE_CONFIG;
      break;

    case STATE_CONFIG:
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
      if (millis() - lastTime >= 1000) {
        countdown -= 1000;
        lastTime = millis();
      }

      if (eventA && solution.length < password.length) {
        solution.push("A");
        eventA = false;
      }
      if (eventB && solution.length < password.length) {
        solution.push("B");
        eventB = false;
      }

      if (solution.length === password.length) {
        currentState = arraysEqual(solution, password) ? STATE_WIN : STATE_COUNT;
        if (currentState === STATE_COUNT) {
          solution = [];
        }
      }

      if (countdown <= 0) {
        currentState = STATE_LOOSE;
      }
      break;

    case STATE_WIN:
      if (!musicPlaying) {
        console.log("🎵 Música de victoria 🎵");
        musicPlaying = true;
      }
      break;

    case STATE_LOOSE:
      if (!musicPlaying) {
        console.log("🎵 Música triste 🎵");
        musicPlaying = true;
      }
      break;
  }

  // 🔁 Reset manual desde el botón o el micro:bit
  if (eventS) {
    currentState = STATE_INIT;
    eventS = false;
  }
}

// 🔁 Función para resetear todo
function resetGame() {
  countdown = 20000;
  solution = [];
  musicPlaying = false;
  eventA = false;
  eventB = false;
  eventT = false;
  eventS = false;
}

// 🔌 Leer datos del puerto serial
function handleSerialInput() {
  if (port.availableBytes() > 0) {
    let dataRx = port.read();
    if (dataRx === 'A') eventA = true;
    else if (dataRx === 'B') eventB = true;
    else if (dataRx === 'T') eventT = true;
    else if (dataRx === 'S') eventS = true;
  }
}

// 🖥️ Mostrar interfaz gráfica
function updateUI() {
  fill(0);
  switch (currentState) {
    case STATE_CONFIG:
      text("Configura: " + int(countdown / 1000), width / 2, height / 2);
      break;

    case STATE_COUNT:
      text("Cuenta: " + int(countdown / 1000), width / 2, height / 2 - 40);
      text("Ingresado: " + solution.join(", "), width / 2, height / 2 + 40);
      break;

    case STATE_WIN:
      fill(0, 150, 0);
      text("¡Ganaste!", width / 2, height / 2);
      break;

    case STATE_LOOSE:
      fill(150, 0, 0);
      text("Perdiste", width / 2, height / 2);
      break;
  }

  connectBtn.html(port.opened() ? 'Disconnect' : 'Connect to micro:bit');
}

// ⌨️ Eventos del teclado para pruebas
function keyPressed() {
  if (key === 'a' || key === 'A') eventA = true;
  if (key === 'b' || key === 'B') eventB = true;
  if (key === 't' || key === 'T') eventT = true;
  if (key === 's' || key === 'S') eventS = true;
}

// 🔗 Conexión al micro:bit
function connectBtnClick() {
  if (!port.opened()) {
    port.open('MicroPython', 115200);
  } else {
    port.close();
  }
}

// 🔍 Comparación de claves
function arraysEqual(a1, a2) {
  return a1.length === a2.length && a1.every((val, i) => val === a2[i]);
}
```
