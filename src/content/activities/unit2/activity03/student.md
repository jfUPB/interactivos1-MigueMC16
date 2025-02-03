### Entradas del Micro:bit 
* Botones A y B ⏺️: Según como se programen, los botones al hundirlos ordenan al microbit determinadas cosas.
* Micrófono 🎤: Recibe señales de audio. Si el microbit tiene indicaciones para hacer algo con las mismas, las procesará. 
* Acelerómetro 🏃: Manda información al microbit sobre si está experimentando aceleración (ejm: si lo agitas).
* Botón del logo ⏹️: Funciona como botón adicional independiente de los A y B.

### Salidas del Micro:bit 
* Parlante o Speakers 🔈: Deja al usuario oir sonidos provenientes del microbit.
* Luces Led 🚨: Reproduce imágenes de 5x5 pixeles, simulando una pantalla.
* Radio 📻: Mediante una antena de radio, el microbit manda señales a otros microbit.
* Led Amarillo USB 🟡: Se enciende cuando el microbit está interactuando con un computador. 

### ¿Qué se puede hacer con las entradas?
#### Botones: 
Puedes hacer un contador, oprimiendo ambos botones para resetarlo, oprimiedo B para sumar y oprimiendo A para mostrar los números arriba de 10 deslizándose. 

```py
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

```py
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

```py
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

### ¿Qué se puede hacer con las salidas?

#### Altavoz: 
Puedes hacer un metrónomo que emita un sonido en un tick y descanse por un número determinado de ticks. 

```py
from microbit import *

import music

tempo = 100


while True:

    music.set_tempo(bpm=tempo)

    music.play(['C4:1', 'r:3']) # play C for 1 tick, rest for 3 ticks

    if button_a.was_pressed():

        tempo -= 5

    if button_b.was_pressed():

        tempo += 5  
```

#### Luces Led: 
Puedes hacer que el microbit reproduzca caras con las luces led. 

```py
from microbit import *



while True:

    if button_a.is_pressed():

        display.show(Image.HAPPY)

    if button_b.is_pressed():

        display.show(Image.SAD)
```

#### Radio: 
Puedes enviar de un micro a otro la imagen de un pato. Si agitas el microbit envía un mensaje que indica duck. Mediante el radio, y siempre que los dos microbit
estén en el mismo grupo de onda, el otro microbit recibirá el mensaje de duck y reproducirá la imagen de un pato. 

```py
from microbit import *

import radio

radio.config(group=23)

radio.on()



while True:

    message = radio.receive()

    if message:

        display.show(Image.DUCK)

    if accelerometer.was_gesture('shake'):

        display.clear()

        radio.send('duck')

```
  
