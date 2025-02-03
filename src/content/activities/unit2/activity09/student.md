### Diseño del Código

Le pedí a ChatGPT el código basado en el de la actividad 8 y en una lógica diseñada por mí, tomando los colores del semáforo como estados. Aquí el prompt:

"Necesito que me diseñes una máquina de estados para microbit en Python usando el siguiente código como referencia. Dicha máquina debe ser un semáforo que tome como clase a la pantalla del microbit. Dicha clase tendrá 3 estados: Red, Yellow y Green. Iniciará en Green por defecto. La pantalla iluminará todos sus leds color verde, de color amarillo cuando esté en estado Yellow y rojo en estado Red. De no ser posible, Reemplaza Green por la letra G en display de la pantalla, para Yellow una Y y para Red una R. Cuando esté en Green, Guardará el tiempo del cambio de estado e iniciará un intérvalo antes de cambiar a Yellow, Intérvalo de 5000 ticks. El estado Yellow durará 2000 ticks antes de pasar a Red. Red 5000 y pasará a Green, repitiéndose en Bucle. Bucle que no será interrumpido hasta que yo no toque el botón del logo del microbit. Este es el código de referencia. "

### Código: 

``` py
from microbit import *
import utime

class TrafficLight:
    def __init__(self):
        self.state = "Green"
        self.startTime = utime.ticks_ms()
        self.intervals = {"Green": 5000, "Yellow": 2000, "Red": 5000}
        self.running = True
        self.update_display()

    def update_display(self):
        display.clear()
        if self.state == "Green":
            display.show("G")
        elif self.state == "Yellow":
            display.show("Y")
        elif self.state == "Red":
            display.show("R")

    def update(self):
        if not self.running:
            return
        
        if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.intervals[self.state]:
            self.startTime = utime.ticks_ms()
            if self.state == "Green":
                self.state = "Yellow"
            elif self.state == "Yellow":
                self.state = "Red"
            elif self.state == "Red":
                self.state = "Green"
            self.update_display()

    def stop(self):
        self.running = False

traffic_light = TrafficLight()

while True:
    traffic_light.update()
    if button_logo.is_pressed():
        traffic_light.stop()
        break
    utime.sleep_ms(100)
```
