### Hallazgos

#### NOTA:

A las variables microBitX = int(values[0]) + windowWidht/2 y microBitY = int(values[1]) + windowHeight/2 se le suman esos dos parámetros para que  la ubicación 0,0 que se manda en el micro:bit se ubique en el centro del lienzo.

#### NOTA: 

Pues ya hay una verificación con los mensajes impresos en consola, se pueden poner mensajes en consola por ssi los estados acaban por no ocurrir.

#### NOTA: 

updateButtonStates hace que si el botón A es presionado (si antes no lo estaba y ahora sí) haga que el microbit empieze a funcionar como una especie de mouse (haciendo una equivalencia entre sus X y Y y las que envíe el micro:bit) y configura el tamaño de línea. 
También hace que si B estaba siendo presionado y se suelta, se escoja un color aleatorio para la línea. 
Por último, hace que el estado previo de A y B sean los nuevos estados. Esto se hace para que los estados no se queden en un ciclo infinito. Por ejemplo, con A: Si prevmicroBitAState no cambia nunca a true (cosa que sí hace con el código como está si se presiona A) nunca se va a generar un nuevo tamaño de línea ni el código va a dejar entrar nunca nuevos estados para A.

### NOTA: 

El "no se están recibiendo 4 datos del microbit" significa que en el serial no se están escribiendo los datos, por que incluso si no muevo el micro:bit y tampoco oprimo ningún botón, de todas formas debería estar escribiendo 
xValue = 0, yValue = 0, AState = False y BState = False. Aquí pueden ocurrir dos cosas: o el código del microbit está mal escrito (no tiene el write) o el cable del serial está mal conectado. Si no es así, una tercera opción puede ser que p5js abre el puerto ANTES de que el microbit pueda empezar a escribir, y entonces, en esos primeros instantes de lectura, no recibe nada por que el microbit no es muy rápido.

