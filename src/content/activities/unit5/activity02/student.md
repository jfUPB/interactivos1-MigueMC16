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

### Experimento 3
