### Respuestas

#### ¿Cómo se comunican el sketch y el microbit?

Se comunican a través del puerto serial. Se establecen variables que faciliten la conección en p5.js, un puerto, un botón para establecer la conexión y una variable que nos diga si el microbit está conectado o no: 

```js
let port;
let connectBtn;
let microBitConnected = false;
```
Usando las funciones createSerial() se establece el puerto y se crea un botón para que el usuario pueda abrirlo. Si se le da click y el puerto no está abierto, se abre con por.opened(), si se vuelve a dar click y estaba abierto, se cierra con port.close() (todo esto está escrito en la función connectBtnClick())
```js
function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);

  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
  } else {
    port.close();
  }
```
#### ¿Cómo es el paquete de datos enviado por el microbit?

El formato de datos es ASCII y plantea enviar entre corchetes cuatro datos del microbit separados con comas y con un salto de línea, para que, siguiendo este protocolo, al llegar al sketch se pueda hacer una diferenciación de los datos con las comas y se haga tambien una diferenciación de los paquetes de datos gracias al salto de línea. 

```py
data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)
```

#### ¿Qué parte del sketch convierte datos del microbit a coordenadas?

Hay dos partes importantes para que esto se logre: Lo primero es recibir los datos desde el serial y hacer que el dato 0,0 coincida en el centro del lienzo (por eso
se le suma windowWidth/2 y windowHeight/2)
```js
 if (values.length == 4) {
          microBitX = int(values[0]) + windowWidth / 2;
          microBitY = int(values[1]) + windowHeight / 2;
 ```

Ahora que tenemos los datos pulidos, podemos hacer que las coordenadas x y y que usa la función draw para dibujar, coincidan con los valores microBitX y microBitY, 
para que los datos que llegan de microbit, ahora sí, controlen la aplicación. 
```js
  if (microBitAState === true) {
        let x = microBitX;
        let y = microBitY;
```
#### ¿Cómo se generan los eventos A pressed y B released?

### Dibujos Realizados
