# PiCon-One Software, Code and Programs

![test](https://github.com/fm4dd/picon-one-sw/workflows/test/badge.svg)

## Background

This is the software repository for the PiCon One Controller based on a Raspberry Pi Zero W.
The hardware repository is located at https://github.com/fm4dd/picon-one-hw.

## Components

### DS3231 Realtime Clock (I2C 0x68)

- Setup log [setup1_rtc_ds3231.md](./setup1_rtc_ds3231.md)
- Test Code: src/rtc-ds3231/test-ds3231.c
```
pi@rpi0w:~ $ cd picon-one-sw/src/rtc-ds3231/

pi@rpi0w:~/picon-one-sw/src/rtc-ds3231 $ make
gcc -Wall -g -O1    test-ds3231.c   -o test-ds3231

pi@rpi0w:~/picon-one-sw/src/rtc-ds3231 $ ./test-ds3231
DS3231 DATE: Tuesday 2020-06-02 TIME: 00:36:16
```

### 7-Segment display TM1640 (2-wire serial)

- Setup:
The 7-segment display examples require the WiringPi library:

```
pi@rpi0w:~/picon-one-sw $ sudo apt-get install wiringpi
```

- Test Code: src/rtc-7seg-tm1640

The test code is based on the TM1640 userspace driver written
by Michael Farell, with thanks and full credits.
The original driver code is located in https://github.com/micolous/tm1640-rpi, and
licensed under GPLv3.

I modified the code for supporting the two 4-digit modules
with a total of 8-digits, adding function to support
the extra colon and degree LED segments, and confirming signal
generation with an oscilloscope.

```
pi@rpi0w:~/picon-one-sw/src/7seg-tm1640 $ make
gcc -Wall -g -O1   -c -o tm1640.o tm1640.c
gcc -Wall -g -O1   -c -o tm1640-ctl.o tm1640-ctl.c
gcc tm1640.o tm1640-ctl.o -o tm1640-ctl -lwiringPi -lm
gcc -Wall -g -O1   -c -o timetest.o timetest.c
gcc tm1640.o timetest.o -o timetest -lwiringPi -lm
gcc -Wall -g -O1   -c -o stopwatch.o stopwatch.c
gcc tm1640.o stopwatch.o -o stopwatch -lwiringPi -lm
gcc -Wall -g -O1   -c -o temptime.o temptime.c
gcc tm1640.o temptime.o -o temptime -lwiringPi -lm
```

1. timetest

This program shows the current time on all eight digits: hour, min, seconds, and two-digit sub-seconds. It demonstrates the responsiveness of apps under RPi Linux.

2. temptime

This program shows the current time in HH:MM on the first (left) display, and the CPU temperature on the second (right) display. This program is helpful to monitor how the controller heat develops inside the case when the protective lid is closed.

3. stopwatch

This program implements a simple stopwatch, showing the elapsed time over all eight digits. It is controlled by three push-buttons: "Mode" starts or restarts the clock, "Enter" stops the clock, and "Up" resets the clock to zero.

## License

MIT License

Other license forms for derivative work may apply, as as listed from original authors.
