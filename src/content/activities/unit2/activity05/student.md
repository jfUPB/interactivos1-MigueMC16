### ¿Qué son los datos y puertos seriales?

Este semestre estoy viendo Internet de las Cosas y es un asunto que corresponde más a dicha materia, pero para hacer la historia corta, un puerto serial es una conexión entre
dos dipositivos, uno emisor y otro receptor de los datos. Con este método, los datos se transmmiten bit a bit con un estándar de comunicación RS- 232. Este tipo de comunicación
no es el más rápido actualmente aunque han habido mejorías en la velocidad de tansmisión de los cables. Hay dificultades respecto a la longitud de los cables y sus competencias 
como os puertos paralelos y el Ethernet. 

Send Love, el ejemplo que propone esta unidad es un ejemplo de envío de datos seriales desde el computador hasta el micro:bit. Por ence, el programa que se vaya a realizar tiene 
que tener esa fundamentación. Mi idea es, como he hecho a lo largo de esta materia, reutilizar código y adaptarlo a mis necesidades actuales. Así, en vez e ordenar al mirco:bit
que me muestre un corazón, le pediré que me mande letras en función de lo que decida el usuario en p5.js.

### Código Referencia p5.js

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
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
    fill('white');
    ellipse(width / 2, height / 2, 100, 100);
}

function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
        }
        else{
            fill('green');
        }
        background(220);
        ellipse(width / 2, height / 2, 100, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
    }


    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h');
}
```

### Código Referencia python

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
        uart.write('C')
        sleep(500)
    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                uart.write(D)
```

### Código Referencia p5.js

```js
let port;
let connectBtn;
let sendBtn;

function setup() {
    createCanvas(400, 400);
    background(220);

    port = createSerial();
    
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(100, 300);
    connectBtn.mousePressed(connectBtnClick);
    
    sendBtn = createButton('Send h');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
}

function draw() {
    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    } else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    if (port.opened()) {
        port.write('h');  // Enviar 'h' al micro:bit
    }
}

```

### Código Modificado python

```py
from microbit import *

uart.init(baudrate=115200)

while True:
    if uart.any():
        data = uart.read(1)
        if data and data[0] == ord('h'):  # Si recibe 'h'
            display.show('h')  # Muestra "h"
            sleep(5000)  # Mantiene la "h" por 5 segundos
            display.clear()  # Limpia la pantalla

```

### ¿Qué cambió?

El microbit no solo estaba recibiendo datos del computador, sino que era capaz de enviar. Así que por motivos de simplicidad, elimié las partes del codigo que en micropython enviaban datos y en java script leían. 
Además, mejoré la interfaz de java script para quq solo mostrara un botón, el de enviar la letra h. Cuando el microbit lo reciba, va a repdroducir una h en pantalla durante 5 segundos (5000 ticks) y después simplemente
limpiaría pantalla a la espera de un nuevo envío. 
