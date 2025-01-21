### Código en P5.js

```js
let position = 200;
let circle1;
let increment = 0;

function setup() {
  createCanvas(640, 400);
  circle1 = new Circle(position);


}
function draw() {
  background(220);
  circle1.update(increment);
  circle1.show();
}

class Circle
{
  constructor(positionX)
  {
    this.positionX = positionX;

  }
  update(increment){
    this.positionX = this.positionX+increment;
  }
  show()
  {
  stroke(0);
  fill(127);
  circle(this.positionX, 200, 48)
  }
}
function keyPressed(){
  if (key === 'a'){
    circle1.update(10); 
    print('A is pressed');
  }
  if (key === 'b'){
    circle1.update(-10);
    print('B is pressed');
  }
}
```
