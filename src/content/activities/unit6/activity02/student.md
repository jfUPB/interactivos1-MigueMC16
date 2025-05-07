### Notas

#### ¿Qué pasaría si mi rampa de acceso a internet se corta?

No podría acceder a servidores y demás sitios de comunicación de dispositivos, al menos no de forma directa y ya verías si también inalámbrica. 

#### Ejemplos de la vida cotidiana

Si entro a una farmacia y pido unos medicamentos, estoy haciendo una solicitud a alguien que me sirve, un "servidor". Hay información que el servidor me puede dar gratis, como qué medicamentos hay, el precio que tienen y demás, pero por otras cosas, como el medicamento en sí, tengo que pagar si quiero dejar la farmacia con él. Así mismo pasa en lo digital, hay información dentro de un servidor a la que se puede acceder libremente y otra por la que, o bien hay que pagar, o bien es ilegal acceder a ella. Si me robo algo de una farmacia, estoy cometiendo un crimen, igual que si robo información no autorizada de un servidor. 

#### URL de mi página favorita

Una página que me gusta visitar es la de ligas de futbo fantasy Biwenger. La dirección es https://biwenger.as.com/team. Así puedo ver el equipo que tengo puesto para cada jornada. Si no pusiera la ruta específica, me manda simplemente al menú principal, desde donde puedo acceder a la página de forma específica con un desplegable de opciones.

### Protocolo HTTP en comparación a los seriales: 

#### Similitudes

* Existe un método específico de empaquetado de datos.
* Existe una comprobación del estado de los datos.
* Existe un método específico de lectura de datos.

#### Diferencias

* En la comunicación serial hay uno que envía y otro que lee si se enviaron los datos de forma correcta. Aquí hay que pedir permisos al que va a enviar, pues no entrega información a la ligera.
* Los datos recibidos son archivos con extensiones en vez de datos crudos, pues la información es mucho más compleja.
* Se debe indicar el origen o la fuente del solicitante, hay más pasos de seguridad. 

HTTP es más complejo por las comprobaciones a seguir y por la masa de datos más grande que se maneja. 


#### Ejemplo Formulario de Login

* El HTML definiría la ubicación de los campos Usuario, contraseña, registro, encabezado de página, pie de página, lenguaje de la página, ubicación del logo de la página, etc.

* El CSS, una vez definido eso, Le pone contornos a los campos de texto, le pone imágenes al logo de la página, al encabezado, le pone un diseño al fondo de la página, etc. 
 
* JavaScript verificaría si el usuario y la contraseña que el usuario ingresa son correctas, si se hundió un botón para volver, si se presionó el típico "¿no se ha registrado aún? cree una cuenta" o el "¿olvidó su contraseña?"

#### Ventajas del modelo basado en eventos

Como no sería eficiente tener un draw que me dibuje la página varias veces, es mucho mejor dibujar una vez la página y volverla a dibujar o dibujar otra sólo si el usuario ha hecho una acción para que pase ello. Un código que responda a eventos permite al usuario decidir qué hacer y no atenerse a donde esté la ejecución del código en ese momento, dándole más libertad y mayor tiempo de respuesta por parte de la página. 

#### ¿Por que es conveniente usar javascrip?

Por que si el usuario ya usa una conexión en JavaScript, respondiendo con el mismo lenguaje nos permite ahorrarnos traducciones que demorarían un poco más de tiempo la conexión, Además que JavaScript cuenta con más bibliotecas visuales que otros lenguajes. 

#### Diferencias clave entre HTTP y Socket.IO

La HTTP te envía  una gran masa de datos, por eso hay que esperarla y pedir permiso, pero socket.IO es muy rápida por que, entre otras cosas, los datos que envía son más pequeños. Imagina entrar a Google maps: Para entrar primero es una petición HTTP por que Google maps te tiene que pasar no sólo las páginas que hay en su interfaz junto con botones y menús desplegables, sino un mapa enormecon un montón de datos contenidos en él. Pero si queremos iniciar una ruta y ver nuestra posición en tiempo real, ahí es cuando estamos comunicándonos con un método mucho más rápido como Socket.IO. 
