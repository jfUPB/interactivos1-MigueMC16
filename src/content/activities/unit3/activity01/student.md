### Solución

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

### Correcciones al Código

### Código Optimizado
