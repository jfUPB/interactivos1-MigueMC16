### Respuestas

#### Pregunta 1
Se están enviando un 0,0,False,False en repetidas ocasiones. Puedo inferir que se tratan de los eventos que procesa el micro:bit, que los primeros dos datos se corresponden a valores numéricos y los últimos dos, booleanos, cosa lógica, pues el microbit está enviando su aceleración en x (número), aceleración en y (número), botón A presionado (booleano) y botón B presionado (booleano) por el Serial. 

#### Pregunta 2
Se puede inferir que se van a enviar cuatro cadenas o strings por separado en una sola línea, y luego se incluye un formato en el que se dictan qué datos se deben enviar al Serial como cadenas de caracteres.

#### Pregunta 3
Por mero orden, es fácil decir así el estado en el que se encuentran las 4 variables mencionadas cada 100 ms. 

#### Pregunta 4
Hice el experimento, y es un desorden de caracteres difíciles de interpretar, así que, si bien el código corre sin esas comas o salto de línea, es conveniente ponerlos.

#### Pregunta 5
Esta la sé por haber trabajado en Internet de las Cosas con sensores antes. Estos realizan lecturas cada determinado tiempo y si uno no se sincroniza con ellas, o no recibe ningún dato o recibiría demasiado como para poder leerlos, sobrecargando la consola y haciendo que, eventualmente, al ejecución falle. Además, desgastaría la batería del microchip rapidísimo.

#### Pregunta 6
Si acerco hacia mí el micro:bit, el valor en y  es negativo. Si lo alejo, positivo. Y respecto a x, cuando el micro:bit va hacia la izquierda, el valor es negativo, y si va hacia la derecha, positivo. 

#### Pregunta 7
Si los presiono, naturalmente su estado pasa de False a True.

#### Pregunta 8
Con el was pressed, la lectura llega después, un poco retrasada a si coloco is pressed.

#### Pregunta 9
Cada carácter tiene una conversión en la tabla ASCII. Si recurrimos a esta tabla, habría que convertir esto: 

9 6 9 , 6 5 2  , t r u e , f a l s e \n 

En los bits tipo ASCII, obtendremos esto: 

[57, 54, 57, 44, 54, 53, 50, 44, 84, 114, 117, 101, 44, 70, 97, 108, 115, 101, 10]

Así mismo es como los generadores de Semillas en Minecraft interpretan lo que uno ingrese, incluso si es una cadena de caracteres, por ejemplo. 
