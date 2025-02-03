### Volvamos Atrás

Este código ya lo realicé en la unidad 4. Lo explicaré más a profundidad acá: 

```py
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
### Explicando music.play()

* music.play() tiene varios parámetros. Tiene uno obligatorio que es la melodía que tiene que reproducir (o la nota musical que funciona distinto)
* El segundo parámetro, wait, se refiere a si el código debe esperar a que la canción termine de sonar para seguir ejecutándose o si puede dejar a la canción sonar en segundo plano. Es por ello que el código en la unidad 4 no me funcionaba correctamente cuando trataba de parar la música con music.stop(). El código se detenía antes de leer 
music.stop() y por eso la canción sonaba entera. Por defecto, wait siempre viene en true.
* El tercero es si la musica debe sonar en loop hasta nueva orden o si para después de una repetición. Por defecto, viene false.

#### Pequeño inciso: notas musicales

Si yo escribo "C4:2", ¿qué estaré queriendo decir?

* C es la notación anglosajona para Do. Cada nota tiene una letra correspondiente, Si es B, La es A y así.
* 4 corresponde a la octava del Do, pues hay múltiples Do y cada uno puede ser más grave o agudo. Entre ma´s alta la escala, más aguda la nota.
* :2 Se refiere al tiempo que debe durar la nota. Una negra es un pulso, una blanca son dos pulsos, una corchea es medio pulso, etc. 
