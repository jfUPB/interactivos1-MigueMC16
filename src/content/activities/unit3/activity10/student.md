### La versatilidad de las máquinas de estados

Las máquinas de estados han probado ser trasladables de un lenguaje a otro de forma relativamente sencilla, al no usar prácticamente ninguna función específica de un 
programa o un flujo lineal que pueda no funcionar entre lenguajes. Al usar esencialmente algo tan elemental como booleanos y condicionales (cosas que prácticamente todo 
lenguaje tiene) nos aseguramos que algo escrito en, por ejemplo, javascript, pueda replicarse en micropython sin necesidad de importe de librerías externas o un refacturing
extensivo del código. Tanto es así que el diagrama de flujo es el mismo para ambos lenguajes y cualquier otro al cual se quiera trasladar el programa. 

### La importancia de los vectores de prueba y las regresiones

Al ser un método de programación cuyo flujo es cíclico, tenemos que centrarnos en que el ciclo NUNCA se desvíe o nos lleve a estados en donde no queremos estar o, al menos,
en donde no deberíamos de momento. Los vectores de prueba miran los límites del programa y puntos críticos en búsqueda de escapes del bucle no deseados. Hacer regresiones 
implica corregir errores a tiempo 
