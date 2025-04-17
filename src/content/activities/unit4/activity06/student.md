### Respuestas a las Preguntas

#### (Soportar todo con pedazos de código luego)

1. Un protocolo de comunicación es un estándar, una forma específica en la que los dispositivos deben enviar y recibir paquetes de datos. Si se comunican con distintos estándares, la recepción de los datos será errónea o directamente imposible. 

2. Se separan por comas dado que así se pueden diferenciar los datos específicos que vienen empaquetados con otros y mantener el orden. cada que una máquina descifre ASCII, sabrá que cuando llegue un 44 será el fin de un dato y que va a recibir uno nuevo

3. Es necesario para que así la máquina receptora no se sature esperando que sigan llegando más datos. Delimitar los paquetes además es útil desde la lógica de programación, pues si esperamos que llegue una cantidad específica de datos, debemos hacer un corte para esa cantidad. 

4. Es necesario por que el código debería poder ejecutarse y prepararse mientras que el micro:bit no esté conectado. Es como si un computador de mesa no pudiera encender hasta que el mouse no esté conectado. 

5. Todo se resetea con el Sleep, que hace que el microbit vuelva a un estado "inicial" para volver a enviar paquetes de datos. 

6. Significa que cada carácter que está enviando está traducido como un número que se puede consultar desde la tabla ASCII, indistinto de si es un carácter numérico o string. 

7. Es necesario que pregunte para que empiece la lectura. De no haber datos disponibles, no tendría por que meterse en una tarea de lectura innecesaria. 

8. Se elimina con el data.trim("\n").

9. Usaría el data.split(" ").

10. Necesita convertir los bytes que estánn llegando a UN SOLO ENTERO, por que los que nos sirve recibir es un 100 y no un 1 0 0. Por eso mismo luego convierte los otros datos a booleanos (los datos de los botones).

11. Por el puerto serial se enviarían los siguientes caracteres: 
[49 50 51 44 55 54 53 44 70 97 108 115 101 44 84 114 117 101].

12. Aprendí a separar las coordenadas tridimiensionales que usa micro:bit para calcular la aceleración que está experimentando en varios ejes.

13. Aprendí el protocolo ASCII. Ya había trabajado con envío de datos seriales antes pero no con este protocolo, sino con paquetes en formato JSON. Y no lo había hecho con micropython y javascript sino con python, así que es un nuevo lenguaje en el cual ya sé enviar y recibir datos seriales. 
