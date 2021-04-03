This set of instructions assumes that you have a fully soldered and functional PhoneBot PCB. We will set up the firmware on the board, and make sure that Bluetooth is functional.

## Configure the board definition in Arduino IDE

First we need to install the boot loader onto the AtMEGA32u4. The AtMEGA is running at 3.3V, and therefore the clock frequency can be configured to be at most 8 MHz. For the boot loader, we will be using SparkFun's Arduino Pro Micro 8MHz boot loader, since it has the same processor running in the same configuration. This assumes you have the Arduino IDE installed. If not, install [Arduino 1.8.10](https://www.arduino.cc/en/Main/OldSoftwareReleases#previous). Other versions have not been tested.

Open the Arduino IDE. Go to `Tools/Board/Boards Manager`. Next search for `SparkFun AVR Boards` and install the board set. If the board is not available, you need to add the package to your preferences: do so by following the instructions [here](https://learn.sparkfun.com/tutorials/installing-arduino-ide/board-add-ons-with-arduino-board-manager).

Select the `SparkFun Pro Micro` board from the boards list. Select the `3.3V 8MHz` configuration.

## Flashing the Boot Loader

If the Boot Loader is already flashed, and you simply want to install firmware, skip this section and proceed to [Upload Firmware](#upload-firmware).

Now it's time to program the board. Use your preferred AVR programmer supported by the Arduino IDE. Select your programmer from the list in `Tools/Programmer`.

Since programming headers are too large to have permanently on the board, the ICSP header won't be populated on the board. Instead, when programming, place the male ICSP header in the programmer, and hold the pins in place with your hand while you program the board.

With the programming header in place, make sure that the board is powered appropriately for the programmer you are using. The `AVR ISP MKII` requires the board to have it's own power, but other programmers may have options to supply power. It is advised to use a programmer which does not provide power, since the BLE module, which is 3V3 tolerant, is on the same power rail, and can be damaged by accidental 5V supply. PhoneBot is also designed to provide it's own power. To power PhoneBot, plug in the batteries and power the board by turning the power switch to `On, or plug in a power-only USB-C cable and turn the power switch to `On`(to ensure that accidental data isn't sent during programming of the boot loader). Next click`Tools/Burn Bootloader`.

## Troubleshooting Bootloader Programming

If everything goes well, then the boot loader will be installed. If not, however, here are some helpful troubleshooting tips. First, make sure that all error messages are being displayed. This will make debugging a lot easier. In the Arduino IDE, go to `File/Preferences` and make sure that `Show verbose output during: ` is checked for `compilation` and `upload`. Re-try the upload with the new settings.

### Common Errors

#### Pin Reversal

This will typically result in the following error:

    avrdude: stk500v2_program_enable(): bad AVRISPmkII connection status: Target reverse inserted
    avrdude: initialization failed, rc=-1
             Double check connections and try again, or use -F to override
             this check.


    avrdude done.  Thank you.

    Error while burning bootloader.

The ICSP header is meant to be inserted into the top of the board, with GND in the bottom right. Check with the schematic and layout to be certain.

## Upload Firmware

Once the bootloader is installed, we will be able to upload Arduino sketches. Plug in a USB C cable to the board. Open the sketch `PhoneBot/embedded/firmware/firmware.ino` and load it onto the board. It will automatically configure the BLE module for you. If, instead, you would like the procedure for configuring Bluetooth in order to transmit and receive data with the ATMEGA, continue reading.

### Testing BLE

Open the sketch `PhoneBot/embedded/test_ble/test_ble.ino`. This sketch is configured to user the ATMEGA to route serial commands from the user's computer to the BLE module and back. This means that any serial command you send through the Arduino Serial Monitor (in the upper right hand corner) will be forwarded to the BLE module. By default, the baud rate of the BLE module is 115200. This is too fast for the ATMEGA to be able to forward reliably (read and then write to USB serial, it has no problem reading 15200 baud). So make sure that `Serial1` is set to 115200, and `Serial` is set to something less than that (57600 or less).

Now Upload the sketch and open the Serial Monitor. You may need to restart the BLE module (turn the board's power switch off and then on while still plugged in to USB). Make sure that `Carriage Return` mode is enabled in Serial Monitor. If command mode is not entered by default, then enter it by typing `$$$` and press enter (make sure that carriage return is not enabled when sending the `$$$` command). If the result is garbled, that's ok. Now we need to set the baud rate of the serial monitor to something lower. To do this we run the command `SB,04` (with carriage return). Exit out of command mode by typing `---` with a carriage return. Now, restart the device in order to enable the new settings.

In the sketch, change the `Serial` and `Serial1` rates to 57600. Now, re-upload the sketch and all responses from the BLE module should be readable.

Full documentation of all of the commands is in the [user datasheet](http://ww1.microchip.com/downloads/en/DeviceDoc/RN4870-71-Bluetooth-Low-Energy-Module-User-Guide-DS50002466C.pdf).

#### Configure BLE Transparent UART

In order to give the PhoneBot board commands through Bluetooth, we need to enable transparent UART. Do this by entering

```
+\r
SS,C0\r
SR,0100\r
R,1\r
```

in command mode. This first turns on echo so that commands sent to the BLE module are forwarded to it's serial port (and thus to the PhoneBot board). This is useful for debug purposes, since it echos back the command you send it. Next, it enables the transparent UART service. Then it enables `Write Without Response`, which allows the phone to write data to the RX service without waiting for an ACK to come back from the PhoneBot. This speeds up communication. Finally, it reboots in order to enable the settings.

## Connect to a Phone

It is now possible to connect and send data to the PhoneBot board via an app. We will be using the `Microchip Bluetooth Data` app [found here](https://play.google.com/store/apps/details?id=com.microchip.bluetooth.data&hl=en_US).

### Connect

Open the app and click `BM 64`. Click `Scan` and then find your device and click on it. Click `Transfer data to device` (Transparent UART). Make sure that `Write with Response` is `ON` (on the bottom of the screen). You should be able to send the PhoneBot data by typing text in the `Raw` tab and clicking send. You can also stress test the connection by using the `Timer` tab and repeatedly sending messages. The connection should be capable of sending 20 byte packages with 50ms delay without overflowing the queue.
