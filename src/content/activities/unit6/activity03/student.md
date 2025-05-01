### Notas

#### ¿Por qué es importante usar librerías?

Si escribimos todo el contenido de una librería en un código, primero que nada, el código queda abrumadoramente largo y pesado, en vez de simplemente importar las librerías y usar lo que necesitemos de ellas, y segundo, importar librerías nos ayuda a usar recursos de ellas en otros código de manera mucho más eficiente y sencilla que copiando todo en un sólo código.

#### ¿Qué pasa si cambio el nombre de la carpeta?

Pues que sale el siguiente mensaje: Cannot GET /page1.html. 

Esto se debe a que le estoy diciendo al servidor que busque a page1 en una carpeta que no existe o donde al menos no está page1, con lo que no puedo mostrarla y simplemente devuelve un mensaje de error. 

#### ¿Qué pasa si cambio el request de un navegador?

Naturalmente el navegador tiene que preguntar por algo que exista, pues el propio servidor está esperando que le pregunten por eso. Si cambio el request que espera el servidor, debe haber un cambio en la URL para poder validar la petición, por que, si no, será imposible para el servidor saber lo por que le están preguntando. 

#### ID y Usuarios

Aquí voy a asumir cosas por que no me salió el ID del usuario, simplemente la consola verificó si un usuario había entrado o se había salido, con los mismos mensajes para ambas páginas. Eso me anima a pensar que el ID es el mismo, apoyado también en que estoy accediendo a esas páginas desde una misma dirección IP. 

#### Escuchando Mensajes de Clientes

Se registran los movimientos de las páginas, esto queda en evidencia gracias a los paquetes de datos que se envían constantemente, el paquete Data con la posición x y y de la página y demás,  el ID del cliente y demás. Las actualizaciones fallan una vez que quito el broadcast por que ya no está enviando esos datos constantemente, y por tanto, page1 no se actualiza como debería, o al menos, es mi caso. 

#### Poner en Marcha el Servidor

Por supuesto que si intento entrar por el puerto 3000 al servidor habiéndolo cerrado y abriendo el 3001, no voy a poder. El puerto es como la puerta de una tienda: Si muevo la puerta un poco a la izquierda, luego no voy a poder entrar por donde solía hacerlo por que ahí no está la ubicación del acceso.  
