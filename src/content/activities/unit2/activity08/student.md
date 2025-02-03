###  ¿Qué hace el código?

El código instancia dos objetos de la clase píxel y les indica cuando iluminarse y cuando no. Les permite crearse con una posición en x y y, unos ticks para un intérvalo 
de duración de estado y un nivekl de luz inicial. 

#### Estados: 
Hay dos estados principales en el código: Init y WaitTimeout. 
El intérvalo Ini hace que el led se encienda, cambiando a su vez el estado del led a WaitTimeout.
Una vez está en WaitTimeout, debe espera a que el intérvalo se acabe para, combinadas estas dos condiciones, se apague el led.

#### Eventos: 
Mientras el programa está con un led en Init, le solicita el tiempo actual en el que se produce un cambio de estado.
A su vez, mientras el led está en WaitTimeout, el programa pregunta si el intérvalo ya pasó para poder apagar el led, por lo cual, StartTime e Interval son variables por 
las que el código pregunta contínuamente. 

#### Acciones: 
Las acciones principales son, por supuesto, encender y apagar un led dependiendo de su estado y del tiempo trascurrido. Por elloe s importante tener bien definido que 
queremos que pase durante el estado de un objeto; para que el flujo de las acciones sea correcto. Cómo una suerte de semáforo. 
