# DEXA-C097 ESP32S3 HAT

Hardware repository for Cinwrite PCB, a HAT for 9.7 inch parallel epaper display controller **DEXA-C097** sold by Good display which uses IT8951 as epaper timings controller and display engine.

This hardware is open source and you can use it commercially or modify it for your own purpouses according to the terms specified in the [License](https://github.com/martinberlin/H-cinread-it8951/blob/main/LICENSE). The only restriction is that it should be not mass produced and not sold without the permission of the Licensor.

![License SVG](/Schematic/oshw_license.svg)

Licensed by:

![Fasani Corporation Logo](/Schematic/Fasani_logo.png)

We only ask to be notified if you make a product based on this so we can showcase it as a success story. Collaborations and corrections on any terms including lowering consumption are welcome, please make an Issue, or feel free to contact me to the email in my github profile.

## OSHWA Certification UID [ES000029](https://certification.oshwa.org/es000029.html)

**The goal of this HAT is to provide:**

- WiFi
- BLE
- DS3231 real time clock
- Fast 40Mhz SPI
- 3V to 5V step-up
- 3.7v LiPo battery charger

You can get the produced and tested [Cinwrite board in Tindie](https://www.tindie.com/products/fasani/cinwrite-dexa-c097-hat-for-parallel-epapers) and at the same time you collaborate with our research.

## Where to get this

Even if you plan to send a batch for fabrication, we strongly recommend to get one first, so you collaborate with my work. But more importantly you can test the hardware to evaluate if it suits your project.
I don't have big quantities at the moment but you can get one at:

- [Lectronz FASANI store](https://lectronz.com/stores/fasani)
- https://www.tindie.com/stores/fasani

Both have similar prices. We can ship to all European countries and also to China mainland. Please check the hardware notices and also the DEXA-CO97 board from GOOD-DISPLAY if you want to drive a 1200x825 9.7" epaper display.

## Hardware notices

Cinread early revisions that Goodisplay is selling need a small very tiny bridge in the resistances at the side of SPI. Also the Silkscreen has an error and SI is swapped by CLK.
Please refer to the following image to the correct SPI labels:

![Corrected labels](components/assets/cinread-correct-IO.jpg)

Important: Please mind that the Cinread PCB has the SPI labels wrong. And that it needs a 0 Ω 0402 in the position marked as yellow. 6th counting from below at the SO Slave Output pin is please check the image carefully and place it in the right side. This will enable SPI.

UPDATE: On last point about the 0 Ω resistance just check that is there. But Good-Display is selling this with the hardware modification done so SPI should be running as it should.

## Ideas that could be added to the schematic

Provided by colleagues [Larry Bank](https://github.com/bitbank2) and [Lovyan03](https://github.com/lovyan03)

1) Increase the pull-up resistors from 2.2 to 4.7K. Safer if the external I2C device already has its own pullups. DONE
2) Add a voltage divider to measure the battery voltage. Something with one end connected to another GPIO to allow the current to be shut off completely when not needed. DONE
3) You might want to get a connector to connect a JTAG debugger. DONE

## ESP32-S3 Pin for JTAG

Referenced from this page in [Espressif documentation](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/api-guides/jtag-debugging/configure-builtin-jtag.html)

	
```
GPIO19 D-

GPIO20 D+

5V -> V_BUS
```

This second USB will also enable DFU mode to flash the Microcontroller.

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

### Firmware examples

Meet [epaper weather station](https://github.com/martinberlin/epaper-weather-station) first project featuring this hardware and our test PCB.
![epaper-weather-station-blurred](https://user-images.githubusercontent.com/2692928/174765248-a73e6c50-6e04-450f-8496-265ebc25c480.jpg)

[Schematics](/Schematic/IT8951-S3-HAT-Schematic.pdf)

### Other hardware projects

- [Bistable-smart-switch](https://github.com/martinberlin/bistable-smart-switch) smart switch built with ESP32C3 and epaper display
- [SPI Adapters](https://github.com/martinberlin/H-spi-adapters) various SPI adapters and related PCBs
