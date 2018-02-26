# Raspberry Pi Hello World

Now that the Raspberry Pi is [ready and booted up](raspberry_pi_setup.md) the next step is to start controlling something.
The simplest would be a single LED but instead these instructions will skip to an [RGB LED](https://smile.amazon.com/gp/product/B01C19ENDM).

## Basic LED Diagram

![LED Schema](images/basic_led_diagram.svg)

When hooking up the LED to the Raspberry Pi, the GPIO pins will be used and will act as the power source when pulled high (turned on).

The resistor is connected into the circuit to control the current that flows from the Pi to the LED.

The LED (Light Emitting Diode) is as its name says a diode which allows current to flow in one direction only, so if the plus and minus are connected the wrong way, the LED won't light up.

A RGB LED is essentially three LEDs in a single package and as such has three connectros, one for each color and a shared ground.

![RGB LED Schema](images/rgb_led_pins.svg)

Each of the colors will need a resistor.

![RGB LED Diagram](images/rgb_led_diagram.svg)

In order to choose the right resistor, the specs for the LED need to be known, meaning the forward voltage and forward current are needed. The [RGB LED](https://smile.amazon.com/gp/product/B01C19ENDM) used in the example here has the specs:

| Color    | Forward Voltage    | Forward Current     |
|----------|--------------------|---------------------|
| Red      | 2 V                | 20 mA               |
| Green    | 3 V                | 20 mA               |
| Blue     | 3 V                | 20 mA               |

The Raspberry Pi GPIO pins, which are the source for the LED, have a voltage of 3.3 V. Taking this data, the formula to compute the needed resitor is as followed.

![LED resistor formula](images/led_resistor_formula.svg)

Using the formula, the resistors values can be calculated. To find the right resistor, use the [resistor color calculator](http://www.resistorguide.com/resistor-color-code-calculator/). The exact resistor might not be readily available in which case picking the nearest larger resistor will still work. For example, if the result of the calculation asks for a 65 Ω resistor, a 68 Ω resistor will do if that's what is available.

| Color   | Resistor   | Resistor Color Code   |
|---------|------------|-----------------------|
| Red     | 65 Ω       | ![65 ohm](images/65_ohm.png)   |
| Green   | 15 Ω       | ![15 ohm](images/15_ohm.png)   |
| Blue    | 15 Ω       | ![15 ohm](images/15_ohm.png)   |

## Connecting it All

Putting it all together, the following diagram shows the RGB LED connected to GPIO 18, 23 and 24 respectively.

![RGB LED connected to Raspberry Pi](images/raspberry_rgp_led_diagram.svg)

## Controlling the LED

### Using the `gpio` Utility

Power up the Raspberry Pi and open a `ssh` connection to the Pi. The simplest way to control the GPIOs is using the `gpio` command.

First set the GPIO mode of the used pins to output (write). The default of the GPIOs is input (read).

```
gpio -g mode 18 out
gpio -g mode 23 out
gpio -g mode 24 out
```

The pins are ready to be written to.

```
gpio -g write 18 1 # turn on red
gpio -g write 18 0 # turn off red
gpio -g write 23 1 # turn on green
gpio -g write 23 0 # turn off green
gpio -g write 24 1 # turn on blue
gpio -g write 18 1 # turn on red (turns LED purple)
```

Check the status of the pins.

```
gpio readall
```

This will print the status of all pins of the Raspberry Pi.

```
 +-----+-----+---------+------+---+-Pi ZeroW-+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 |   IN | 1 |  3 || 4  |   |      | 5v      |     |     |
 |   3 |   9 |   SCL.1 |   IN | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 1 |  7 || 8  | 0 | IN   | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | IN   | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |   IN | 0 | 11 || 12 | 1 | OUT  | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | OUT  | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 1 | OUT  | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK |   IN | 0 | 23 || 24 | 1 | IN   | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 1 | IN   | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |   IN | 1 | 31 || 32 | 0 | IN   | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |   IN | 0 | 35 || 36 | 0 | IN   | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | IN   | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+-Pi ZeroW-+---+------+---------+-----+-----+
```

### Using Python Library

Create a Python script `vim blink_led.py` and paste the below code.

```python
import RPi.GPIO as GPIO
import time

red = 18
green = 23
blue = 24

GPIO.setmode(GPIO.BCM)

GPIO.setup(red, GPIO.OUT)
GPIO.setup(green, GPIO.OUT)
GPIO.setup(blue, GPIO.OUT)

colors = [red, green, blue]
for color in colors:
    GPIO.output(color, GPIO.HIGH)
    time.sleep(1)
    GPIO.output(color, GPIO.LOW)

GPIO.setup(red, GPIO.IN)
GPIO.setup(green, GPIO.IN)
GPIO.setup(blue, GPIO.IN)
```

Run the script.

```
python blink_led.py
```

## End Result

The wired up Raspberry Pi with the RGB LED looks something like the below image.

![Final Raspberry Pi hooked up to RGB LED](images/final_raspberry_pi_rgb_led.jpg)

## Hello Universe

For fun, this secion takes the control of the RGB LED one step further and allows a user to control the LED's brightness and color through a GUI.

Instead of controlling the pins connected to the LED by simply pulling the pins high (on) or low (off), 

# References

* [RGB LED Datasheet](https://www.sparkfun.com/datasheets/Components/YSL-R596CR3G4B5C-C10.pdf)
* [GPIO Utility](http://wiringpi.com/the-gpio-utility/)
* [Raspberry Pi GPIO Python Library](https://pypi.python.org/pypi/RPi.GPIO)
