### Código Propuesto: 

El código en p5.js copia del de la actividad 6 la base, susbtrae los elementos que tienen que ver con el círculo cambia color y añade cuatro botones nuevos. La funcionalidad que el enunciado quiere que implemente ya la hacía dicho código con "send love". Send love lo que hace es enviar a micro:bit la letra 'h', el cual muestra la imagen de un corazón, de acuerdo con el código original. Lo único que tuve que hacer es instanciar más botones que envíen más letras ('j', 'k', 'l') y cambiar la imagen con la que el micro:bit  debe responder.

#### Código en p5.js

```js
let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
  
    let sendBtn1 = createButton('Send Love');
    sendBtn1.position(220, 300);
    sendBtn1.mousePressed(sendBtn1Click);
  
    let sendBtn2 = createButton('Send Good Night');
    sendBtn2.position(220, 250);
    sendBtn2.mousePressed(sendBtn2Click);
  
    let sendBtn3 = createButton('Send Good Feelings');
    sendBtn3.position(220, 200);
    sendBtn3.mousePressed(sendBtn3Click);
  
    let sendBtn4 = createButton('Send Math Text');
    sendBtn4.position(220, 150);
    sendBtn4.mousePressed(sendBtn4Click);
}

function draw() {
  background(220);
        
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtn1Click() {
    port.write('h');
}
function sendBtn2Click() {
    port.write('j');
}

function sendBtn3Click() {
    port.write('k');
}
function sendBtn4Click() {
    port.write('l');
}
```

### Código cargado en el micro:bit

```js
from microbit import *

uart.init(baudrate=115200)
display.show(Image.BUTTERFLY)

while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        sleep(500)
    if accelerometer.was_gesture('shake'):
        uart.write('C')
        sleep(500)
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(500)
                display.show(Image.HAPPY)
            if data[0] == ord('j'):
                display.show(Image.SLEEP)
                sleep(500)
                display.show(Image.HAPPY)
            if data[0] == ord('k'):
                display.show(Image.FABULOUS)
                sleep(500)
                display.show(Image.HAPPY)
            if data[0] == ord('l'):
                display.show(Image.CONFUSED)
                sleep(500)
                display.show(Image.HAPPY)
```
