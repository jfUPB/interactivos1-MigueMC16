### Código:

```js
let startAngle = 0;
let startAngle2 = 180;
let deltaAngle = 0.2;
let color1 = 100;
let color2 = color1 + 100;
let constantAngle = 0.02;

let colorsSin = []; // Colores para la mitad de la pantalla con seno
let colorsCos = []; // Colores para la mitad de la pantalla con coseno

function setup() {
  createCanvas(640, 240);

  // Inicializamos los colores para cada círculo
  for (let x = 0; x < width / 2; x += 24) {
    colorsSin.push([random(color1, color2), random(color1, color2)]);
  }
  for (let x = width / 2; x <= width; x += 24) {
    colorsCos.push([random(color1, color2), random(color1, color2)]);
  }
}

function draw() {
  background(255);

  let angle = startAngle;

  // Primera mitad de la pantalla (usando seno)
  for (let i = 0, x = 0; x < width / 2; x += 24, i++) {
    let y = map(sin(angle), -1, 1, 0, height);
    stroke(0);
    fill(colorsSin[i][0], colorsSin[i][1], 200);
    circle(x, y, 48);

    angle += deltaAngle; // Cambia el ángulo de inicio de cada círculo

    // Si la simulación está activa, actualizamos los colores
    if (constantAngle !== 0) {
      colorsSin[i] = [random(color1, color2), random(color1, color2)];
    }
  }

  // Segunda mitad de la pantalla (usando coseno)
  angle = startAngle2; // Resetear el ángulo para la segunda mitad
  for (let i = 0, x = width / 2; x <= width; x += 24, i++) {
    let y = map(cos(angle), -1, 1, 0, height);
    stroke(0);
    fill(colorsCos[i][0], 200, colorsCos[i][1]);
    circle(x, y, 48);

    angle += deltaAngle;

    // Si la simulación está activa, actualizamos los colores
    if (constantAngle !== 0) {
      colorsCos[i] = [random(color1, color2), random(color1, color2)];
    }
  }

  // Actualizamos los ángulos solo si la simulación está activa
  if (constantAngle !== 0) {
    startAngle += constantAngle;
    startAngle2 += constantAngle;
  }
}

function keyPressed() {
  if (key === 'c') {
    color1 = color1 + 50;
  }
  if (key === 'x') {
    color1 = color1 - 50;
  }
  if (key === 'm') {
    constantAngle = 0; // Pausa la simulación
  }
  if (key === 'n') {
    constantAngle = 0.02; // Reanuda la simulación
  }
}

```

### Explicación del Cógido: 

Este código realiza un mapeo con seno y conseno. Dibuja círculos ligeramente separados entres sí que se mueven de arriba a abajo siguendo los valores que devuelven las funciones trigonométricas. 
Como son cíclicas, los círculos van de arriba a abajo. Los círculos inician en ángulos distintos para dar ese efecto de onda. La mitad de la pantalla usa seno como mapeo y la otra mitad, coseno. 
La aleatoriedad y los parámetros modificables vienen con los colores de los círculos. Funciona asignando a los círculos colores de acuerdo a valores RGB, dejando dos de tres de estos parámetros depender de la función random. 
La función random oscila entre color1 y color2, que distan 100 unidades entre sí. Se almacenan los valores de color únicos de cada color en una matriz. 

### Interactividad: 

El usuario puede modificar los resultados del código de la siguiente forma: 

* Oprimiendo 'x': Cambia los valores de color 1 y color dos restando 50, haciendo que el número random cambie sus límites de generación.
* Oprimiendo 'c': Cambia los valores de color 1 y color dos sumando 50, haciendo que el número random cambie sus límites de generación.
* Oprimiendo 'm': Detiene la generación, haciendo que se almacenen los colores random y las pocisiones de los círculos.
* Oprimiendo 'n': Deja proseguir la simulación, generando un nuevo patrón.
