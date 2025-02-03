### ¿Qué quiero comprobar?

Quiero hacer un código en el cual reproduzca música en el microbit mientras que un botón esté presionado, y pare cuando el botón deje de estarlo. Así podré comprobar el estado de cada botón.

### Código que voy a usar

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

No funcionó. 

### Corrección de código #1
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
