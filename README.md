
This project allows for remote power control of an [Optoma UHD35](https://www.optomaeurope.com/product-details/uhd35) projector (and possibly other models) via [ESPHome](https://esphome.io/) and [Home Assistant](https://www.home-assistant.io/). 

### Problem
The Optoma UHD35 has no support for HDMI-CEC, so the only option to start it is either to configure it to auto start when plugged in or using the remote. The auto start could be solved with a smart plug, but complicates shutdown as simply cutting power is not ideal. 

### Solution
The projector has an RS232 port on the back allowing for full control. We can use that along with a microcontroller to remotely control the projector. 

### Bill of materials
 - ESP32 microcontroller (or similar)
 - [RS232 level shifter](https://www.sparkfun.com/products/449), I used one from SparkFun because it was readily available. Anything equivalent should work. 

### Connection
Wire up the RX/TX, VCC and GND connections between the adapter and your microcontroller. You may need to swap RX/TX to make things work. 
Annoyingly the projector does not provide power out when off/standby, so a separate power supply is required for the microcontroller. 

### Setup
Copy the two files, esphome-projector.yaml and uart_read_line_sensor.h into your esphome directory. [Studio Code](https://github.com/hassio-addons/addon-vscode) is helpful to get that header file up. 

Adjust the yaml as needed, primarily the GPIO pins to match the RX/TX pins on your particular microcontroller. The board type may need to be adjusted as well. 

⚠️ It is important to keep the `baud_rate: 0` setting on the logger. If removed, the log will be sent out via serial and this will confuse the projector mightily. 

### Further notes
This project is loosely based on a project by [rasclatt](https://github.com/rasclatt-dot-com/ESPHome-Optoma-Projector-Serial-To-MQTT-bridge), the primary improvement is that it no longer needs to continuously poll the projector for state information, but rather this is read out as the projector announces it. 

### Possible improvements
It would be easy to implement further commands to the projector like reading out lamp hours, input names and such. I personally have no need for this, but forks/pull requests are always appreciated. 