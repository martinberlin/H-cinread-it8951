# Cinread-IT8951 HAT

Hardware repository for the Cinread.com HAT for 9.7 inch parallel epaper display sold by Good display. 

The aim of this is to provide:

- WiFi
- BLE
- 3V to 5V step-up
- 3.7v LiPo battery charger

## Hardware notices

Cinread early revisions that Goodisplay is selling need a small very tiny bridge in the resistances at the side of SPI. Also the Silkscreen has an error and SI is swapped by CLK.
Please refer to the following image to the correct SPI labels:
![Corrected labels](components/assets/cinread-correct-IO.jpg)

## Ideas that could be added to the schematic

Provided by colleagues [Larry Bank](https://github.com/bitbank2) and [Hideo](https://github.com/lovyan03)

1) Increase the pull-up resistors from 2.2 to 4.7K. Safer if the external I2C device already has its own pullups
2) Add a voltage divider to measure the battery voltage. Something with one end connected to another GPIO to allow the current to be shut off completely when not needed.
3) If you're not using GPIO 39-42, you might want to get a connector to connect a JTAG debugger.

![jtag](components/assets/jtag.png)

1 Is a done deal, will just switch the I2C pullups to 4.7K
2 Is not hard to do I will check how unexpected maker does it and do something similar
3 Is a nice to have feature, but won't add an extra USB for that, will just expose the pins clearly in case anyone wants to connect a JTAG debugger

### Connection IOs used for preliminary tests

On my first tests to implement this using an Espressif ESP32-S3-WROOM-1U-N8R2 module with 2MB PSRAM I used the following GPIOs:

```
CONFIG_EINK_SPI_MOSI=11
CONFIG_EINK_SPI_MISO=12
CONFIG_EINK_SPI_CLK=13
CONFIG_EINK_SPI_CS=10
CONFIG_EINK_BUSY=3
```

Note: Reset PIN is not needed and it's also not in [Cinread PCB sold by Good display](https://www.good-display.com/product/425.html).

Important: On the early revisions of the Cinread PCB SPI pins CLK and SI (Slave in) are swapped. So it should be relabeled or swapped by you when you connect it to ESP32S3, otherwise it will not work. Also MISO pin should be connected and spi_3wire parameter for Lovyan GFX set to false, since it is in fact 4 wire SPI (CLK, MOSI, MISO)

This is using at the testing breadboard Lovyan GFX in their develop branch, since it reads the [mem_address to write the image from SPI](https://github.com/lovyan03/LovyanGFX/issues/242), hence using the master branch of the library it will not work or it will write the image incorrectly on the 9.7" display.

### Hardware requirements

- [DEXA-C097](https://www.good-display.com/product/425.html) PCB sold by Good-Display
- One ED097OC* or **ED097TC2 parallel epaper**, sold by Aliexpress, check more [9.7" details in EPDiy project](https://github.com/vroland/epdiy#join-the-discussion) supported epapers table
- This board that is still on production.

In case you want to test it earlier, please get a ESP32-S3-WROOM-1U-N8R2 and connect the 4 required SPI cables, plus 5V and GND making your own PCB.
