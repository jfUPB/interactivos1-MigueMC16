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
