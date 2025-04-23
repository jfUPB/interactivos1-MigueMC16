### Vectores de Prueba

* En config Presionaré demasiadas veces A. El resultado esperado es que llegue a 60 y no aumente.
* En config Presionaré demasiadas veces B. El resultado esperado es que llegue a 0 y no aumente.
* De config presionaré A 3 veces y luego T para ir a Count, para presionar S y que me lleve a Init para luego pasar en Config. El contador debería resetearse y volver a 20.
* De config iré a count, ingresaré cuantas contraseñas erradas posibles. El resultado debería ser entrar en loose state.
* De config iré a count, ingresaré La contraseña correcta y después spamearé los botones A y B. Luego de un rato, S El resultado debería ser entrar en Win state y, después de S, ir a init.
* De config iré a count, no haré nada y justo antes de que acabe la cuenta regresiva, oprimiré S. El resultado debería ser ir a init sin pasar por loose. 

### Vectores que Pasaron

Pasaron la pruevba los vectores 1, 2, 3, 4, y 6. 

### Vectores que Fallaron

El vector que no funcionó es el 5. No estoy logrando que se ingrese la contraseña correcta, que se realicen las comprobaciones. Tampoco estoy viendo donde está la falla en el código. Además, está ocurriendo lo siguiente: después de la primera ejecución del código, al acceder de nuevo a CONFIG y oprimir T, se resetea CONFIG y tengo que volver a oprimir T para ir a COUNT. Ese problema pasa específicamente cuando dejo que la máquina de estados llegue a los estados WIN o LOOSE, por que si accedo a CONFIG desde COUNT, el problema con el evento T no ocurre. 

### Correcciones

El evento T se resetea antes de siquiera poder ser usado, con lo que es preferible añadir el reseteo de los eventos de forma más retrasada. De entre las opciones dispoibles, se colocó dicho reseteo después de tarea bomba () en el ciclo while. Con esto en mente, también corregí la lógica de máquina de estados para que no se acceda al estado INIT más de una vez. Ahora los vectores funcionan al completo.

### Código Nuevo

```py
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
waitingTime = utime.ticks_ms()
Password = ["A", "B", "A"]
solution = []

# Estado inicial
current_state = STATE_INIT
countdown = 20000
music_playing = False

# Variables globales de eventos

eventA = False
eventB = False
eventT = False
eventS = False

def tareaBomba():
    global current_state, countdown, music_playing, eventA, eventB, eventT, eventS, waitingTime, solution, Password

    if current_state == STATE_INIT:
        countdown = 20000
        solution = []
        current_state = STATE_CONFIG  # Sólo se ejecuta una vez

    if current_state == STATE_CONFIG:
        music.stop()
        music_playing = False
        if eventT:
            current_state = STATE_COUNT
            display.clear()
        elif eventA:
            countdown = min(countdown + 1000, 60000)
            display.show(int(countdown / 1000))
        elif eventB:
            countdown = max(countdown - 1000, 1000)
            display.show(int(countdown / 1000))
        else:
            display.show(int(countdown / 1000))

    if current_state == STATE_COUNT:
        display.clear()
        if utime.ticks_diff(utime.ticks_ms(), waitingTime) > COUNT_INTERVAL:
            countdown -= 1000
            waitingTime = utime.ticks_ms()
            display.set_pixel(2, 2, 9)
            sleep(100)
            display.clear()

        if eventA and len(solution) < len(Password):
            solution.append("A")
            eventA = False

        if eventB and len(solution) < len(Password):
            solution.append("B")
            eventB = False

        if len(solution) == len(Password):
            if solution == Password:
                current_state = STATE_WIN
            else:
                solution = []

        if countdown <= 0:
            current_state = STATE_LOOSE

        if eventS:
            eventS = False
            countdown = 20000
            solution = []
            current_state = STATE_CONFIG

    if current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music_playing = True
            music.play(music.BIRTHDAY, wait=False)

        if eventS:
            eventS = False
            countdown = 20000
            solution = []
            current_state = STATE_CONFIG

    if current_state == STATE_LOOSE:
        display.show(Image.SAD)
        if not music_playing:
            music_playing = True
            music.play(music.DADADADUM, wait=False)

        if eventS:
            eventS = False
            countdown = 20000
            solution = []
            current_state = STATE_CONFIG

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
    eventT = False
    eventA = False
    eventB = False
    eventS = False
```
