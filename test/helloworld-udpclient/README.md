# UDP Time Example
This application reads the current UTC time by sending a packet to utcnist.colorado.edu (128.138.140.44)

This example is implemented as a logic class (UDPGetTime) wrapping a UDP socket. The logic class handles all events, leaving the main loop to just check if the process has finished.

## Pre-requisites
To build and run this example the requirements below are necessary:
* A computer with the following software installed
  * CMake
  * yotta
  * python
  * arm gcc toolchain
  * a serial terminal emulator (e.g. screen, pyserial, cu)
  * optionally, for debugging, pyOCD
* A frdm-k64f development board
* An ethernet connection to the internet
* An ethernet cable
* A micro-USB cable

## Getting Started
1. Connect the frdm-k64f to the internet using the ethernet cable
2. Connect the frdm-k64f to the computer with the micro-USB cable, being careful to use the micro-usb port labeled "OpenSDA"
3. Check out a copy of mbed-example-network
4. Open a terminal in the root mbed-example-network directory
5. Update all the dependencies of mbed-example-network

    ```
    $ yt up
    ```

6. Check that there are no missing dependencies

    ```
    $ yt ls
    ```

7. Build the examples. This will take a long time if it is the first time that the examples have been built.

    ```
    $ yt build
    ```

8. Copy `build/frdm-k64f-gcc/test/mbed-example-network-test-helloworld-udpclient.bin` to your mbed board and wait until the LED next to the USB port stops blinking.

9. Start the serial terminal emulator and connect to the virtual serial port presented by frdm-k64f.

10. Press the reset button on the board.

12. The output in the terminal window should look like:

    ```
    UDP client IP Address is 192.168.2.2
    UDP: 3635245075 seconds since 01/01/1900 00:00 GMT
    ```

12. The LED should blink slowly (about 0.5Hz)

## Using a debugger

Proceed normally until step 7 included, then:

1. Open a new terminal window, then start the pyOCD GDB server.

    ```
    $ python pyOCD/test/gdb_server.py
    ```

    The output should look like this:

    ```
    Welcome to the PyOCD GDB Server Beta Version
    INFO:root:new board id detected: 02400201B1130E4E4CXXXXXX
    id => usbinfo | boardname
    0 => MB MBED CM (0xd28, 0x204) [k64f]
    INFO:root:DAP SWD MODE initialised
    INFO:root:IDCODE: 0x2BA01477
    INFO:root:K64F not in secure state
    INFO:root:6 hardware breakpoints, 4 literal comparators
    INFO:root:4 hardware watchpoints
    INFO:root:CPU core is Cortex-M4
    INFO:root:FPU present
    INFO:root:GDB server started at port:3333
    ```

2. Open a new terminal window, go to the root directory of your copy of mbed-network-examples, then start GDB and connect to the GDB server.

    ```
    $ arm-none-eabi-gdb -ex "target remote localhost:3333" -ex load ./build/frdm-k64f-gcc/test/mbed-example-network-test-helloworld-udpclient
    ```

3. In a third terminal window, start the serial terminal emulator and connect to the virtual serial port presented by frdm-k64f.

4. Once the program has loaded, start it.

    ```
    (gdb) c
    ```

5. The output in the terminal window should look like in step 11 above.

6. The LED should blink slowly (about 0.5Hz)