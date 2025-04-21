### Código p5js

```js
let serialBuffer = []; // Buffer para almacenar bytes recibidos

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
function readSerialData() {
  // Acumula los bytes recibidos en el buffer
  let available = port.availableBytes();
  if (available > 0) {
    let newData = port.readBytes(available);
    serialBuffer = serialBuffer.concat(newData);
  }

  // Procesa el buffer mientras tenga al menos 8 bytes (tamaño de un paquete)
  while (serialBuffer.length >= 8) {
    // Busca el header (0xAA)
    if (serialBuffer[0] !== 0xaa) {
      serialBuffer.shift(); // Descarta bytes hasta encontrar el header
      continue;
    }

    // Si hay menos de 8 bytes, espera a que llegue el paquete completo
    if (serialBuffer.length < 8) break;

    // Extrae los 8 bytes del paquete
    let packet = serialBuffer.slice(0, 8);
    serialBuffer.splice(0, 8); // Elimina el paquete procesado del buffer

    // Separa datos y checksum
    let dataBytes = packet.slice(1, 7);
    let receivedChecksum = packet[7];
    // Calcula el checksum sumando los datos y aplicando módulo 256
    let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;

    if (computedChecksum !== receivedChecksum) {
      console.log("Checksum error in packet");
      continue; // Descarta el paquete si el checksum no es válido
    }

    // Si el paquete es válido, extrae los valores
    let buffer = new Uint8Array(dataBytes).buffer;
    let view = new DataView(buffer);
    microBitX = view.getInt16(0) + windowWidth / 2;
    microBitY = view.getInt16(2) + windowHeight / 2;
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;
    updateButtonStates(microBitAState, microBitBState);

  }
}

function draw() {
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } 
  else {
    microBitConnected = true;
    connectBtn.html("Disconnect");  
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
