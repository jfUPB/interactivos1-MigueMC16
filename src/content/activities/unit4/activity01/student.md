### Ejemplo 1:
#### ¿Qué me llamó la atención?

Me encanta el rastro tan lindo que deja el círculo. Leyendo el código, entendí que en la función Draw, por cada frame, se dibuja un círculo, y si la posición de este se actualiza cada frame, se va creando una estela. Hay vectores de posición x y y que se encargan de ubicar al círculo en el espacio y vectores de velocidad que se encargan de cambiar su posición respecto al tiempo. Si uno hace click con el mouse, su la bola acelera tanto en x como en y. Cambia de color pues cada que se presiona el mouse, el hue, o valor del color, aumenta 1. Este tiene un máximo de 360 y, cuando llega a ese máximo, se resetea a 1. Aquí el enclace: https://editor.p5js.org/LaWikipedia/full/xaxriTVf_

![](RastrodeColores.gif)

#### Mis modificaciones al código

```py
if (key === 'w') {
    // Add speed to circle's position to make it move
    circlePositionY = circlePositionY - circleSpeedY;
    print ("w was pressed");
    // Increase hue by 1
    circleHue = circleHue + 1;
  }
  if (key === 's') {
    // Add speed to circle's position to make it move
    circlePositionY = circlePositionY + circleSpeedY;
    print ("s was pressed");
    // Increase hue by 1
    circleHue = circleHue + 1;
  }
  if (key === 'a') {
    // Add speed to circle's position to make it move
    circlePositionX = circlePositionX - circleSpeedX;
    print ("a was pressed");
    // Increase hue by 1
    circleHue = circleHue + 1;
  }
  if (key === 'd') {
    // Add speed to circle's position to make it move
    circlePositionX = circlePositionX + circleSpeedX;
    print ("d was pressed");
    // Increase hue by 1
    circleHue = circleHue + 1;
  }
```

#### ¿Qué cambios hice?

No me gustaban los bordes grises, así que los removí, y cambié la forma en la que la que el círculo es controlado. Ahora con movimientos horizontales y verticales, en vez diagonales, con las teclas w, a, s, y d. Aquí el enlace: https://editor.p5js.org/LaWikipedia/full/bx058uKE-

![](RastrodeColoresPropio.gif)

### Ejemplo 2:
#### ¿Qué me llamó la atención?

A medida que se mueve el mouse ocurren dos cosas y me encanta el efecto visual generado: si mueves el mouse en el eje X, va a aumentar el radio del círculo que se está generando con las líneas rectas (todo visible en le gif de más abajo) y la cantidad de líneas rectas está dada por el movimiento en el eje Y, con una función map, que agarra un valor que está entre un mínimo y un máximo, y devuleve uno proporcional a ese valor entre dos nuevos mínimos y máximos. En este código, la cantidad de líneas se llama circleResolution y su valor esta dado por la función map. El enlace acá: https://editor.p5js.org/LaWikipedia/full/73XrSeEO1

![](LineasRotatorias.gif)

#### Mis modificaciones al código

```py
 for (var i = 0; i <= circleResolution; i++) {
    var hue = map(i, 0, circleResolution, 0, 360); // Va de 0 a 360 según la línea
    stroke(hue, 100, 100); // Saturación y brillo al máximo

    var x = cos(angle * i) * radius;
    var y = sin(angle * i) * radius;
    line(0, 0, x, y);
  }
```

#### ¿Qué cambios hice?

Cada línea ahora va a ser coloreada con un estandar Hue, Saturation, Value, y va a cambiar únicamente el valor del Hue por ccada línea. Va a funcionar como una proporción en el ciclo for: El ciclo for tiene un valor que oscila entre 0 y circleResolution, que es i, y queiro que cada línea se coloree en el orden correcto del espectro de color, haciendo que la última siempre sea 360 (el valor máximo del Hue) y la primera siempre sea 0 (el mínimo del Hue). Así que tengo dos valores qye van a oscilar entre dos topes y quiero que sean proporcionales entre si. Para esto, me sirve la función map. Digamos que circleResolution es 20 y estoy con i = 5. Eso significa que estoy a un cuarto del camino entre 0 y 20. Así mismo debería estar el Hue, a un cuarto de camino entre 0 y 360, osea que el Hue en la recta 5 sería de 9, y así con todas. El enlace: https://editor.p5js.org/LaWikipedia/full/NFdltnmTQ

![](LineasRotatoriasPropio.gif)

### Ejemplo 3:
#### ¿Qué me llamó la atención?

Me gusta el efecto visual generado, y todo funciona con ¡trigonometría! Para que la pupila apunte al mouse hay que hayar el ángulo generado entre estos dos y para ello podemos graficarlos en un plano cartesiando, ver sus coordenadas x y y, armar un triángulo rectángulo con la pupila y el mouse y encontrar los catetos con sus coordenadas para luego usar una fórmula que nos descifra el ángulo usando los catetos, que sería la fórmula de arco tangente. Sé que suena difícil, así que lo grafiqué para que se entienda: 

Aquí la imagen

El enlace: https://editor.p5js.org/LaWikipedia/full/dXJQqiUjU

![](Aim.gif)

#### Mis modificaciones al código

```py
let fly;

function setup() {
  createCanvas(400, 400);
  fly = new Fly();
  // Set angle mode so that atan2() returns angles in degrees
}

function draw() {
  background(0);
  fly.update();
  fly.checkEdges();
  fly.show();
}

class Fly {
  constructor() {
    this.position = createVector(random((10), (width-10)), random(10, height-10));
    this.velocity = createVector(1, 1);
    

  }

  update() {
    this.acceleration = createVector(mouseX/1000, mouseX/1000);
    this.position.add(this.velocity);
    this.velocity.add(this.acceleration);
  }

  show() {
    
    stroke(0);
    strokeWeight(2);
    fill(10, 225, 225);
    ellipse(this.position.x -28, this.position.y-10, 50, 20);
    
    stroke(0);
    strokeWeight(2);
    fill(10, 225, 225);
    ellipse(this.position.x +28, this.position.y-10, 50, 20);
    
    stroke(0);
    strokeWeight(1.5);
    fill(0);
    line(this.position.x-38, this.position.y-10, this.position.x+38, this.position.y-10);
    
    stroke(0);
    strokeWeight(2);
    fill(50, 20, 150);
    circle(this.position.x, this.position.y, 48);
    
    stroke(0);
    strokeWeight(1);
    fill(255);
    circle(this.position.x, this.position.y, 25);
    
    stroke(0);
    strokeWeight(0.5);
    fill(0);
    circle(this.position.x, this.position.y, 15);
    
    stroke(255);
    strokeWeight(0.5);
    fill(255);
    circle(this.position.x-5, this.position.y-5, 10);
    
    stroke(0);
    strokeWeight(1.5);
    fill(250, 100, 150);
    arc(this.position.x, this.position.y+15, 20, 15, (2*PI),PI , CHORD);
    stroke(0);
    strokeWeight(1);
    fill(255);
    arc(this.position.x, this.position.y+15, 20, 7, (2*PI),PI , CHORD);
   
  }

  checkEdges() {
     if (this.position.x > width-50 || this.position.x < 50) {
    this.velocity.x = this.velocity.x * -1;
  }
  if (this.position.y > height-50 || this.position.y < 50) {
    this.velocity.y = this.velocity.y * -1;
  }
  }
}
```

#### ¿Qué cambios hice?

Añadí una clase mosca para que persigan los ojos a la mosca. El movimiento de la mosca depende de donde esté el mouse, es verdad, pero acelera en función a la posición del mouse, no se ubica exactamente donde está él, y así puedo medir qué tan responsivo es el código. Lógicamente, ya el cálculo de la arco tangente no es con los componentes x y y del mouse, sino de la mosca. Cabe resaltar que la clase ya estaba hecha gracias a la clase de Simulación de Sistemas Físicos. 

Aquí el gif

El enlace: https://editor.p5js.org/LaWikipedia/full/IHeEkkDQd

