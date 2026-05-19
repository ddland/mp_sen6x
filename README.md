# Sensirion SEN6x
Micropython driver for the sensirion SEN63-C and SEN66 sensing platforms.

## Installation
Copy the sen6x.py to the library folder on the raspberrypi-pico (or just in the root of the device).

Connect the SEN66 to a I2C bus on the raspberrypi-pico and power it with the 3.3V source.
Once connected and started the `get_data()` command will start a new measurement and return the values. If there has been enough time (somewhere between one and 2 days) since the last fan-cleaning routine, the fan fill run 10 seconds at high power to flush some air trough the system.

When using the SEN63-C for NOx or VOC measurements the sensor will return -1. These values are not measured by the sensor.

## Example code
```python
import machine
import time

from sen6x import SEN6X
i2c0 = machine.I2C(0, sda=machine.Pin(0), scl=machine.Pin(1), freq=100000)
sen = SEN6X(i2c0)
sen.start()
time.sleep(1)
print(sen.data_header())
for ii in range(5):
    print(sen.get_data()) 
    time.sleep(1)
```

If the watch-dog timer is used, a WDT object can be passed to the SEN6x initializing function and every second orso the watchdog is fed with an update.
