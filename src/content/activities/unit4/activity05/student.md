### Aplicación de lo Aprendido

#### Enlace a la Aplicación Original

https://editor.p5js.org/LaWikipedia/full/xaxriTVf_

#### Código Nueva Aplicación

```py
let circlePositionX = 200;
let circlePositionY = 200;
let circleSpeedX = 0;
let circleSpeedY = 0;
let circleRadius = 25;
let circleHue = 0;
let rainbowStroke = true;

// Serial
let port;
let connectBtn;
let microBitConnected = false;

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAITMICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};

let appState = STATES.WAIT_MICROBIT_CONNECTION;
let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevmicroBitAState = false;
let prevmicroBitBState = false;

function setup() {
  createCanvas(400, 400);
  background(255);
  ellipseMode(RADIUS);
  colorMode(HSB);
  strokeWeight(4);

  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);

  describe(
    'Un círculo comienza en el centro del lienzo. El micro:bit controla su movimiento. A cambia su tamaño cíclicamente. B alterna entre línea arcoíris y negra.'
  );
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
}

function updateButtonStates(newAState, newBState) {
  // Botón A: al presionar
  if (newAState === true && prevmicroBitAState === false) {
    circleRadius += 2;
    if (circleRadius > 60) {
      circleRadius = 4;
    }
    print("A presionado → radio ahora es: " + circleRadius);
  }

  // Botón B: al soltar
  if (newBState === false && prevmicroBitBState === true) {
    rainbowStroke = !rainbowStroke;
    print("B soltado → modo arcoíris: " + rainbowStroke);
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

function draw() {
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");

    if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length === 4) {
          let rawX = int(values[0]);
          let rawY = int(values[1]);
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";

          microBitX = rawX + windowWidth / 2;
          microBitY = rawY + windowHeight / 2;

          updateButtonStates(microBitAState, microBitBState);
        } else {
          print("No se están recibiendo 4 datos del micro:bit");
        }
      }
    }
  }

  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        print("Microbit ready to draw");
        appState = STATES.RUNNING;
      }
      break;

    case STATES.RUNNING:
      if (microBitConnected === false) {
        print("Waiting microbit connection");
        cursor();
        appState = STATES.WAIT_MICROBIT_CONNECTION;
        return;
      }

      const centerX = windowWidth / 2;
      const centerY = windowHeight / 2;
      const threshold = 50;

      if (microBitX > centerX + threshold) {
        circleSpeedX = 2;
      } else if (microBitX < centerX - threshold) {
        circleSpeedX = -2;
      } else {
        circleSpeedX = 0;
      }

      if (microBitY > centerY + threshold) {
        circleSpeedY = -2;
      } else if (microBitY < centerY - threshold) {
        circleSpeedY = 2;
      } else {
        circleSpeedY = 0;
      }

      circlePositionX += circleSpeedX;
      circlePositionY += circleSpeedY;
      circlePositionX = constrain(circlePositionX, circleRadius, width - circleRadius);
      circlePositionY = constrain(circlePositionY, circleRadius, height - circleRadius);

      if (rainbowStroke) {
        stroke(circleHue, 100, 100);
        circleHue = (circleHue + 1) % 360;
      } else {
        stroke(0);
      }

      fill(0);
      ellipse(circlePositionX, circlePositionY, circleRadius);
      break;
  }
}

```

#### Enlace a la Nueva Aplicación

https://editor.p5js.org/LaWikipedia/full/7cvS7xraH

#### GIF de la aplicación en Ejecución
