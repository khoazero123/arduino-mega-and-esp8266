ELECTRONICS - ARDUINO MEGA CONTROLLER WITH ESP8266
==================================================

Note dated 06/30/2017

In this article I will talk about the Arduino Mega controller combined with the ESP8266 controller. To test the functionality of the board, a counterfeit copy of this board ordered in China was used, so the photos and tested functionality refer to the copy rather than to the original. The original board can be found on the [RobotDyn website](http://robotdyn.com/ "Robotdyn") .

The purchase history of this board is as follows. To control the [layout of the railway](http://www.sysengineering.ru/blog/%D0%B1%D1%8B%D1%82-%D0%BC%D0%BE%D0%B4%D0%B5%D0%BB%D1%8C-%D0%B6%D0%B5%D0%BB%D0%B5%D0%B7%D0%BD%D0%BE%D0%B9-%D0%B4%D0%BE%D1%80%D0%BE%D0%B3%D0%B8-%D1%87%D0%B0%D1%81%D1%82%D1%8C-1/ "Gen. - Model of the railway (part 1)") using the DCC ++ protocol, I needed an Arduino board. First, I selected the Arduino UNO microcontroller and controlled the trains through the virtual COM port of the computer (USB port) connected to the microcontroller. But at the beginning of 2017, I found an article " [Roco z21 based on Arduino](https://sites.google.com/site/valerievalleyrr/arduino-projects/a21-ru ""lite" version of the Roco z21 based on Arduino") " which indicated that the layout can be controlled via WiFi if you use the Arduino Mega board, combined with the ESP8266 controller.

So, the board is made in the size of the original [Arduino Mega](https://www.arduino.cc/en/Main/ArduinoBoardMega "Arduino mega") controller . The ATmega2560 controller is shifted to the right side, and the ESP8266 controller is located on the left side. Above the WiFi controller, there is a USB connector, which is replaced with a conventional Type-B to Micro-B, and the interaction of this connector with the components of the board is via a USB-TTL CH340G converter and an 8-pin DIP switch. A photo of the board is presented below.

![WeMos Mega + WiFi](http://www.sysengineering.ru/media/3901/life-arduino-mega-and-esp8266-01.jpg?width=700)\
![WeMos Mega + WiFi](http://www.sysengineering.ru/media/3902/life-arduino-mega-and-esp8266-02.jpg?width=700)

The USB port mode selector may be in the modes described in the table below

|   | one | 2 | 3 | four | five | 6 | 7 | eight | RXD / TXD |
| USB isolated | --- | --- | --- | --- | --- | --- | --- | --- | 0 |
| USB\
<->\
ATmega2560 | --- | --- | ON | ON | --- | --- | --- | --- | 0 |
| USB\
<->\
ESP8266 (firmware) | --- | --- | --- | --- | ON | ON | ON | --- | 0 |
| USB <->\
ESP8266 (work) | --- | --- | --- | --- | ON | ON | --- | --- | 0 |
| ATmega2560\
<->\
ESP8266 | ON | ON | --- | --- | --- | --- | --- | --- | 0 |
| USB\
<->\
ATmega2560\
<->\
ESP8266 | ON | ON | ON | ON | --- | --- | --- | --- | 3 |

To test the board's performance, it is necessary to test the operation of each of the controllers, as well as the interaction of the controllers among themselves. To verify you will need:

-   The Mega-WiFi board itself.
-   A computer with a Windows operating system.
-   USB-microUSB cable.
-   USB-TTL driver for [CH340](http://robotdyn.ru/upload/drivers/CH340driver.rar "CH340 driver") converter .
-   [Arduino IDE](https://www.arduino.cc/en/Main/Software "Arduino IDE") software environment (old builds can be downloaded from the [version archive](https://www.arduino.cc/en/Main/OldSoftwareReleases "Previous IDE Releases") ).
-   Test firmware [Arduino Mega and ESP8266](http://www.sysengineering.ru/media/3907/arduino-mega-and-esp8266.zip "Arduino Mega and ESP8266.zip") .
-   Paper clip.

First you need to **install the board driver** , or more precisely, the driver of the CH340 controller. To do this, you need to connect the board to the computer using the USB cable, open the *Device Manager* , find the connected unknown USB device and update the driver for it, specifying the folder with downloaded and unpacked CH340 drivers as the driver source.

![](http://www.sysengineering.ru/media/3903/life-arduino-mega-and-esp8266-03.jpg?width=500)

After installing the drivers, the device appears in the device manager as *USB-SERIAL CH340 (COMx)* . You need to find this device and write out the number of the virtual COM port to which the board is connected, for example COM4.

![](http://www.sysengineering.ru/media/3904/life-arduino-mega-and-esp8266-04.jpg?width=500)

After the board has become visible, it can be disconnected from the computer, but you need to remember which USB port was connected to and include the board only in this port - if you include it in another, then the COM port number can change.

Now you can do the **installation and configuration of the Arduino IDE software environment** . After downloading the desired version of the program, for example 1.8.3, and installing on your computer, you need to configure it. To do this, do the following:

1.  Create a folder for your projects, for example *C: \ ArduinoProjects* .
2.  *Unzip the Arduino Mega and ESP8266.zip* archive into the project folder .
3.  Launch the *Arduino IDE* program .
4.  Open *Settings* via *File* -> *Settings* .
5.  In the *Sketch folder location* field, specify *C: \ ArduinoProjects* .
6.  In the *Additional links* field *for the board manager,* enter\
    *http://arduino.esp8266.com/stable/package_esp8266com_index.json* .

    ![](http://www.sysengineering.ru/media/3905/life-arduino-mega-and-esp8266-05.jpg?width=700)

7.  Save settings by clicking *OK* .
8.  Open the *Board Manager* through *Tools* -> *Board* -> *Board Manager* .
9.  Find *esp8266 by ESP8266 Comunity* in the list and install the package of the desired version, for example 2.3.0.
10. Close the *Arduino IDE* program .

To check the **operation of the ATmega2560 controller** , the following operations must be **performed** :

1.  Using a paper clip, switch the sliders of the DIP switch to the positions indicated in the table below.

    |   | one | 2 | 3 | four | five | 6 | 7 | eight | RXD / TXD |
    | USB\
    <->\
    ATmega2560 | --- | --- | ON | ON | --- | --- | --- | --- | 0 |

2.  Connect the board to the computer using a USB cable.
3.  Launch the *Arduino IDE* program .
4.  Select the *Arduino / Genuino Mega or Mega 2560 **board* through\
    *Tools* -> *Board* -> *Arduino / Genuino Mega or Mega 2560* .

    ![](http://www.sysengineering.ru/media/3906/life-arduino-mega-and-esp8266-06.jpg?width=500)

5.  Select the COM port that was written out from the *Device Manager* when installing the board driver, through\
    *Tools* -> *Port* -> *COM4* .
6.  Open the test program *TEST_MEGA* via *File* -> *Folder with sketches* -> *TEST_MEGA* .
7.  Download firmware to the controller via *Sketch* -> *Download* .
8.  After successfully downloading the firmware to the controller, open the *Port Monitor* through *Tools* -> *Port Monitor* .
9.  In the *Port Monitor* you need to select a baud rate of *115200 baud* , after which the test program menu should be displayed in the monitor window.

    ![](http://www.sysengineering.ru/media/3909/life-arduino-mega-and-esp8266-07.jpg?width=700)

10. In the input field for the *Port Monitor* commands , enter the English letter "a" and click *Submit* , in response to the command, the controller will enter the blinking mode with the built-in LED for 20 seconds.
11. The *Port Monitor* is necessary to send a command to "b" - the controller will free memory size of the SRAM = 7873 and the total size of memory EEPROM = 4096.

    ![](http://www.sysengineering.ru/media/3910/life-arduino-mega-and-esp8266-08.jpg?width=500)

12. The *Port Monitor* is necessary to send the command "c" - the controller will test the status of the outputs and produces a table with the results.

    ![](http://www.sysengineering.ru/media/3911/life-arduino-mega-and-esp8266-09.jpg?width=500)

    In the table, all outputs except 0, 1, and 13 must be OK. Faults were detected on pins 0, 1, 13, since 0 and 1 pins are occupied by a serial port (UART), and pin 13 has an integrated LED.
13. All checks are carried out - the controller must be disconnected from the USB port of the computer.

To check the **operation of the ESP8266 controller** , do the following:

1.  Using a paper clip, switch the sliders of the DIP switch to the positions indicated in the table below.

    |   | one | 2 | 3 | four | five | 6 | 7 | eight | RXD / TXD |
    | USB\
    <->\
    ESP8266 (firmware) | --- | --- | --- | --- | ON | ON | ON | --- | 0 |

2.  Connect the board to the computer using a USB cable.
3.  The program *Arduino IDE* select board WeMos D1 R2 & mini via the *Tools* -> *Payment* -> *WeMos D1 R2 & mini* .

    Note. The recommended board type Olimex MOD-WIFI-ESP8266 (-DEV) is not suitable.

4.  Check the controller settings in accordance with the figure.

    ![](http://www.sysengineering.ru/media/3912/life-arduino-mega-and-esp8266-10.jpg?width=700)

5.  Open the test program *TEST_ESP8266* through *File* -> *Folder with sketches* -> *TEST_ESP8266* .
6.  In lines 7 and 8 of the program, instead of dots, you need to insert the name of the home WiFi network and its password.
7.  Download firmware to the controller via *Sketch* -> *Download* .
8.  After a successful download, disconnect the board from the computer.
9.  Using a paper clip, switch the sliders of the DIP switch to the positions indicated in the table below.

    |   | one | 2 | 3 | four | five | 6 | 7 | eight | RXD / TXD |
    | USB <->\
    ESP8266 (work) | --- | --- | --- | --- | ON | ON | --- | --- | 0 |

10. Connect the board to the computer using a USB cable.
11. Using the *Port Monitor,* see what the controller gives out.

    ![](http://www.sysengineering.ru/media/3913/life-arduino-mega-and-esp8266-11.jpg?width=700)

    The values ​​of *Chip Real Size* (real size of memory) and *Chip Flash Size* (size of memory set during firmware) should be the same.\
    It is necessary to write down the IP address that the controller received from the WiFi switch - this address will be needed in the next test.
12. Check conducted - the controller must be disconnected from the computer's USB port.

To check the **interaction of the ATmega2560 and ESP8266 controllers, the** following operations must be **performed** :

1.  Using a paper clip, switch the sliders of the DIP switch to the positions indicated in the table below.

    |   | one | 2 | 3 | four | five | 6 | 7 | eight | RXD / TXD |
    | USB\
    <->\
    ATmega2560\
    <->\
    ESP8266 | ON | ON | ON | ON | --- | --- | --- | --- | 3 |

2.  Connect the board to the computer using a USB cable.
3.  In the *Arduino IDE* program, select the *Arduino / Genuino Mega or Mega 2560 **board* through\
    *Tools* -> *Board* -> *Arduino / Genuino Mega or Mega 2560* .
4.  Open the test program *TEST_MEGA-ESP* via *File* -> *Folder with sketches* -> *TEST_MEGA-ESP* .
5.  Download firmware to the controller via *Sketch* -> *Download* .
6.  After successfully downloading the firmware to the controller, open the *Port Monitor* through *Tools* -> *Port Monitor* .
7.  The *Arduino Mega* controller should read the data from the data exchange port with the *ESP8266* controller and output it to the *Port Monitor* window .

    ![](http://www.sysengineering.ru/media/3914/life-arduino-mega-and-esp8266-12.jpg?width=700)

8.  Open the page in the browser at the IP address provided in the *Port Monitor* .

    ![](http://www.sysengineering.ru/media/3915/life-arduino-mega-and-esp8266-13.jpg?width=500)

9.  In the open page, alternately press the *ON* and *OFF* buttons and monitor the status of the LED built into the board. The LED should turn on with the *ON* command and turn off with the *OFF* command .
10. Check conducted - the controller must be disconnected from the computer's USB port.

The board can be found in the online store RobotDyn, eBay or AliExpress with the keywords "ATmega2560 ESP8266 CH340G". The price of the board ranges from $ 10 to $ 25, depending on the quality of execution and the greed of the seller. I bought a board on AliExpress for $ 11.

Another review of the board can be read at this [link](https://geektimes.ru/post/287124/ "RobotDyn Strikes Double: Mega + ESP8266") .