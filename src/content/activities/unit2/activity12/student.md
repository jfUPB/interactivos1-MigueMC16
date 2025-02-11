### Código Inicial

```py
from microbit import *
import utime
import music

STATE_CONFIG = 0
STATE_COUNT = 1

COUNT_INTERVAL = 1000

current_state = STATE_CONFIG
countdown = 20000

while True:
    # pseudoestado STATE_INIT
    if current_state == STATE_CONFIG and current_state != STATE_COUNT:
        if button_a.was_pressed():
         countdown += 1000
         display.show(countdown/1000)
        elif button_b.was_pressed():
         countdown -= 1000
         display.show(countdown/1000)
        elif pin_logo.is_touched():
            current_state == STATE_COUNT
        else:
         display.show(countdown/1000)
    if current_state == STATE_COUNT and current_state != STATE_CONFIG:
        if pin_logo.is_touched():
            current_state=STATE_CONFIG
        elif countdown >0:
          for i in range (20000):
            display.show(countdown/1000)
            countdown -= 1000
            sleep (1000)
            i += 1000
        elif countdown == 0:
            music.play(music.FUNK, wait=False, loop=False)
 ```

### Código Con Correcciones:

```py
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2

COUNT_INTERVAL = 1000

current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos

while True:
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG  # Vuelve a CONFIG

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
            display.show(int(countdown / 1000))  
            countdown -= 1000
            sleep(1000)

        # Cuando el countdown llega a 0, reproducir sonido y mostrar imagen
        music.play(music.DADADADUM, wait=False, loop=False)
        display.show(Image.SAD)

        # Si se agita el micro:bit, reiniciar el estado
        if accelerometer.was_gesture('shake'):
            current_state = STATE_INIT

```
### ¿Que correcciones hay aplicadas?

* Se reemplazó el ciclo for de el estado de COUNT por un ciclo while y se añadió un pequeño estado INIT por motivos de escalabilidad, así, si hay más variables que se tengan que inicializar antes de entrar a la configuración, se pueden añadir ahí.
* Se usaron los métodos max() y min() para limitar la cantidad de segundos que se pueden poner en CCONFIG.
* Dividir countdown entre 1000 devolvía un float que reproducía en pantalla los números así: 20.0; usando int, ahora se resproducen sin el decimal: 20.
