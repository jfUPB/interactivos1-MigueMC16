### Descripción del Código: 

El código es exactamente el mismo del de la actividad 6, no realicé cambios en el de micro:bit pero si en el archivo de p5.js.
El código original ya hacía lo que necesitaba pero dibujaba un círculo en vez de un cuadrado, así que sólo modifiqué esas líneas para que dibujara la figura deseada: 

```js
function draw() {

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            fill('red');
        }
        else if(dataRx == 'B'){
            fill('yellow');
        }
        else{
            fill('green');
        }
        background(220);
        square(width / 2, height / 2, 100);
        fill('black');
        text(dataRx, width / 2, height / 2);
    }
```

#### Funcionamiento: 
El micro:bit se carga con un código que logra comunicarse con el código en sketch.js a través del index del archivo de p5.js. Es capaz de enviar señales a dicho código, las cuales deben tener alguna funcionalidad o representación en sketch.js. Así mismo, este puede enviar mensajes a través de eventos para que el micro:bit los reciba y los interprete se haya programado desde un inicio. Estos mensajes pueden ser caracteres, números y demás. 
