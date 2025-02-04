### ¿Cómo el código logra la concurrencia?

Los condicionales if y else son como semáforos, que indican por que bifurcación debe correr el código. Es muy importante implementarlos si se quiere tener concurrencia.
Pero también se necesita llamar a cada estado y limpiar el anterior una vez se recae en este. Es importante entonces, definir un flujo adecuado del programa para que, 
si la idea es acceder a un estado, no se quede esperando a que otro termine para acceder o directamente no acceda. Como detalle final, un while que siempre esté en true 
garantiza que el cógigo sea cíclico y que nunca salga de la máquina de estados (si esa es la intención del sieño).

### Vectores de Prueba

* Dejar correr durante tres estados ➡️ Oprimir A ➡️ Oprimir A ➡️ Dejar correr durante un estado.
  Resultados Esperados: Happy, Smile, Sad, Smile, Happy, Smile.
* Dejar correr durante dos estados ➡️ Oprimir A ➡️ Oprimir A ➡️ Oprimir A ➡️ Oprimir A 
  Resultados Esperados: Happy, Smile, Happy, Sad, Smile, Happy.
* Dejar correr durante dos estados ➡️ Oprimir A ➡️ dejar correr durante tres estados ➡️ Oprimir A
  Resultados Esperados: Happy, Smile, Happy, Smile, Sad, Happy, Sad.

### Resultados Obtenidos

* Primer vector: Pasa la prueba. ✔️
* Segundo vector: Pasa la prueba. ✔️
* Tercer vector: Pasa la prueba. ✔️

El código es robusto y el flujo de estados está bien implementado.

