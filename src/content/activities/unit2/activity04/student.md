### ¿Qué quiero comprobar?

Quiero hacer un código en el cual reproduzca música en el microbit mientras que un botón esté presionado, y pare cuando el botón deje de estarlo. Así podré comprobar el estado de cada botón.

#### Código que voy a usar

```Python
from microbit import *
import music
while True:

    while button_a.is_pressed() and button_b.is_pressed():
        music.FUNK

    while button_b.is_pressed():
        music.WAWAWAWAA

    while button_a.is_pressed():
        music.BLUES
    while pin_logo.is_touched():
        music.CHASE
        
    sleep(100)

```
### Resultados

No funcionó. Eso es por que estoy redactando music.play y no music.play(). Esos paréntesis hacen toda la diferencia, pues pasa el código de entender lo que escribí como 
una variable a entenderlo como función. 

#### Corrección de código #1
```Python
from microbit import *
import music
while True:

    while button_a.is_pressed() and button_b.is_pressed():
        music.play(music.FUNK)

    while button_b.is_pressed():
        music.play(music.WAWAWAWAA)

    while button_a.is_pressed():
        music.play(music.BLUES)
        
    while pin_logo.is_touched():
        music.play(music.CHASE)
        
    sleep(100)
```

### ¿Qué sigue?
Me gustaría que la música parase oprimiendo de nuevo el botón. Mi lógica pasa por poner un contador que registre el número de veces que el botón ha sido presionado. 
Cuando sea par, habrá un if que comprobará esto y dentendrá la música. Cuando sea impar, la música sonará. 

#### Nuevo código
```Python
from microbit import *
import music
countA = 1
countB = 1
countAB = 1
countLogo = 1
while True:

    
    if button_a.is_pressed() and button_b.is_pressed() and countAB%2!=0:
        music.play(music.FUNK)
        countAB = countAB+1
    if countAB%2==0:
        music.stop
        countAB = countAB+1
    if button_b.is_pressed() and countB%2!=0:
        music.play(music.WAWAWAWAA)
        countB = countB+1
    if countB%2==0:
        music.stop
        countB = countB+1
    if button_a.is_pressed() and countA%2!=0:
        music.play(music.BLUES)
        countA = countA+1
    if countA%2==0:
        music.stop
        countA = countA+1
    if pin_logo.is_touched() and countLogo%2!=0:
        music.play(music.CHASE)
        countLogo = countLogo+1
    if countLogo%2==0:
        music.stop
        countLogo = countLogo+1
   
        
    sleep(100)
```
### Resultado
No funnciona por el parámetro wait de la función music.play(), que explicaré más adelante en la actividad 7. Aquí dejo un código corregido y más eficiente, además de 
uno que deja de reproducir la canción una vez el botón deja de ser presionado, lo que, a mi criterio, logra mejor el objetivo de esta unidad, que era comprobar el estado
de los botones.

#### Código Corregido

```Python
from microbit import *
import music

# Variables para controlar el estado de cada sonido
playing_A = False
playing_B = False
playing_AB = False
playing_Logo = False

while True:
    if button_a.is_pressed() and button_b.is_pressed():
        if not playing_AB:  
            music.play(music.FUNK, wait=False, loop=True)  # Reproducir en bucle
        else:
            music.stop()
        playing_AB = not playing_AB  # Alternar estado
        while button_a.is_pressed() and button_b.is_pressed():
            pass  # Esperar hasta que se suelten

    if button_b.is_pressed():
        if not playing_B:
            music.play(music.WAWAWAWAA, wait=False, loop=True)
        else:
            music.stop()
        playing_B = not playing_B
        while button_b.is_pressed():
            pass  

    if button_a.is_pressed():
        if not playing_A:
            music.play(music.BLUES, wait=False, loop=True)
        else:
            music.stop()
        playing_A = not playing_A
        while button_a.is_pressed():
            pass  

    if pin_logo.is_touched():
        if not playing_Logo:
            music.play(music.CHASE, wait=False, loop=True)
        else:
            music.stop()
        playing_Logo = not playing_Logo
        while pin_logo.is_touched():
            pass  

    sleep(100)

```

#### Código Nuevo

```Python
from microbit import *
import music

while True:
    if button_a.is_pressed() and button_b.is_pressed():
        music.play(music.FUNK, wait=False, loop=True)

    elif button_b.is_pressed():
        music.play(music.WAWAWAWAA, wait=False, loop=True)

    elif button_a.is_pressed():
        music.play(music.BLUES, wait=False, loop=True)

    elif pin_logo.is_touched():
        music.play(music.CHASE, wait=False, loop=True)

    else:
        music.stop()  # Se detiene la música cuando ningún botón está presionado

    sleep(100)

```
