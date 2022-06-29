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
3) You might want to get a connector to connect a JTAG debugger.

## ESP32-S3 Pin for JTAG

Referenced from this page in [Espressif documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/jtag-debugging/configure-builtin-jtag.html)

	
```
GPIO19 D-

GPIO20 D+

5V -> V_BUS
```

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

MISO pin should be connected and spi_3wire parameter for Lovyan GFX set to false, since it is in fact 4 wire SPI (CLK, MOSI, MISO)

This is using at the testing breadboard Lovyan GFX in their develop branch, since it reads the [mem_address to write the image from SPI](https://github.com/lovyan03/LovyanGFX/issues/242), hence using the master branch of the library it will not work or it will write the image incorrectly on the 9.7" display.

![PCB front](/components/assets/IT8951-HAT-Front.jpg)
![PCB back](/components/assets/IT8951-HAT-Back.jpg)

### Hardware requirements

- [DEXA-C097](https://www.good-display.com/product/425.html) PCB sold by Good-Display
- One ED097OC* or **ED097TC2 parallel epaper**, sold by Aliexpress, check more [9.7" details in EPDiy project](https://github.com/vroland/epdiy#join-the-discussion) supported epapers table
- This board that is still on production.

In case you want to test it earlier, please get a ESP32-S3-WROOM-1U-N8R2 and connect the 4 required SPI cables, plus 5V and GND making your own PCB.
