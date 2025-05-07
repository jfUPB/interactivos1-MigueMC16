### Implementación

Para hacer que al código de p5.js lleguen datos del serial y los pueda leer, ponemos las siguientes variables globales: ```

```js
let port;
let connectBtn;
```

En el set up abrimos el puerto:

```js
  port = createSerial();
  connectBtn = createButton('Connect to micro:bit');
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
```
Y por último, una funcionalidad similar en function draw() al keyPressed que afecte a los eventos con los datos seriales recibidos:

```js
  // 🔽 Aquí leemos datos si hay algo disponible
  if (port.availableBytes() > 0) {
    let dataRx = port.read();
    if (dataRx === 'A') eventA = true;
    else if (dataRx === 'B') eventB = true;
    else if (dataRx === 'T') eventT = true;
    else if (dataRx === 'S') eventS = true;
  }

  // 🔽 Verificamos el estado de la conexión
  if (!port.opened()) {
    connectBtn.html('Connect to micro:bit');
  } else {
    connectBtn.html('Disconnect');
  }

```

Luego, vamos a MicroPython y usamos el siguiente código para pasarlo a microbit y que sepa qué es lo que tiene que escribir en el serial una vez presionemos los botones:

```py
from microbit import *

uart.init(baudrate=115200)

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        sleep(500)
    if accelerometer.was_gesture('shake'):
        uart.write('S')
        sleep(500)
    if pin_logo.is_touched():
        uart.write('T')
        sleep(500)
```
