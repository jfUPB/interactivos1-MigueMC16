### Código Inicial

 ` ` ` py
from microbit import *
import utime
import music

STATE_CONFIG = 0
STATE_COUNT = 1

COUNT_INTERVAL = 1000

current_state = STATE_CONFIG
countdown = 20000

while True:
    # pseudoestado STATE_INIT
    if current_state == STATE_CONFIG and current_state != STATE_COUNT:
        if button_a.was_pressed():
         countdown += 1000
         display.show(countdown/1000)
        elif button_b.was_pressed():
         countdown -= 1000
         display.show(countdown/1000)
        elif pin_logo.is_touched():
            current_state == STATE_COUNT
        else:
         display.show(countdown/1000)
    if current_state == STATE_COUNT and current_state != STATE_CONFIG:
        if pin_logo.is_touched():
            current_state=STATE_CONFIG
        elif countdown >0:
          for i in range (20000):
            display.show(countdown/1000)
            countdown -= 1000
            sleep (1000)
            i += 1000
        elif countdown == 0:
            music.play(music.FUNK, wait=False, loop=False)
 ` ` `
     
