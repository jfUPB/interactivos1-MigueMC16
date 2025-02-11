### Pruebas de Código

### Primer Vector: 
Presiono B 10 veces, presionar A 7 veces, Presionar Logo, sacudir el microbit al final de la cuenta regresiva. Además, Inciaré countdown en 9000.

#### Resultado Esperado:
El conteo baja hasta 1, pese a sguir presionando B, no baja de 1. Sube a 8, entra en la cuenta regresiva y vuelve a INIT.

#### Resultado Final: 
Pasa lo descrito, pero al final, la música se reproduce de forma muy extraña. Además, por la forma en la que está programado, el microbit durante la cuenta regresiva no tiene concurrencia.

### Corrección: 

```py
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3
COUNT_INTERVAL = 1000

current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos
music_playing = False  # Variable para evitar que la música se active múltiples veces

while True:
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG
        music_playing = False  # Reinicia la variable de música

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
                countdown = 0
                current_state = STATE_WIN
            else:
                display.show(int(countdown / 1000))  
                countdown -= 1000
                sleep(1000)

        # Cuando el countdown llega a 0, reproducir sonido y mostrar imagen (solo una vez)
        display.show(Image.SAD)
        if not music_playing:
            music.play(music.DADADADUM, wait=False)  # Se ejecuta sin bloquear
            music_playing = True

        # Esperar a que se agite para reiniciar
        while not accelerometer.was_gesture('shake'):
            sleep(100)  # Pequeña pausa para evitar loops rápidos
        current_state = STATE_INIT
    
    if current_state == STATE_WIN:
        display.show(Image.HAPPY)
        if not music_playing:
            music.play(music.BIRTHDAY, wait=False)  # Se ejecuta sin bloquear
            music_playing = True

        # Esperar a que se agite para reiniciar
        while not accelerometer.was_gesture('shake'):
            sleep(100)
        current_state = STATE_INIT

```
Añadí un nuevo evento que implicó unnuevo estado: ¿qué pasa si al presionar A en COUNT desactivo la bomba? el microbit estaría forzado a dejar la cuenta regresiva y pasar a un estado de victoria, donde reproduzco música alegre 
y una cara feliz. Easí que el while donde está la cuenta regresiva, tiene que tener esa condición en cuenta. Además, la música se estaba loopeando una y otra vez, por lo que tuve que usar una variable llamada music_playing
para que sólo se reprodujera una vez en un estado, pues eso era lo que la hacía sonar raro: 

### Segundo Vector:
Presionar B, Presionar B, Presionar Touch Logo, Presionar A, Agitar. 

#### Resultado Esperado: 
El conteo inicia en 9, baja a 8, baja a 7, entra al conteo, luego al estado de WIN y de vuelta a INIT.

#### Resultado Final:
El código unciona bien hasta el conteo, pero nunca cae en el estdo de WIN y al presionar A parece como si hubiera perdido. Estp se debe a que antes de que el código pueda ir a WIN, pone countdown en 0, lo que lo obliga
a romper el loop y darme la ara tiste que representa que la bomba explotó.

### Código Corregido: 
 
```py
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3
COUNT_INTERVAL = 1000

current_state = STATE_CONFIG
countdown = 20000  # Inicia en 20 segundos
music_playing = False  # Variable para evitar que la música se active múltiples veces

while True:
    if current_state == STATE_INIT:
        countdown = 20000  # Reinicia el contador a 20 segundos
        current_state = STATE_CONFIG
        music_playing = False  # Reinicia la variable de música

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
                current_state = STATE_WIN  # Cambio inmediato al estado de victoria
                break  # Salgo del loop para evitar que siga ejecutando el estado de pérdida

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
Uso break en vez de countdown = 0 para romper el loop de While y, ahí si, caer en WIN

### Tercer Vector:
Presionar B, Presionar B, Presionar Touch Logo, Presionar A, Agitar. 

#### Resultado Esperado: 
El conteo inicia en 9, baja a 8, baja a 7, entra al conteo, luego al estado de WIN y de vuelta a INIT.

#### Resultado Final:
pasa lo descrito.

### Cuarto Vector:
Presionar B, Presinar B, Presionar A, Presionar el Logo, Apretar A en medio de la cuenta regresiva y luego sacudir el microbit. Además, iniciaré countdown en 9000. 

#### Resultado Esperado:
El conteo debería iniciar en 9, bajar a 8, bajar a 7 subir a 8 y entrar en cuenta regresiva, después pasar a WIN y volver a INIT.

#### Resultado Final: 
Pasa lo descrito, pero la música de WIN se sigue reproduciendo incluso en INIT, y no es mi intención que se reproduzca por fuera de su estado. Es por ello que en init debo poner music.stop en INIT para que deje de sonar. 

### Quinto Vector:
Presionar B, Presinar B, Presionar A, Presionar A, Presionar A, Presionar el Logo, Apretar A en medio de la cuenta regresiva y luego sacudir el microbit. Además, iniciaré countdown en 9000. 

#### Resultado Esperado:
El conteo debería iniciar en 9, bajar a 8, bajar a 7 subir a 8, subir a 9, subir a 10 y entrar en cuenta regresiva, después pasar a WIN, reproducir música y volver a INIT, parando la música.

#### Resultado Final: Pasa lo descrito. 

### Código Final: 
```py
from microbit import *
import utime
import music

STATE_INIT = 0
STATE_CONFIG = 1
STATE_COUNT = 2
STATE_WIN = 3
COUNT_INTERVAL = 1000

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
                current_state = STATE_WIN  # Cambio inmediato al estado de victoria
                break  # Salgo del loop para evitar que siga ejecutando el estado de pérdida

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
