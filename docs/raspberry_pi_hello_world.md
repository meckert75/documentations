# Raspberry Pi Hello World

Now that the Raspberry Pi is [ready and booted up](raspberry_pi_setup.md) the next step is to start controlling something.
The simplest would be a single LED but instead these instructions will skip to an [RGB LED](https://smile.amazon.com/gp/product/B01C19ENDM).

## Basic LED Diagram

![LED Schema](images/basic_led_diagram.svg)

When hooking up the LED to the Raspberry Pi, the GPIO pins will be used and will act as the power source when pulled high (turned on).

The resistor is connected into the circuit to control the current that flows from the Pi to the LED.

The LED (Light Emitting Diode) is as its name says a diode which allows current to flow in one direction only, so if the plus and minus are connected the wrong way, the LED won't light up.

A RGB LED is essentially three LEDs in a single package and as such has three connectros for each color and a shared ground.
Each of the colors will need a resistor.

![RGB LED Schema](images/rgb_led_diagram.svg)

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



# References

* [RGB LED Datasheet](https://www.sparkfun.com/datasheets/Components/YSL-R596CR3G4B5C-C10.pdf)
