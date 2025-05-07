### El Código: 

El código que voy a mostrar es de mi autoría, hecho en simulación de sistemas físicos interactivos. Este consiste en una araña que suelta brillitos por su boca mientras pende de su hilo de telaraña.

``` js
let count = 0;
let increment = 0.01;
let stars = [];
let pendulum;
let mouse = false;
let wind;
let aPressed = false;
let shot;
let wPressed  = false;
let gravity;
let count2 = 0;
let increment2 = 0.01;
let emitter;
let windTime = 0;
let imageTexture;
let imageSpider;
let windSound;
let nightSound;

function preload()
{
  imageTexture= loadImage('https://archive.org/download/particula_202409/Part%C3%ADcula.png');
  imageSpider = loadImage('https://archive.org/download/particula_202409/Ara%C3%B1a.png');
  nightSound = loadSound('https://archive.org/download/sonidos-del-bosque-de-noche/Sonidos%20Del%20Bosque%20De%20Noche.mp3');
  windSound = loadSound('https://archive.org/download/wind-2/Wind%282%29.wav');
}

function setup() {
  createCanvas(600, 460);
  nightSound.loop();
  pendulum = new Pendulum(width/2, 0, 250);
  emitter = new Emitter(pendulum.bob.x, pendulum.bob.y);  // Inicializa el emitter en la posición del bob
  for (let i = 0; i <= 30; i += 1) {
    stars[i] = new Star();
  }
  wind = createVector(0.02, 0);
  gravity = createVector(0, 0.05);
}

function draw() {
  background(0,0,20);
  
  for (let i = 0; i <= 30; i += 1) {
    stars[i].show();
    stars[i].update();
  }
  pendulum.update();
  pendulum.show();
  emitter.run();  // Ejecuta el emitter
  
  
  
   if (windTime > 0) {
    windTime--; // Reducimos el contador
  }
}

class Star {
  constructor() {
    this.position = createVector(random(0, width), random(0, height));
    this.radius = random(8, 16);
  }
  
  show() {
    this.alfa1 = createVector(180, 0);
    this.alfa2 = createVector(250, 0);
    this.alfa = p5.Vector.lerp(this.alfa1, this.alfa2, count);
  }
  
  update() {
    fill(this.alfa.x);
    circle(this.position.x, this.position.y, this.radius);
    count += increment;
    if (count >= 1 || count <= 0) {
      increment *= -1;
    }
  }
}

class Pendulum {
  constructor(x, y, r) {
    this.pivot = createVector(x, y);
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;
    this.angleVelocity = 0;
    this.angleAcceleration = 0;
    this.damping = 0.99;
    this.ballr = 24;
  }

  update() {
    if (!mouse) {
      let gravity = 0.4;
      this.angleAcceleration = (-1 * gravity / this.r) * sin(this.angle);
      this.angleVelocity += this.angleAcceleration;
      this.angleVelocity *= this.damping;
      this.angle += this.angleVelocity;
    } else {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(diff.x, diff.y) + PI;
    }

    // Actualiza la posición del emitter con la posición del bob
    emitter.origin.set(this.bob.x, this.bob.y);
  }

  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle));
    this.bob.add(this.pivot);
    
    stroke(250);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);
    
     image(imageSpider, this.bob.x-60, this.bob.y - 110, this.ballr * 5, this.ballr * 5);
  }
  applyForce(force)
  {
    if (force.mag()>=0.1){
      this.angleAcceleration += force.mag()/20;
      this.angleVelocity += this.angleAcceleration;
      this.angleVelocity *= this.damping;
      this.angle += this.angleVelocity;
    }
  }
}

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    this.addParticle();
    
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let particle = this.particles[i];
      particle.update();
      particle.show();
      
      if (wPressed) {
         if (windTime > 0) {
        particle.applyForce(wind);
        }
      }
      
      if (aPressed) {
        for (let i = 1; i >= 0; i -= 0.005) {
          shot = createVector(i, 0);
          particle.applyForce(shot);
        }
        aPressed = false;
      }
      
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

class Particle {
  constructor(x, y) {
    let v1 = createVector(-1, +1);
    let v2 = createVector(1, +4);
    this.position = createVector(x, y);
    this.velocity = p5.Vector.lerp(v1, v2, count2);
    this.acceleration = createVector(0, 0);
    this.lifespan = 250;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2.0;
    this.acceleration.mult(0);
    count2 += increment2;
    if (count2 >= 1 || count2 <= 0) {
      increment2 *= -1;
    }
  }

  show() {
    image(imageTexture, this.position.x - 8, this.position.y - 8, 10, 10);
    
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

function keyPressed() {
  if (key === 'w') {
    wPressed = true;
    windTime = 300;
    pendulum.applyForce(wind);
    windSound.play();
  }
  if (key === 'a') {
    aPressed = true;
  }
  if (keyCode === UP_ARROW){
    wind.x = wind.x + 0.02;
    print (wind.x);
    
  }
  if (keyCode === DOWN_ARROW){
    wind.x = wind.x - 0.02;
    print (wind.x);
  }
}

function mousePressed() {
  mouse = true;
}

function mouseReleased() {
  mouse = false;
}

```

### El INPUT y el OUTPUT

El INPUT del usuario, los datos que este provee al sistema, vienen de apretar teclas. Algo que no comenté es que las arañas y las partículas pueden ser afectadas por VIENTO, generando así reslutados ditintos cada vez que se les aplica.
El viento es activado por la tecla w, así que el usuario sólo debe presionarla para hacerle saber al sistema que su OUTPUT es la misma araña y partículas pero barridas, esta vez, por el viento. 

### Más Interactividad

La intensidad del viento también puede ser variada por el usuario, y, aunque no es un cambio visual, me interesa recalcarla por que es una forma del usuario de conducir el código. 
Puedes afectar los resultados de la simulación aumentando con la flecha arriba o flecha abajo el arrastre del viento. Si este supera el valor de 1, no solo afectará a las partículas, sino a la araña, que empezará a alejarse más del centro de la pantalla. 
Otra cosa que el usuario puede hacer, que si devuelve algo visual, es arrastrar la araña con el mouse para que haga un movimiento de péndulo. La posicón del cursor son instrucciones para que el programa determine la posición de la araña. Luego, es cuestión de aplicar físicas. 

