### Entradas del Micro:bit 
* Botones A y B ⏺️: Según como se programen, los botones al hundirlos ordenan al microbit determinadas cosas.
* Micrófono 🎤: Recibe señales de audio. Si el microbit tiene indicaciones para hacer algo con las mismas, las procesará. 
* Acelerómetro 🏃: Manda información al microbit sobre si está experimentando aceleración (ejm: si lo agitas).
* Botón del logo ⏹️: Funciona como botón adicional independiente de los A y B.

### Salidas del Micro:bit 
* Parlante o Speakers 🔈: Deja al usuario oir sonidos provenientes del microbit.
* Luces Led 🚨: Reproduce imágenes de 5x5 pixeles, simulando una pantalla.
* Indicador de Micrófono 🎤: Le deja saber al usuario cuando el micrófono está siendo activado.
* Led Amarillo USB 🟡: Se enciende cuando el microbit está interactuando con un computador. 

### ¿Qué se puede hacer con las entradas?
#### Botones: 
Puedes hacer un contador, oprimiendo ambos botones para resetarlo, oprimiedo B para sumar y oprimiendo A para mostrar los números arriba de 10 deslizándose. 

```Phyton
from microbit import *


count = 0

display.show(count)


while True:

    if button_a.is_pressed() and button_b.is_pressed():

        count = 0

        display.scroll(count)

    elif button_b.is_pressed():

        count += 1

        display.scroll(count)

    elif button_a.is_pressed():

        display.scroll(count)

    sleep(100)
```

#### Micrófono: 
Puedes hacer que se muestre un corazón en pantalla si aplaudes, usando variables de LOUD y QUIET, que es como el microbit identifica niveles de sonido.

```Phyton
from microbit import *


while True:

    if microphone.current_event() == SoundEvent.LOUD:

        display.show(Image.HEART)

        sleep(200)

    if microphone.current_event() == SoundEvent.QUIET:

        display.show(Image.HEART_SMALL)
```

#### Acelerómetro: 
Puedes hacer un piedra, papel y tijeras con dos microbits. Se programan asignado a cada uno de los eventos un número de 0 a 2. Mientras agitas el micrbit, 
este escoge un númeor random entre 0 y 2, y al dejar de agitarlo, mostrará en pantalla su elección.  

```Phyton
from microbit import *

import random



while True:

    if accelerometer.was_gesture('shake'):

        tool = random.randint(0,2)

        if tool == 0:

            display.show(Image.SQUARE_SMALL)

        elif tool == 1:

            display.show(Image.SQUARE)

        else:

            display.show(Image.SCISSORS)
```

```Phyton
```
  
