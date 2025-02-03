### Display de Imágenes 

En la página de Microbit se encuentra este ejemplo de cómo en pantalla se puede crear alguna animación, con varias imgégenes sucedieéndose entre sí. 
En este ejemplo, no hay una imagen de biblioteca a la cual llamar, por lo que se dibuja en pantalla led a led. Cada una de estas líneas en el código 
representan una de las líneas en la pantalla de mmicrobit. Cinco líneas de 5 leds cada una sirvenn para representar los 25 leds del microbit. 
El 0 representa nula luminosidad y 9 toda la luz. Por ello, se le puede indicar al código que led se debe encender para crear una imagen y, posteriormente,
una secuencia de imágenes. 

```py
from microbit import *

while True:
    display.show(Image(
        "00000:"
        "00900:"
        "09990:"
        "00900:"
        "00000"))
    sleep(500)
    display.show(Image(
        "00000:"
        "09990:"
        "09990:"
        "09990:"
        "00000"))
    sleep(500)
    display.show(Image(
        "90909:"
        "09990:"
        "99999:"
        "09990:"
        "90909"))
    sleep(500)
```
### El código

Básicamente usé el código de la página como base y creé una nueva animación con 8 imágenes y un sleep menor para que la secuencia fuera más rápida. 

```py
    from microbit import *

while True:
    display.show(Image(
        "90909:"
        "00000:"
        "00000:"
        "00000:"
        "09090"))
    sleep(100)
    display.show(Image(
        "00000:"
        "90909:"
        "00000:"
        "09090:"
        "00000"))
    sleep(100)
    display.show(Image(
        "00000:"
        "00000:"
        "99999:"
        "00000:"
        "00000"))
    sleep(100)
    display.show(Image(
        "00000:"
        "09090:"
        "00000:"
        "90909:"
        "00000"))
    sleep(100)
    display.show(Image(
        "09090:"
        "00000:"
        "00000:"
        "00000:"
        "90909"))
    sleep(100)
    display.show(Image(
        "00000:"
        "09090:"
        "00000:"
        "90909:"
        "00000"))
    sleep(100)
    display.show(Image(
        "00000:"
        "00000:"
        "99999:"
        "00000:"
        "00000"))
    sleep(100)
    display.show(Image(
        "00000:"
        "90909:"
        "00000:"
        "09090:"
        "00000"))
    sleep(100)
```
