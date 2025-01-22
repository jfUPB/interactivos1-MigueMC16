### ¿Qué es lo está pasando?

#### Cuando se presiona A y B
Lo que ocurre es que el código incluído en p5.js tiene una serie de eventos que van a pasar cuando se identifica que uno de los dos botones está siendo presionados:
```js
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
        }
```
La tarjeta le informa a p5.js que un botón está siendo presionado y p5.js responde rellenando con un color el círculo dibujado. 

#### Cuando se agita el micro
Hay reservado un evento especial para cuando el acelerómetro de la trajeta percibe que está siendo agitado, lo que hace que ella misma mande un tipo de dado distinto de color al programa de p5.js.
```js
if(port.availableBytes() > 0){
        let dataRx = port.read(1);
       ...
        else{
            fill('green');
        }
```
Aquel else existe por que primero el programa corrobora si hay datos ingresando. Sólo pueden haber 3 datos entrantes, y si no es ni A, ni B, es que la tarjeta está haciendo agitada.

#### Cuando se clickea "Send Love"
Aquí ya no es la tarjeta enviando información a p5.js, sino lo opuesto. Cuando esto sucede, el sketch le manda a la micro 'h', y si vamos al código con el que se cargó la tarjeta, esta al recibir 'h' dibuja una imagen en su "panntalla", que es un corazón.

