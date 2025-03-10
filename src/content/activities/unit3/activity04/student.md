### Código en MicroPython: 

``` py
# Imports go at the top
from microbit import *
import utime
import music

# Estados de la máquina
STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3
STATE_LOOSE = 4

COUNT_INTERVAL = 1000
waitingTime = utime.ticks_ms()  # Inicializa correctamente la espera
Password = ["A", "B", "A"]
solution = []

# Estado inicial
current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos
music_playing = False  # Para evitar repetir la música

# Variables globales de eventos
eventA = False
eventB = False
eventT = False
eventS = False

def tareaBomba():
    global current_state, countdown, music_playing, eventA, eventB, eventT, eventS, waitingTime, solution, Password

    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG
        music_playing = False  # Reinicia la variable de música
        music.stop()
    
    if current_state == STATE_CONFIG:
        if eventA:  
            countdown = min(countdown + 1000, 60000)  # Máximo 60 segundos
            display.show(int(countdown / 1000))
            eventA = False  

        elif eventB:  
            countdown = max(countdown - 1000, 1000)  # Mínimo 1 segundo
            display.show(int(countdown / 1000))
            eventB = False  

        elif eventT:  
            current_state = STATE_COUNT
            display.clear()  # Limpia la pantalla antes de iniciar la cuenta regresiva
            eventT = False  

        else:
            display.show(int(countdown / 1000))

    if current_state == STATE_COUNT:
        display.clear()
        # Control de la cuenta regresiva
        if utime.ticks_diff(utime.ticks_ms(), waitingTime) > COUNT_INTERVAL:
            countdown -= 1000
            waitingTime = utime.ticks_ms()
            display.set_pixel(2, 2, 9)  # Muestra un punto en el centro
            sleep(100)
            display.clear()
            
        if eventA and len(solution) < len(Password):  
            solution.append("A")
            eventA = False

        if eventB and len(solution) < len(Password):  
            solution.append("B")
            eventB = False

        # Verificación de la clave
        if len(solution) == len(Password):
            if solution == Password:
                current_state = STATE_WIN
            else:
                solution = []  # Reset para otro intento

        if countdown <= 0:
            current_state = STATE_LOOSE

        if eventS:
            eventS = False
            current_state = STATE_INIT

    if current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music_playing = True
            music.play(music.BIRTHDAY, wait=False)  # Se reproduce solo una vez
        
        if eventS: 
            current_state = STATE_INIT
            
    if current_state == STATE_LOOSE:
        display.show(Image.SAD)
        if not music_playing:
            music_playing = True
            music.play(music.DADADADUM, wait=False)  # Se reproduce solo una vez
        
        if eventS: 
            current_state = STATE_INIT

# Función de eventos
def tareaEventos():
    global eventA, eventB, eventT, eventS  

    if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('A'):
                eventA = True
            if data[0] == ord('B'):
                eventB = True
            if data[0] == ord('T'):
                eventT = True
            if data[0] == ord('S'):
                eventS = True

    if button_a.was_pressed():
        eventA = True
    if button_b.was_pressed():
        eventB = True
    if pin_logo.is_touched():
        eventT = True
    if accelerometer.was_gesture('shake'):
        eventS = True

while True:
    tareaEventos()  
    tareaBomba()
```

### Código en Javascript

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
    let sendBtn1 = createButton('Press A');
    sendBtn1.position(220, 0);
    sendBtn1.mousePressed(sendBtnClick1);
    let sendBtn2 = createButton('Press B');
    sendBtn2.position(220, 100);
    sendBtn2.mousePressed(sendBtnClick2);
    let sendBtn3 = createButton('Shake');
    sendBtn3.position(220, 200);
    sendBtn3.mousePressed(sendBtnClick3);
    let sendBtn4 = createButton('Touch Logo');
    sendBtn4.position(220, 300);
    sendBtn4.mousePressed(sendBtnClick4);
   
}
 


function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick1() {
    port.write('A');
}
function sendBtnClick2() {
    port.write('B');
}
function sendBtnClick3() {
    port.write('S');
}
function sendBtnClick4() {
    port.write('T');
}
```
