### Vectores de Prueba

* En config Presionaré demasiadas veces A. El resultado esperado es que llegue a 60 y no aumente.
* En config Presionaré demasiadas veces B. El resultado esperado es que llegue a 0 y no aumente.
* De config presionaré A 3 veces y luego T para ir a Count, para presionar S y que me lleve a Init para luego pasar en Config. El contador debería resetearse y volver a 20.
* De config iré a count, ingresaré cuantas contraseñas erradas posibles. El resultado debería ser entrar en loose state.
* De config iré a count, ingresaré La contraseña correcta y después spamearé los botones A y B. Luego de un rato, S El resultado debería ser entrar en Win state y, después de S, ir a init.
* De config iré a count, no haré nada y justo antes de que acabe la cuenta regresiva, oprimiré S. El resultado debería ser ir a init sin pasar por loose. 

### Vectores que Pasaron

Pasaron la pruevba los vectores 1, 2, 3, 4, y 6. 

### Vectores que Fallaron

El vector que no funcionó es el 5. No estoy logrando que se ingrese la contraseña correcta, que se realicen las comprobaciones. Tampoco estoy viendo donde está la falla en el código. 
