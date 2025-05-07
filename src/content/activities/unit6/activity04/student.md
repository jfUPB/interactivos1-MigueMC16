### NOTAS

#### Experimento 1

Si detengo el servidor, sale un mensaje indicando que no se puede acceder al sitio web y que localhost ha rechazado la conexión. Siguiendo con la metáfora de la farmacia, ¿cómo puedo tomar uno de sus productos si acaban de cerrar el establecimiento?. Cuando oprimo de nuevo npm start, la página vuelve a estar disponible por que el servidor está de nuevo abierto.

#### Experimento 2

Página 1 no fue capaz de funcionar correctamente por que no tenía la información inicial de la página 2. Es útil enviar el estado inicial de cada página al conectarse para que ambas partan desde determinado punto y no tengan que esperar a que la otra se mueva para finalmente recibir datos. 

#### Experimento 3

Se dispara el log y muestra los datos de page1, pues lo que está esperando la página dos es recogerlos diréctamente del servidor. Precisamente como page1 es la que envía el paquete de datos getdata y page2 solo lo recibe, no se dispara ningún log cuando muevo page2

#### Experimento 4

#### Experimento 5
