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

### Código Final

```js
let port;
let connectBtn;
let position = 200;
let circle1;
let increment = 0;

function setup() {
    createCanvas(400, 400);
    background(220);
    circle1 = new Circle(position);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
    let sendBtn = createButton('Send Love');
    sendBtn.position(220, 300);
    sendBtn.mousePressed(sendBtnClick);
}

function draw() {
  background(220);
        circle1.update(increment);
        circle1.show();
       

    if(port.availableBytes() > 0){
        let dataRx = port.read(1);
        if(dataRx == 'A'){
            circle1.update(10);
        }
        else if(dataRx == 'B'){
            circle1.update(-10);
        }
        else{
            circle1.update(0);
        } 
       text(dataRx, width / 2, height / 2);
    }

    if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtnClick() {
    port.write('h');
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
```
