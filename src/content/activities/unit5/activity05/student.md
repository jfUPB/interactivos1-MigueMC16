### Respuestas

1. En cuanto a eficiencia y velocidad, es preferible el envío con binario, ya que requiere menos bytes para enviar la misma información que el ASCII, como se puede ver en este ejemplo: 

```py
struct.pack('>2h2B', -327, 289, 0, 1)
```
#### Resultado: 
FE D9 01 21 00 01

```py
```
#### Resultado:

Ahora, en cuanto a facilidad y uso de recursos, es preferible ASCII, pues la forma en la que está redactado el paquete es más intuitiva que la binaria (no hay que especificar, por ejemplo, el tipo de dato que se va a enviar de la forma 2b2h, como lo hace el binario) y además, el procesamiento de datos ASCII requiere menos líneas de código, pues en vez de recorrer una matriz indicando cuales son los índices que corresponden al valor de una variable, aquí sólo hay que separar por comas y el byte que indica salto de línea es el byte donde acaba el paquete. 

2. En necesario para que el micro:bit no lea datos erróneos o desincronizados, indicando donde debe iniciar un paquete de datos y donde se espera que termine, para su correcto almacenamiento y procesamiento. 

3. El framing funciona poniéndole una "cabeza" y una "cola" a un paquete de datos. La cabeza se encarga de indicar el inicio del paquete y la cola simplemente indica el final y es una comprobación de la validez de los datos.

4. Un carácter de sincronización es un elemento en el código que ayuda a controlar los tiempos de ejecución y de recepción de datos, con lo cual se puedan realizar lecturas precisas y correctas. 

5. el checksum comprueba que los bytes enviados 

6. En la función readSerialData()

* concat() concatena (Nunca mejor dicho) los bytes que se reciben desde el puerto y se guardan en newData. En newData hay un montón de bytes y hay que ponerlos en orden para su correcta lectura

* El bucle está ahí por que se esperan paquetes de 8 bytes, si no tiene 8 bytes, ya sabemos de entrada que es un paquete no válido, por lo que es nuestro primer filtro de descarte. 

* Nuestro segundo filtro de descarte es la cabeza del paquete de datos. Si se lee un byte que no sea ese, se sabe que no es el inicio de un paquete de datos y se descarta hasta encontrar el 0xAA.

* Precisamente, Shift es la orden que se da para que, si hay un byte que no sea cabeza de paquete, se descarte. Continue simplemente hace que se siga leyendo hasta que se encuentre el bendito 0xAA.

* Si se almacena un paquete, y tiene menos de 8 bytes, se deja de leer, no vale la pena seguir procesándolo por que ya sabemos que está malo.

* Slice te saca una "tajada" del vector de bytes recolectado. En cambio, splice lo disuelve, por eso no se puede disolver o cortar el vector sin antes sacar una tajada del mismo.

* El reduce es muy útil por que te permite reducir un array a un solo valor, que puede ser un entero, un string, otro array, y por ello es un método tan útil para poder realizar una suma de los elementos de un vector. Sólo necesita que le indiques una función que se encargue de reducir y el valor inicial.

* Se supone que los bytes tienen un checksum original. DE NO TENERLO, significa que están corruptos y que no se pueden usar.

* Continue deja que sigas con la lectura de bytes, pues esto es un flujo constante de información por parte del micro:bit.

* El DataView es un objeto necesario para leer datos binarios que tiene los métodos necesarios para interpretar floats, enteros de 16 o 32 bits, etc. Te ahorra varias conversiones extrayendo datos directamente del buffer. 

* Es necesario hacer las conversiones por que los datos en el buffer son bytes en binario, y no número enteros como los necesitamos en el código. 
