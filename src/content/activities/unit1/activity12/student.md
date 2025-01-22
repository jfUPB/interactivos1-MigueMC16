let port;
let connectBtn;

function setup() {
    createCanvas(400, 400);
    background(220);
    port = createSerial();
    connectBtn = createButton('Connect to micro:bit');
    connectBtn.position(80, 300);
    connectBtn.mousePressed(connectBtnClick);
  
    let sendBtn1 = createButton('Send Love');
    sendBtn1.position(220, 300);
    sendBtn1.mousePressed(sendBtn1Click);
  
    let sendBtn2 = createButton('Send Good Night');
    sendBtn2.position(220, 250);
    sendBtn2.mousePressed(sendBtn2Click);
  
    let sendBtn3 = createButton('Send Good Feelings');
    sendBtn3.position(220, 200);
    sendBtn3.mousePressed(sendBtn3Click);
  
    let sendBtn4 = createButton('Send Math Text');
    sendBtn4.position(220, 150);
    sendBtn4.mousePressed(sendBtn4Click);
}

function draw() {
  background(220);
        
}

function connectBtnClick() {
    if (!port.opened()) {
        port.open('MicroPython', 115200);
    } else {
        port.close();
    }
}

function sendBtn1Click() {
    port.write('h');
}
function sendBtn2Click() {
    port.write('j');
}
function sendBtn3Click() {
    port.write('k');
}
function sendBtn4Click() {
    port.write('l');
}
