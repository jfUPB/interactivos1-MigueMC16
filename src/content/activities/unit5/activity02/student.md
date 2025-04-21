### Experimento 1

* El resultado que entrega el Serial en modo texto es... nada, no se ve nada. Quizá se deba a que el formato que estamos mandando no es compatible con una lectura de texto ASCII
  
* En modo Hexadecimal se ve esto:

00 00 00 00 00 00
00 00 00 00 00 00
00 00 00 00 00 00

...
Si empiezo a mandar datos, un paquete que contenda xValue = 650, yValue = 77, A = True y B = False, se muestra esto:

02 8A 00 4D 01 00

Creo que se ve así por que a los datos numéricos que se están enviando le pedimos los dos bits más significativos, osea que por xValue va ese 02 y 8A y por yValue ese 00 4D. Por útlimo, 01 (A is True) y 00 (B is false).

* ¿Si me preguntas? se ve un poco más complicado de leer que el ASCII, pues está incluyendo caracteres alfabéticos. Pero veo una ventaja: Envía menos bits a la aplicación receptora, por lo cual creo que es mejor protocolo para aplicaciones más robustas que realicen más envíos.

### Experimento 2

Cómo ya se mencionó previamente, se envían 6 bits. El formato es muy explicativo: Los dos primeros datos con el formato (2b2h) son datos numéricos que piden los bits más significativos y ahí van 4. El 2h simplemente indica que estamos mandando dos booleanos y como un booleano solo tiene dos estados, solo necesita un un bit para expresar true (01) o false (00).

### Experimento 3

La diferencia entre un formato ASCII y un formato binario es que en ASCII es, en un inicio, más fácil de leer, pero cada pequeño símbolo que representa un dato en un paquete de datos, se envía un bit. Además, se envía un bit para representar un salto de línea, una coma, si hay un número de tres cifras pesa más que uno de dos...
Con un formato binario, se tiene mayor control sobre el número de bits que se van a enviar y, aunque no sean fáciles de leer en un inicio, es mucho más ligero el paquete de datos que se manda. 
