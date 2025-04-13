## ¿Cómo funciona el código?

### Antes de Empezar: Qué hace el código: 

El códgio dibuja líneas al presionar el mouse, tomando la posición de este como centro. Dichas líneas se distribuyen en una circunferencia. Se dibujan varias en diversos ángulos que
van variando de acuerdo a una velocidad angular. Hay factores que pueden variar dependiendo del usuario, como el estilo de línea, el color, el tamaño, etc. 

### Primera Parte: Variables Globales, Importes y Set Up

Vamos paso a paso. Para desmenuzar el código, primero miremos qué lo compone: 

#### Variables globales

```js
var c;
var lineModuleSize = 0; //El largo de la línea
var angle = 0;
var angleSpeed = 1; // la velocidad con la que el ángulo de las líneas cambia
var lineModule = [];
var lineModuleIndex = 0;

var clickPosX = 0;
var clickPosY = 0;
```
Las variables globales van a ser accesibles desde todos los métodos del código. C es una variable asociada con el color de las barras dibujadas. LineModuleSize es el 
tamaño de dichas líneas y el ángulo es en el cual se dibuja esa línea con respecto a la posiciópn del mouse. angleSpeed es la velocidad con la que varía dicho ángulo entre 
línea y línea. LineModule es un arreglo donde se ubican distintas imágenes, que se corresponden a otros tipos estilizados de línea. lineModuleIndex es el índice de dicho arreglo, 
el que nos dice en qué parte de él estamos. Por último, clickPos X y Y que son variables posicionales en función de donde se ubique el mouse para empezar a dibujar las líneas. 

#### Función Preload

```js
function preload() {
  lineModule[1] = loadImage('data/02.svg');
  lineModule[2] = loadImage('data/03.svg');
  lineModule[3] = loadImage('data/04.svg');
  lineModule[4] = loadImage('data/05.svg');
}
```

El preload es una función esencial que se usa cuando uno va a cargar recursos externos a un sketch. Antes de que se ejecute, carga en el mismo imágenes, videos, audios o
demás elementos que, de lo contrario, su carga podría intervenir con el desarrollo del código. Aquí se está llenando el arreglo lineModule con imágenes que serán las que 
estilizen la línea y tengamos variedad de formas. 

#### Función SetUp

```js
function setup() {
  createCanvas(windowWidth, windowHeight);
  background(255);
  //cursor(CROSS);
  noCursor(); //Esta Línea de código Oculta el mouse
  strokeWeight(0.75); //Esto es el grosor de las barras

  c = color(181, 157, 0); //Y esto, el color de las barras
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight); //Reescala la pantalla para tener siempre un tamaño acorde con los dispositivos
}
```

Aquí en el setup se toman varias decisiones de diseño visual: El grosor de las líneas, que será tomado como una constante, la variable c, asociada con el color, toma un valor finalmente, 
el noCursor() oculta el mouse en pantalla y el windowResized escala la pantalla acorde al dispositivo. 

### Segunda Parte: La Infamee Función Draw

Esta función, personalmente, es la que más me costó entender, pero es la que hace que las cosas pasen en el lienzo: 

Básicamente, mientras el mouse esté siendo presionado, el centro desde donde se crean las líneas, asume las coordenadas del mouse. Además, si este se mueve mientras shift esté seindo
presionado, va a moverse perfectamente horizontal o vertical, pues los pequeños cambios que se generen serán corregidos por la función de valor absoluto (abs()). Push() guarda el 
estado actual, tranlate mueve el centro a la posición del mosue, tint pinta las líneas con c, el color previamente asignado, se crea una línea con los valores de tamaño 
preestablecidos en el set up y si el indes del arreglo que guarda los diseños para las líneas es 0, entonces simplemente se dibuja una línea recta. angle += angleSpeed 
hace que cada línea se dibuje en un ángulo distinto a la anterior (pues angle irá cambiando cada vez) y Pop() restauraría el sistema coordenado original.

```js
function draw() {
  if (mouseIsPressed && mouseButton == LEFT) {
    var x = mouseX;
    var y = mouseY;
    if (keyIsPressed && keyCode == SHIFT) {
      if (abs(clickPosX - x) > abs(clickPosY - y)) {
        y = clickPosY;
      } else {
        x = clickPosX;
      }
    }

    push();
    translate(x, y);
    rotate(radians(angle));
    if (lineModuleIndex != 0) {
      tint(c);
      image(lineModule[lineModuleIndex], 0, 0, lineModuleSize, lineModuleSize);
    } else {
      stroke(c);
      line(0, 0, lineModuleSize, lineModuleSize);
    }
    angle += angleSpeed;
    pop();
  }
}
```

### Tercera Parte: Demás Imputs

#### Cuando el mouse cambia de posición

```js
function mousePressed() {
  // create a new random color and line length
  lineModuleSize = random(50, 160);

  // remember click position
  clickPosX = mouseX;
  clickPosY = mouseY;
}
```
Cuando se levanta el mouse y se vuelve a oprimir, los valores de tamaño que tenían las líneas antes, varían completamente, haciendo que cada que vuuelvas a hacer click tengas
líneas de tamaños variados. Además, el centro de donde se originan las líneas cambia a la nueva posición del mouse. Sino, se seguirían generado en donde estaba el mouse antes. 

#### Cambios en la velocidad y el tamaño

```js
function keyPressed() {
  if (keyCode == UP_ARROW) lineModuleSize += 5;
  if (keyCode == DOWN_ARROW) lineModuleSize -= 5;
  if (keyCode == LEFT_ARROW) angleSpeed -= 0.5;
  if (keyCode == RIGHT_ARROW) angleSpeed += 0.5;
  
}
```

Con las flechas de arriba o abajo, el usuario puede cambiar el tamaño de las líneas a placer. Con las flechas de adelante y atrásc, el usuario puede cambiar la velocidad
angular, hachiéndola más rápida o más lenta, y, por tanto, los saltos entre las líneas serán mayores o menores, respectivamente. 

#### Salvar o Borrar Imagen

```js
function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
  if (keyCode == DELETE || keyCode == BACKSPACE) background(255);

  // reverse direction and mirror angle
  if (key == 'd' || key == 'D') {
    angle += 180;
    angleSpeed *= -1;
  }
```
Aquí hay varias cosas que se pueden hacer pero solo DESPUÉS de que el usuario presiona y SUELTA la tecla (Por ello el keyReleased. Con S, salva la imagen que esté en pantalla, 
con DELETE borra lo que haya hecho (se resetea el fondo) Y con d hace algo muy peculiar: Cambia al velocidad del angleSpeed, haciendo que ahor alas líneas giren en sentido opuesto 
a como lo hacían (Recordemos de la velocidad no es solo magnitud sino también dirección) y al ángulo en el que estuvieran las líneas les suma 180 para que ahora se empiecen a generar en
el lado opuesto de la "circunferencia". 

#### Cambios en el color y la forma

```js
  // change color
  if (key == '0') c = color(random(255), random(255), random(255), random(80, 100));
  // default colors from 1 to 4
  if (key == '1') c = color(181, 157, 0);
  if (key == '2') c = color(0, 130, 164);
  if (key == '3') c = color(87, 35, 129);
  if (key == '4') c = color(197, 0, 123);

  // load svg for line module
  if (key == '5') lineModuleIndex = 0;
  if (key == '6') lineModuleIndex = 1;
  if (key == '7') lineModuleIndex = 2;
  if (key == '8') lineModuleIndex = 3;
  if (key == '9') lineModuleIndex = 4;
}
```
Aquí la cosa es sencilla. Si el usuario oprime 0, las líneas se van a colorear de forma aleatoria, mientrass que con 1, 2, 3, y 4, lo harán con colores predefinidos. 
Si oprimen 5, 6, 7, 8, o 9, podrán variar el diseño de las líneas con las imágenes previamente cargadas. Aquí finalmente lineModuleIndex es usada, pues hace que la posición
en el array de imágenes cambie según el botón que escoje el usuario. 

### Anexo: ¿Cómo resolví qué hacía cada cosa?

Admito que hubo uso de chatGPT para descifrar la función Draw(), pero la gran mayoría del código la fui descubriendo a medida que hacía pequeños testeos como los siguientes: 

* Hacer seguimiento a las variables a lo largo del código (cuando se modificaba su valor, qué tipo de valor contenían, qué afectaba su valor y ese valor qué representaba o hacía a lo largo del código).
* Hacer pruebas de los inputs, pues estos indicaban que al oprimir un botón pasaba un algo, y en función de lo que viera en pantalla, podía asociarlo con lo que había escrito.
* Cambiar el valor de las variables para saber exactamente que estaba siendo afectado de forma visual.
* Conocimientos previos de P5.js, que tengo desde el curso de Simulación de Sistemas Físicos.

