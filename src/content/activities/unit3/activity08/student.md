### Implementación

Para hacer que al código de p5.js lleguen datos del serial y los pueda leer, ponemos las siguientes variables globales: ```

```js
let serial;
let portName = ""; // Se define desde el Serial Control App
let input = "";
```

En el set up abrimos el puerto:

```js
  serial = new p5.SerialPort();
  serial.list(); // Lista de puertos disponibles

  serial.open(); // El puerto se abre desde Serial Control App

  serial.on("data", serialEvent);
```
Y por último, una función similar al keyPressed que afecte a los eventos:

```js
function serialEvent() {
  let incoming = serial.readLine().trim();
  if (incoming.length > 0) {
    if (incoming === "A") eventA = true;
    if (incoming === "B") eventB = true;
    if (incoming === "T") eventT = true;
    if (incoming === "S") eventS = true;
  }
```

Luego, vamos a MicroPython y usamos el siguiente código para pasarlo a microbit y que sepa qué es lo que tiene que escribir en el serial una vez presionemos los botones:

```py
from microbit import *

while True:
    if button_a.was_pressed():
        uart.write("A\n")
    if button_b.was_pressed():
        uart.write("B\n")
    if pin_logo.is_touched():
        uart.write("T\n")
    if accelerometer.was_gesture('shake'):
        uart.write("S\n")
```
