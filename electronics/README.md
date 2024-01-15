# Tinycar PCB

For all the electronic need a custom PCB for the Tinycar is designed.
Main features are:
- H-Bridge IC for the DC motor control
- LiPo Battery Charger IC with a custom load sharing mod
- battery voltage measurement
- UART interface (Flash and Serial monitor)
- USB interface (Flash and Serial monitor)
- PWM Servo control for the steering
- OV2640 camera interface
- Output for LEDs (2x headlight, 2x brake light, 4x turn signal)
- 1x GPIO for custom use

## Things to know
- The voltage for the motor and servo comes from the VBat line (not the regulated 3.3 V line) to allow higher voltage (max 4.2 V for a LiPo cell) and therefore higher speed.
- The UART interface is not necessary since USB is available. But it can be very useful for debugging and flashing the ESP32 in case USB for whatever reason does not work.
- BAT_ADC is connected to the wrong pin on the ESP32. It should be connected to GPIO 1 instead of GPIO 21, which does not have an ADC. So one of the custom use GPIOs can be used for the battery voltage measurement if a cable is soldered between these two pins.
- The cap for the BAD_ADC_EN seems to be too high?? It takes a way longer time until the ADC is ready to be read. Maybe it is better to remove the cap. The current version of the firmware just puts HIGH on this pin forever.

