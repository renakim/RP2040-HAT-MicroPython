
<a name="Ethernet_Example_Getting_Started"></a>
:rocket:Ethernet Example Getting Started [MicroPython]
===========================


> These sections will guide you through a series of steps from configuring development environment to running ethernet examples using the **WIZnet's ethernet products**.

- [:rocket:Ethernet Example Getting Started [MicroPython]](#rocketethernet-example-getting-started-micropython)
- [:hammer:Hardware requirements](#hammerhardware-requirements)
- [:bulb:Development environment configuration](#bulbdevelopment-environment-configuration)
  - [Firmware Release](#Firmware)
  - [STEP - 1 : **Installing Micropython**](#step---1--installing-micropython)
  - [STEP - 2 : **Setup Ethernet Libraray**](#step--2--setup-ethernet-libraray)
- [:open_file_folder:Ethernet example structure](#open_file_folderethernet-example-structure)
- [:pushpin:Ethernet example testing](#pushpinethernet-example-testing)
- [:books:Documentation](#booksdocumentation)
  - [Raspberry Pi Pico](#raspberry-pi-pico)
  - [WIZnet Ethernet HAT & EVB](#wiznet-ethernet-hat--evb)



<a name="hardware_requirements"></a>

# :hammer:Hardware requirements

> The ethernet examples use **Raspberry Pi Pico** and **WIZnet Ethernet HAT** ethernet I/O module built on WIZnet's [**W5100S**][link-w5100s] ethernet chip or **W5100S-EVB-Pico** ethernet I/O module built on [**RP2040**][link-rp2040] and WIZnet's [**W5100S**][link-w5100s] ethernet chip.

| Image                                                        | Name                                                      | Etc                                                          |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| <image src= "./static/images/START/raspberrypi_pico.PNG" width="200px" height="110px"> | [**Raspberry Pi Pico**][link-raspberry_pi_pico]           | [Pico Document](https://www.raspberrypi.org/documentation/microcontrollers/raspberry-pi-pico.html) |
| <image src= "./static/images/START/WIZnet-Ethernet-HAT-1.png" width="240px" height="80px"> | [**WIZnet Ethernet HAT**][link-wiznet_ethernet_hat]       | [Ethernet HAT Datasheet](https://docs.wiznet.io/Product/Open-Source-Hardware/wiznet_ethernet_hat) |
| <image src= "./static/images/START/W5100S-EVB-Pico_1.png" width="250px" height="90px"> | [**WIZnet W5100S-EVB-Pico**][link-wiznet_W5100S_evb_pico] | [W5100S-EVB-Pico Datasheet][link-wiznet_W5100S_evb_pico]     |

> ### Pin Diagram

**`WIZnet Ethernet HAT`**

Ethernet-HAT has the same pin arrangement and pin spacing as Raspberry Pi Pico. The W5100S and RJ45 are built-in, Ethernet can be used by plugging into the Raspberry pi pico. **One thing to note when using HAT is to look carefully at the direction and plug it in**. There is a USB shape marked, and this direction and the USB direction of Pico must be the same.

![][link-PICO_HAT]

------

**`WIZnet W5100S-EVB-Pico`**

In the W5100S-EVB-Pico board, GPIO pins are connected the same as the Raspberry Pi Pico board.  If Pico uses Ethernet, **PIO16, GPIO17, GPIO18, GPIO19, GPIO20**, and **GPIO21** pins cannot be used. It is a pin used inside the RP2040 board.

![][link-PICO_EVB]

| I/O  | Pin Name | Description                                    |
| :--- | -------- | ---------------------------------------------- |
| I    | GPIO16   | Connected to **MISO** on W5100S                |
| O    | GPIO17   | Connected to **CSn** on W5100S                 |
| O    | GPIO18   | Connected to **SCLK** on W5100S                |
| O    | GPIO19   | Connected to **MOSI** on W5100S                |
| O    | GPIO20   | Connected to **RSTn** on W5100S                |
| I    | GPIO21   | Connected to **INTn** on W5100S                |
| I    | GPIO24   | VBUS sense - high if VBUS is present, else low |
| O    | GPIO25   | Connected to user LED                          |
| I    | GPIO29   | Used in ADC mode (ADC3) to measure VSYS/3      |

<a name="development_environment_configuration"></a>

# :bulb:Development environment configuration



> To test the ethernet examples, the development environment must be configured to use Raspberry Pi Pico. 

- Required development environment
   - CMake (more then ver 3.12 )
   - [Thonny](https://thonny.org/) (that makes it easier to use micropython) 
- If you must be need to compile the micropython ,your pc should be use Linux or Unix environment.



<a name="Firmware"></a>
## Firmware

If you want to use the firmware without compile, you can use the below firmware.

Latest Releases : https://github.com/Wiznet/RP2040-HAT-MicroPython/releases

V1.0.4 : ▲ [**W5100s_v1.0.4.uf2**](https://github.com/Wiznet/RP2040-HAT-MicroPython/releases/download/v1.0.4/rp2_w5100s_20220318_v1.04.uf2)  ▲  [**W5500_v1.0.4.uf2**](https://github.com/Wiznet/RP2040-HAT-MicroPython/releases/download/v1.0.4/rp2_w5500_20220318_v1.04.uf2)

<a name="step1--installing-micropython"></a>

## STEP - 1 : **Installing Micropython**

1. Download
If the ethernet examples are cloned, the library set as a submodule is an empty directory. Therefore, if you want to download the library set as a submodule together, clone the ethernet examples with the following Git command.

```cpp
/* Change directory */
// change to the directory to clone
cd [user path]

// e.g.
cd D:/RP2040

/* Clone */
git clone https://github.com/Wiznet/RP2040-HAT-MicroPython.git
```

With Visual Studio Code, the library set as a submodule is automatically downloaded, so it doesn't matter whether the library set as a submodule is an empty directory or not, so refer to it.



2. Patch

With Visual Studio Code, each library set as a submodule is automatically patched, but if you do not use Visual Studio Code, each library set as a submodule must be manually patched with the Git commands below in each library directory.

Below is a brief description of the patch file

 ```cpp
  0001-Added-WIZnet-Chip-library.patch : about Ethernet(WIZnet Chip)
  0002-Added-AXTLSlibrary.patch : about SSL/TLS(AXTLS)
 ```

- Micropython
```cpp
  // e.g.     cd D:/RP2040/RP2040-HAT-MicroPython
    cd [user path(=github source code setup path)] 
    /* Patch and submodule */
    cmake CMakeLists.txt

```

3. compile

```cpp

  cd libraries/ports/rp2
  vi Makefile
```

If you enter this path, you can see the below code.

![][link-setup_6]

durning patch , if occur the fail, you can't see the below code in the Makefile.
```cpp
  MICROPY_PY_WIZNET5K ?= 5105 
  //and
  CMAKE_ARGS += -DMICROPY_PY_WIZNET5K=$(MICROPY_PY_WIZNET5K)
```

If you want to use the w5100s and rp2040, you can just use like below code.

```cpp
  make
```

If you want to use the w5500 and rp2040, you can just use like below code.

```cpp
  make MICROPY_PY_WIZNET5K=5500
```


After that, you can visit the micropython link below and check it

[![DHCP and Patch](http://img.youtube.com/vi/86opIykPE-U/0.jpg)](https://youtu.be/86opIykPE-U)
[https://docs.micropython.org/en/latest/develop/gettingstarted.html#compile-and-build-the-code](https://docs.micropython.org/en/latest/develop/gettingstarted.html#compile-and-build-the-code)

:warning:**Notice**

Install `Thonny IDE` on Raspberry Pi Pico by referring to the link above.:point_down:

 - **https://projects.raspberrypi.org/en/projects/getting-started-with-the-pico/0**



------

- The drive will be called **RPI-RP2** on all RP2040 boards. Download the **UF2**(firmware.uf2) file from the link below and put the file in Pico.


- You can also access the firmware installation menu if you click on **MicroPython (Raspberry Pi Pico)** in the status bar and choose ‘Configure interpreter …’.

![][link-setup_1]

- The interpreter settings will open. Click on **Install or update firmware**. 

![][link-setup_2]

- You will be prompted to plug in your Raspberry Pi Pico while you hold the BOOTSEL button.

![][link-setup_3]

- Then you can click **Install**. Wait for the installation to complete and click **Close**.

![][link-setup_4]

- Look at the Shell panel at the bottom of the Thonny editor. You should see something like this:

![][link-setup_5]



<a name="step--2--setup-ethernet-libraray"></a>

# STEP - 2 : **Setup Ethernet Libraray**

`First, import the library of the function you want to use from the library to your PC. To uploading a file to using Thonny IDE, follow these next steps.`

You can access the *umqttsimple* library code in :point_down:

https://github.com/micropython/micropython-lib/blob/master/micropython/umqtt.simple/umqtt/simple.py

You can access the *urequest* library code in :point_down:

https://github.com/micropython/micropython-lib/blob/master/python-ecosys/urequests/urequests.py



- Create a new file, Save it in your computer with the exact name that you want, for example “**(your library name).py**”
- Go to **Open > This computer**

![][link-mpy_lib_1]

- The file should be saved on RPi Pico with the name **“(your library name).py”**
- Go to **File > Save as > Raspberry Pi Pico**

![][link-mpy_lib_2]





<a name="ethernet_example_structure"></a>

# :open_file_folder:Ethernet example structure

Ethernet examples are available at '**RP2040-HAT-MicroPython/examples/**' directory. As of now, following examples are provided.

- [**Loopback**][link-loopback]
- [**DHCP**][link-DHCP]
- [**UPIP**][link-UPIP]
- [**HTTP**][link-HTTP]
  - [WebServer][link-WebServer]
  - [WebClient][link-WebClient]
- [**MQTT**][link-MQTT]
  - [Publish][link-MQTT_Pub]
  - [Subscribe][link-MQTT_Sub]

<a name="Ethernet_example_testing"></a>

# :pushpin:Ethernet example testing

Check if the network is connected normally and if the data is sent to each other.

[w5x00_Ping_Test.py](https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/examples/PING_TEST/w5x00_Ping_Test.py)

> This is the code to set the IP of 192.168.1.20
>
> I hope that the PC also has an environment that communicates with 192.168.1.xxx.

1. Copy the content to code.py on your RPi Pico and run.

![][link-ping_1]

2. Press the `Win+R` key to enter cmd and press Enter or OK to execute the **command prompt**.

![][link-ping_2]

3. When the command prompt window appears, type the **ping command** and press Enter.

```
ping 192.168.1.20 (-option)
```

![][link-ping_3]

4. Ping tests begin as packets are exchanged between hosts. If you look at the time and loss rate among the statistical results, you can find out the status of the Internet network. 

![][link-ping_4]





<a name="Documentation"></a>

# :books:Documentation

Documentation for WIZnet Ethernet HAT and Raspberry pi pico board
## Raspberry Pi Pico
 [**Raspberry Pi Pico Datasheet**](https://datasheets.raspberrypi.org/pico/pico-datasheet.pdf) : An RP2040-based microcontroller board

 [**Getting started with Raspberry Pi Pico**](https://www.raspberrypi.org/documentation/microcontrollers/raspberry-pi-pico.html) :  C/C++ development with Raspberry Pi Pico and other RP2040-based microcontroller boards

## WIZnet Ethernet HAT & EVB
 [**WIZnet Ethernet HAT**](https://docs.wiznet.io/Product/Open-Source-Hardware/wiznet_ethernet_hat)

 [**W5100S-EVB-Pico**](https://docs.wiznet.io/Product/iEthernet/W5100S/w5100s-evb-pico)

Please refer to 'README.md' in each examples directory to find detail guide for testing ethernet examples.





<!--

Link

-->

[link-Installing Micropython]:https://thonny.org/
[link-w5100s]: https://docs.wiznet.io/Product/iEthernet/W5100S/overview
[link-rp2040]: https://www.raspberrypi.org/products/rp2040/



[link-PICO_HAT]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/HAT.png
[link-PICO_EVB]: https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/EVB.png

[link-raspberry_pi_pico]: https://www.raspberrypi.org/products/raspberry-pi-pico
[link-wiznet_ethernet_hat]: https://docs.wiznet.io/Product/Open-Source-Hardware/wiznet_ethernet_hat
[link-wiznet_W5100S_evb_pico]:https://docs.wiznet.io/Product/iEthernet/W5100S/w5100s-evb-pico
[link-library]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/libraries



[link-Micropython]: https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/MicroPython_Thonny.png
[link-firmware]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/firmware.png
[link-setup_1]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/thonny-configure-interpreter.png
[link-setup_2]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/thonny-interpreter-settings.png
[link-setup_3]: https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/thonny-bootsel.png
[link-setup_4]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/thonny-firmware-install.png
[link-setup_5]: https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/repl-connected.png

[link-setup_6]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/rp2_makefile.JPG

[link-mpy_lib_1]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/mpy_lib_1.png
[link-mpy_lib_2]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/mpy_lib_2.png



[link-loopback]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/Loopback
[link-DHCP]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/DHCP
[link-UPIP]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/UPIP
[link-HTTP]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/HTTP
[link-WebServer]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/HTTP/HTTP_Server
[link-WebClient]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/HTTP/HTTP_Client
[link-MQTT]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/MQTT
[link-MQTT_Pub]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/MQTT/Publish
[link-MQTT_Sub]:https://github.com/Wiznet/RP2040-HAT-MicroPython/tree/main/examples/MQTT/Subscribe



[link-ping_1]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/Pico_ping.png
[link-ping_2]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/pico_ping2.png
[link-ping_3]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/pico_ping3.png
[link-ping_4]:https://github.com/Wiznet/RP2040-HAT-MicroPython/blob/main/static/images/START/pico_ping4.png


_[▲ Back to Top](#Ethernet_Example_Getting_Started)_ 

