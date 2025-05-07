### Respuestas

#### ¿Por qué en la unidad 4 se necesitaba el salto de línea?

Antes no podíamos determinar el tamaño de los paquetes de datos, con lo cual necesitabamos un salto de línea que indicara cuando acababa un paquete en concreto. Ahora, 
con el nuevo formato, sí podemos controlar que tan grandes van a ser dichos paquetes y no necesitamos un indicador al final que marque el final de los mismos. 

### ¿Qué tienen de distintos ambos códigos?

En esta versión de lectura binaria del código, la lectura iniciará siempre que haya seis o más bits en el puerto, pero sólo va a leer los primeros seis que lleguen, depositándolos en un array de seis dimensiones. De la posición 0 a la 1, se convertirán esos dos bites en un entero usando una función que no conocía llamada getInt16. Lo mismo con las posiciones 2 y 3 del vector. Para la cuatro, que es el estado el botón A, s usa getUint8 y la misma lógica aplica para el botón B, con lo cual, vemos cambios en la lógica usada para la traducción y almacenamiento de los datos.

### ¿Por qué puede ocurrir el error de sincronización? 

Puede deberse a que, como ahora con binario podemos enviar la misma información con menos bites en comparación de ASCII, el microbit pueda hacer sends mucho más rápido de lo que p5js puede leer (por que estamos trabajando también con el mismo sleep usado para ASCII) entonces eso hace que se almacenen datos erróneos. 

### ¿Qué se ve después de los cambios?

Ahora en los códigos hay varias cosas distintas: El microbit envía un paquete mucho más completo, donde el primer bite es un encabezado (0xAA) y el último es una comprobación de que la suma de los 6 datos recibidos sea una suma válida para demarcar el final de un paquete completo. Entonces, al llegar a p5js, se buscan el bite de encabezado y se descartan los que estén antes de este. Una vez lo encuentra, divide el vector del paquete de datos y hace lo siguiente: aparta las posiciones 1 a la 6 por que esos son los datos a validar. Los valida con checksum, que es el dato en la posición 7, y si resulta que son validos, ahora si procedemos a implementarlos en la aplicación.
