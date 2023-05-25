* Portada
 ## CODIGO
 ```python
 from machine import Pin, ADC, PWM
import utime
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C
from time import sleep_ms
import utime as time
from dht import DHT11, InvalidChecksum



#oled cordanadas de tamaño
ANCHO = 128
ALTO = 64


xAxis = ADC(Pin(27))
yAxis = ADC(Pin(26))
button = Pin(17,Pin.IN, Pin.PULL_UP)

led_left = Pin(14, Pin.OUT)
led_middle = Pin(15, Pin.OUT)
led_right = Pin(12, Pin.OUT)
led_up = Pin(16, Pin.OUT)
led_down = Pin(13, Pin.OUT)

i2c = I2C(1,scl = Pin(19), sda = Pin(18))
oled = SSD1306_I2C(ANCHO, ALTO, i2c)

buzzer = PWM(Pin(6))
buzzer.freq(500)

while True:
    oled.fill(0)

    xValue = xAxis.read_u16()
    yValue = yAxis.read_u16()
    buttonValue = button.value()
    xStatus = "middle"
    yStatus = "middle"
    buttonStatus = "not pressed"

    led_left.value(0)
    led_down.value(0)
    led_up.value(0)
    led_right.value(0)
    led_middle.value(1)

                  #Sensor temperatua


# Check the x and y Value to determine the status of the joystick
    if buttonValue == 0:
        buttonStatus = "pressed"
        led_middle.value(0)
        oled.text("presionado",0,30)
        buzzer.duty_u16(0)

    if xValue <= 600:
        xStatus = "left"
        led_left.value(1)
        led_middle.value(0)
        oled.text("Abajo",5,30)
        buzzer.duty_u16(0)
        

    if xValue >= 60000:
        xStatus = "right"
        led_right.value(1)
        led_middle.value(0)
        oled.text("Arriba",5,10)
        


    if yValue <= 600:
        yStatus = "up"
        led_up.value(1)
        led_middle.value(0)
        oled.text("Izquierda",5,30)

    if yValue >= 60000:
        yStatus = "down"
        led_down.value(1)
        led_middle.value(0)
        oled.text("Derecha",5,30)

    oled.show()

    print("X: " + xStatus + ", Y: " + yStatus + "  button status: " + buttonStatus)
    utime.sleep(0.2)
 ```
