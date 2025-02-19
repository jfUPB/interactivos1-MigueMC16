### Lógica de Programación
#### Arquitectura del código propuesto

```py
# Imports go at the top
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
music_playing = False  # Variable para evitar que la música

eventA=False
eventB=False
eventT=False
eventS=False

def tareaBomba():
 current_state = STATE_CONFIG
 countdown = 20000  # Inicia en 20 segundos
 music_playing = False  # Variable para evitar que la música
 while True:
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG
        music_playing = False  # Reinicia la variable de música
        music.stop()

    if current_state == STATE_CONFIG:
        if (): #eventA is true
            countdown = min(countdown + 1000, 60000)  # Máximo 60 segundos
            display.show(int(countdown / 1000))
            #eventA is false
        elif ():#eventB is true
            countdown = max(countdown - 1000, 1000)  # Mínimo 1 segundo
            display.show(int(countdown / 1000)) 
            #eventB is false
        elif ():#EventT is true
            current_state = STATE_COUNT  
        else:
            display.show(int(countdown / 1000))  

    if current_state == STATE_COUNT:
        #EventT is false
        while countdown > 0:
            if (): #eventA is true
                 waitingTime=utime.ticks_ms()
                   #eventA is false
                 if waitingTime < 5000: #and eventB is true
                     waitingTime=utime.ticks_ms()
                      #eventB is false
                     if waitingTime < 5000: #and eventA is true
                         waitingTime=utime.ticks_ms()
                         #eventA is false
                         if waitingTime < 5000: #And eventS is true
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

            while not (): #eventS is true
                sleep(100)
            current_state = STATE_INIT
    
    if current_state == STATE_WIN:
        #eventS is false
        display.show(Image.HAPPY)
        if not music_playing:
            music.play(music.BIRTHDAY, wait=False)  
            music_playing = True

        while not (): #eventS is true
            sleep(100)
        current_state = STATE_INIT

def tareaEventos():
   if button_a.is_pressed():
       eventA=True
   if button_b.is_pressed():
       eventB=True
   if pin_logo.is_touched():
       eventT=True
   if accelerometer.was_gesture('shake'):
       eventS=True
while True:
    tareaBomba()
    tareaEventos()

```
