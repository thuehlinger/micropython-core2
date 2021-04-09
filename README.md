# LV (Light and Versatile Graphics Library) MicroPython for M5Stack Core2

## Use of the custom firmware

Install ```esptool.py``` via ```pip```.
Connect the M5Stack Core2 via USB. Load the custom firmware using ```esptool.py``` specifying the correct tty:

```
esptool.py --chip esp32 --port /dev/tty.usbserial-6952FF0E93 erase_flash
esptool.py --chip esp32 --port /dev/tty.usbserial-6952FF0E93 write_flash -z 0x1000 ~/firmware/lv_micropython_1.14_168aa6a_esp32_idf4.0_m5stack_m5core2.bin
```

For a demo of the firmware, transfer ```main_test.py``` to the Core2 using ```ampy``` or your favorite CLI.


## Setup for compilation of the custom firware

You will need:
* An Ubuntu linux environment, preferably (I tested on Ubuntu 20.04)
* A set of development tools on Ubuntu (see below)
* esp-idf4, patched with files contained in this repo (```source/esp-idf_components```)
* lv_micropython or micropython, patched with files contained in this repo (```micropython_ports```)

### Install the build environment

```sh
sudo apt-get install build-essential libreadline-dev libffi-dev git pkg-config libsdl2-2.0-0 libsdl2-dev python3.8
```

### Clone lv_micropython repository

```
git clone --recurse-submodules https://github.com/littlevgl/lv_micropython.git
```

### Set up esp_idf4 toolchain and compiler

See https://lemariva.com/blog/2020/03/tutorial-getting-started-micropython-v20

### Patch esp_idf4

Files contained in ```esp-idf_components``` go into ```~/esp-idf/components```.

### Patch lv_micropython

Files contained in ```micropython_ports/esp32``` go into ```~/lv_micropython/ports/esp32```.

### Compile lv_micropython

```sh
cd ~/lv_micropython

export BOARD=M5CORE2_BOARD

make -C mpy-cross
make -C ports/esp32 LV_CFLAGS="-DLV_COLOR_DEPTH=16 -DLV_COLOR_16_SWAP=1" PYTHON=python3

```

The generated firmware will be located in ```ports/esp32/build-M5CORE2_BOARD/firmware.bin```.
