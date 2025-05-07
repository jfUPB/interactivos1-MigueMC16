### Solución Propuesta

La lógica que quiero usar es que haya un tiempo de espera después de que se presione A y que, en vez de ir directamente a WIN, espere un segundo a ver si se presiona B, si si, volverá a repetir el mismo proceso hasta que llegue a Shake y ahí si irá a win. Si no, si excede el tiempo de espera en caulquiera de los if, seguirá contando tranquilamente hacia atrás. 

### Código Inicial

```py
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3
COUNT_INTERVAL = 1000
waitingTime =0

current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos
music_playing = False  # Variable para evitar que la música se active múltiples veces

while True:
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG
        music_playing = False  # Reinicia la variable de música
        music.stop()

    if current_state == STATE_CONFIG:
        if button_a.was_pressed():
            countdown = min(countdown + 1000, 60000)  # Máximo 60 segundos
            display.show(int(countdown / 1000))  
        elif button_b.was_pressed():
            countdown = max(countdown - 1000, 1000)  # Mínimo 1 segundo
            display.show(int(countdown / 1000))  
        elif pin_logo.is_touched():
            current_state = STATE_COUNT  
        else:
            display.show(int(countdown / 1000))  

    if current_state == STATE_COUNT:
        while countdown > 0:
            if button_a.was_pressed():
                 waitingTime=utime.ticks_ms()
                 if button_b.was_pressed() and waitingTime < 5000:
                     waitingTime=utime.ticks_ms()
                     if button_a.was_pressed() and waitingTime < 5000:
                         waitingTime=utime.ticks_ms()
                         if accelerometer.was_gesture('shake') and waitingTime < 5000:
                            current_state=STATE_WIN
                            break     
            display.show(int(countdown / 1000))  
            countdown -= 1000
            sleep(1000)

        if countdown <= 0 and current_state != STATE_WIN:
            # Solo mostrar la cara triste si realmente se perdió
            display.show(Image.SAD)
            if not music_playing:
                music.play(music.DADADADUM, wait=False)  
                music_playing = True

            while not accelerometer.was_gesture('shake'):
                sleep(100)
            current_state = STATE_INIT
    
    if current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music.play(music.BIRTHDAY, wait=False)  
            music_playing = True

        while not accelerometer.was_gesture('shake'):
            sleep(100)
        current_state = STATE_INIT
```

### Problemas

El While está bloqueando la ejecución de los if, por lo que nunca va a entrar el microbit a verificar si se está ingresando la secuencia. 
Para esto, existe una solución: 

* Crear un Array (secuence[]) para almacenar el código de desactivado de la bomba
* Una implementación correcta de utime.ticks_ms() (la estaba usando mal, es indispensable el utime.ticks_diff()
* En vez de hacer el conteo regresivo con un while, lo vamos a hacer con un if que esté al mismo nivel de los if para la secuencia de botones

### Código Final

```py
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3

COUNT_INTERVAL = 1000
waiting_time = 0
sequence = []  # Almacena la secuencia de entrada

current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos
music_playing = False

last_time = utime.ticks_ms()

while True:
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador
        sequence = []  # Reinicia la secuencia
        current_state = STATE_CONFIG
        music_playing = False
        music.stop()

    if current_state == STATE_CONFIG:
        if button_a.was_pressed():
            countdown = min(countdown + 1000, 60000)
        elif button_b.was_pressed():
            countdown = max(countdown - 1000, 1000)
        elif pin_logo.is_touched():
            current_state = STATE_COUNT
            last_time = utime.ticks_ms()
        display.show(int(countdown / 1000))

    if current_state == STATE_COUNT:
        now = utime.ticks_ms()
        
        if button_a.was_pressed():
            sequence.append('A')
            waiting_time = utime.ticks_ms()
        if button_b.was_pressed() and len(sequence) == 1 and utime.ticks_diff(utime.ticks_ms(), waiting_time) < 5000:
            sequence.append('B')
            waiting_time = utime.ticks_ms()
        if button_a.was_pressed() and len(sequence) == 2 and utime.ticks_diff(utime.ticks_ms(), waiting_time) < 5000:
            sequence.append('A')
            waiting_time = utime.ticks_ms()
        if accelerometer.was_gesture('shake') and len(sequence) == 3 and utime.ticks_diff(utime.ticks_ms(), waiting_time) < 5000:
            sequence.append('SHAKE')

        if sequence == ['A', 'B', 'A', 'SHAKE']:
            current_state = STATE_WIN

        if utime.ticks_diff(now, last_time) >= COUNT_INTERVAL:
            countdown -= 1000
            last_time = now
            display.show(int(countdown / 1000))

        if countdown <= 0 and current_state != STATE_WIN:
            display.show(Image.SAD)
            if not music_playing:
                music.play(music.DADADADUM, wait=False)
                music_playing = True
            while not accelerometer.was_gesture('shake'):
                sleep(100)
            current_state = STATE_INIT

    if current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music.play(music.BIRTHDAY, wait=False)
            music_playing = True

        while not accelerometer.was_gesture('shake'):
            sleep(100)
        current_state = STATE_INIT

```
