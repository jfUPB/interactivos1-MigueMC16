### Solución

Mi antiguo semáforo funcionaba mostrando las iniciales de los colores. Pero ahora no puedo hacer eso por que una letra ocupa toda la pantalla y necesito que en la pantalla se muestren los trés semáforos. Por eso, propongo para cada semáforo usar una columna distinta de la pantalla, la 0, la 2 y la 4. Así entonces, puedo hacer
que el rojo, el verde y el amarillo sean patrones que los tres semáforos puedan usar sin que se sobrepongan y que pueda heredar de la clase: La luz roja será iluminar todas las led de la columna, la amarilla, las tres primeras led y la luz verde simplemente iluminará la primera. 

### Código

``` py
# Imports go at the top
from microbit import *
import utime

class Semaforo:
    def __init__(self, redTicks=0, yellowTicks=0, greenTicks=0, xCoord=0):
       self.redTicks= redTicks
       self.yellowTicks= yellowTicks
       self.greenTicks= greenTicks
       self.xCoord= xCoord
       self.state = "Green"
       self.startTime = utime.ticks_ms()
       self.intervals = {"Green": greenTicks, "Yellow": yellowTicks, "Red": redTicks}
       self.running = True
       self.update_display() 
    def update_display(self):
        display.set_pixel(self.xCoord,0,0)
        display.set_pixel(self.xCoord,1,0)
        display.set_pixel(self.xCoord,2,0)
        display.set_pixel(self.xCoord,3,0)
        display.set_pixel(self.xCoord,4,0)
        if self.state == "Green":
            display.set_pixel(self.xCoord, 0, 9)
        elif self.state == "Yellow":
            display.set_pixel(self.xCoord, 0, 9)
            display.set_pixel(self.xCoord, 1, 9)
            display.set_pixel(self.xCoord, 2, 9)         
        elif self.state == "Red":
            display.set_pixel(self.xCoord, 0, 9)
            display.set_pixel(self.xCoord, 1, 9)
            display.set_pixel(self.xCoord, 2, 9)
            display.set_pixel(self.xCoord, 3, 9)
            display.set_pixel(self.xCoord, 4, 9)
    
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


semaforo1 = Semaforo(redTicks=5000,yellowTicks=2000,greenTicks=3000,xCoord=0)
semaforo2 = Semaforo(redTicks=3000,yellowTicks=1000,greenTicks=2000,xCoord=2)
semaforo3 = Semaforo(redTicks=4000,yellowTicks=3000,greenTicks=2000,xCoord=4)

while True:
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
```

### Ventajas

Las ventajas de codificar por clases y además que sean concurrentes es que puedo instanciar varios objetos que reproduzcan a la vez señales en un solo dispositivo y que cada uno tenga características diferentes SIN tener que reescribir el código para cada uno. Siempre que tengan la misma construcción y funciones o formas de actuar similares, puedes ahorrar mucho espacio de código con una clase. 

### Correcciones al Código

Ahora, mi código tiene algunas cosas mejorables: Uso demasiado la función display.set_pixel() y podría implmementar sus iteraciones como un arreglo, así puedo ahorrar líneas de código. Para eso, ChatGPT recomendó la solución "Patterns" que ayuda a definir los leds que tienen que estar encendidos en una fila o columna y ya la arquitectura del programa hace el resto, al haber ya definido que los semáforos iban a trabajar por columnas. Así mismo, para apagar los led en cada cambio de estado, en vez de ir uno a uno apagándolos, como lo hacía yo, Chat GPT sugirió un ciclo for que recorriera las coordenadas y, del 0 al 4, seteando los pixeles en 0 de iluminación, ahorrando así bastante espacio. 

### Código Optimizado

```py
# Imports go at the top
from microbit import *
import utime

class Semaforo:
    def __init__(self, redTicks=0, yellowTicks=0, greenTicks=0, xCoord=0):
        self.redTicks = redTicks
        self.yellowTicks = yellowTicks
        self.greenTicks = greenTicks
        self.xCoord = xCoord
        self.state = "Green"
        self.startTime = utime.ticks_ms()
        self.intervals = {"Green": greenTicks, "Yellow": yellowTicks, "Red": redTicks}
        self.running = True
        
        # Patrones de iluminación por estado
        self.patterns = {
            "Green": [9, 0, 0, 0, 0],
            "Yellow": [9, 9, 9, 0, 0],
            "Red": [9, 9, 9, 9, 9]
        }
        
        self.update_display()

    def update_display(self):
        # Apagar todas las luces de la columna
        for y in range(5):
            display.set_pixel(self.xCoord, y, self.patterns[self.state][y])

    def update(self):
        if not self.running:
            return
        
        if utime.ticks_diff(utime.ticks_ms(), self.startTime) > self.intervals[self.state]:
            self.startTime = utime.ticks_ms()
            self.state = {"Green": "Yellow", "Yellow": "Red", "Red": "Green"}[self.state]
            self.update_display()

    def stop(self):
        self.running = False


semaforos = [
    Semaforo(redTicks=5000, yellowTicks=2000, greenTicks=3000, xCoord=0),
    Semaforo(redTicks=3000, yellowTicks=1000, greenTicks=2000, xCoord=2),
    Semaforo(redTicks=4000, yellowTicks=3000, greenTicks=2000, xCoord=4)
]

while True:
    for semaforo in semaforos:
        semaforo.update()

```
