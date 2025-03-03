### Lógica de Programación
#### Arquitectura del código propuesto

Como ahora hay que quitar la lectura directa de las tareas de la bomba de los botones de Microbit, reemplacé en los condicionales por las variables booleanas "eventA, eventB, eventS, eventT" y en la tarea de eventops se leen los botones de Microbit para cambiar los valores de los booleanos, que inician en False

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

### Problemas con el Código

No hay forma de que las variables booleanas sean leídas por tareabomba, más específicamente, no puede leer cuando estas cambian de valor, por lo que nunca sucede nada y el microbit solo muestra el número 20 constantemente. Para arreglar esto, opté por declarar como globales a las tareas. En Microbit, dicha declaración funciona distinto a C++, en donde simplemente basta con ponerlas en la parte de arriba del código antes de declarar métodos y tareas. 

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
waitingTime = 0

current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos
music_playing = False  # Variable para evitar que la música

# Declaración de las variables globales
eventA = False
eventB = False
eventT = False
eventS = False

def tareaBomba():
    global current_state, countdown, music_playing, eventA, eventB, eventT, eventS
    countdown = 20000  # Inicia en 20 segundos
    music_playing = False  # Variable para evitar que la música
    
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG
        music_playing = False  # Reinicia la variable de música
        music.stop()

    if current_state == STATE_CONFIG:
        if eventA:  # eventA is true
            countdown = min(countdown + 1000, 60000)  # Máximo 60 segundos
            display.show(int(countdown / 1000))
            eventA = False  # Reset eventA

        elif eventB:  # eventB is true
            countdown = max(countdown - 1000, 1000)  # Mínimo 1 segundo
            display.show(int(countdown / 1000))
            eventB = False  # Reset eventB

        elif eventT:  # eventT is true
            current_state = STATE_COUNT
            eventT = False  # Reset eventT

        else:
            display.show(int(countdown / 1000))

    if current_state == STATE_COUNT:
        while countdown > 0:
            if eventA:  # eventA is true
                waitingTime = utime.ticks_ms()
                eventA = False  # Reset eventA
                
                if waitingTime < 5000 and eventB:  # and eventB is true
                    waitingTime = utime.ticks_ms()
                    eventB = False  # Reset eventB
                    
                    if waitingTime < 5000 and eventA:  # and eventA is true
                        waitingTime = utime.ticks_ms()
                        eventA = False  # Reset eventA
                        
                        if waitingTime < 5000 and eventS:  # and eventS is true
                            current_state = STATE_WIN
                            break  # Break the loop to go to the win state

            display.show(int(countdown / 1000))
            countdown -= 1000
            sleep(1000)

        if countdown <= 0 and current_state != STATE_WIN:
            # Solo mostrar la cara triste si realmente se perdió
            display.show(Image.SAD)
            if not music_playing:
                music.play(music.DADADADUM, wait=False)
                music_playing = True

            while not eventS == True:  # eventS is true
                sleep(100)
            current_state = STATE_INIT

    if current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music.play(music.BIRTHDAY, wait=False)
            music_playing = True

        while not eventS == True:  # eventS is true
            sleep(100)
        current_state = STATE_INIT

# Función de eventos
def tareaEventos():
    global eventA, eventB, eventT, eventS  # Declarar las variables globales
    if button_a.is_pressed():
        eventA = True
    
    if button_b.is_pressed():
        eventB = True
    
    if pin_logo.is_touched():
        eventT = True
    
    if accelerometer.was_gesture('shake'):
        eventS = True

while True:
    tareaEventos()  # Detecta los eventos
    tareaBomba()  # Maneja la lógica del juego
```
### Refinamiento del código

Para que el anterior código funcione, uno necestia dejar oprimido los botones o presionarlos múltiples veces para que el microbit tenga la oportunidad de registrar la acción. Esto es por el uso de IsPressed en eventos.
El programa tiene un tiempo muy corto para realizar todo lo que le pido si el botón es presionado en un isntante específico. Hace las comprobaciones en un santiamén y si no se presiona el botón durante esas comprobaciones, no va a realizar nada. Por eso debe usarse WasPressed. Así, en vez de revisar si el botón está siendo presionado, revisa si fue presionado antes de cada iteración, eliminando la necesidad de dejar el botón oprimido. Ahora sólo falta añadir los datos que llegan por serial y el código está listo.

```py
# Función de eventos
def tareaEventos():
    global eventA, eventB, eventT, eventS  # Declarar las variables globales
    if uart.any(): #Revisa si hay data en el puerto serial
        data = uart.read(1) #Lee un bit de ese dato
        if data: #Comvierte el dato en vector
            if data[0] == ord("A"): #La primera pocisión de un vector es 0. La convierte a letra
                eventA = True
            elif data[0] == ord("B"):
                eventB = True  
            elif data[0] == ord("S"):
                eventS = True  
            elif data[0] == ord("T"):
                eventT = True  
    
    if button_a.was_pressed():
        eventA = True
    
    if button_b.was_pressed():
        eventB = True
    
    if pin_logo.is_touched():
        eventT = True
    
    if accelerometer.was_gesture('shake'):
        eventS = True
```

