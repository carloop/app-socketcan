# CAN Analyzer with SocketCAN

SocketCAN is the Linux CAN networking stack and can-utils are a rich set of utilities to analyze CAN traffic. They are great all-purpose tools for car research and hacking.

You can use your Carloop as a SocketCAN interface.

## Hardware

Make sure you have your Carloop with a Particle Photon, Electron or
RedBear Duo connected on it.

Since we'll be using the Particle cloud to compile and flash the
SocketCAN firmware, you'll need a Particle account with the Carloop
device claimed to your account. For more information about claiming your
device on Particle, go to [particle.io/start](https://particle.io/start).

## Firmware

You'll need special firmware on your Carloop to print received CAN messages to the USB serial port and allow transmitting CAN messages sent to the USB serial port.

Flash the SocketCAN serial firmware application to your Carloop.

The easiest way to flash SocketCAN serial firmware is to open the shared
app in the Particle Web IDE and hit the Flash button (lightning bolt).

[Open the SocketCAN firmware >](https://go.particle.io/shared_apps/59375f5fba82211476001274)

## Computer Setup

To use SocketCAN you'll need a laptop running Linux. You could even do it from a Raspberry Pi.

Install the can-utils package: `sudo apt install can-utils`

SocketCAN has drivers to talk to many different devices. Since the Photon/Electron creates a serial port when you connect it to your laptop we'll use the serial line CAN driver (SLCAN).

Connect your Carloop to your computer through the USB connector. A serial port named `/dev/ttyACM0` should now be available. Check with `ls /dev/ttyACM*`

Link the serial port with a CAN network interface.
```
sudo slcand -o -c -s6 /dev/ttyACM0 can0
```
`slcand` comes from can-utils, `s6` is the speed for 500 kbit/s, `ttyACM0` is the serial port, `can0` is the CAN network interface)

At this point when you run
```
ip addr
```
you should see a CAN network 
```
3: can0: <NOARP> mtu 16 qdisc pfifo_fast state DOWN group default qlen 10
    link/can
```

When you are ready to start analyzing traffic, you bring up the CAN network by running
```
sudo ifconfig can0 up
```

You can then use other can-utils like `candump`, `cansniffer` and `cansend`.

Here's for example of running `candump can0` on a Ford Fiesta.

<iframe width="853" height="480" src="https://www.youtube.com/embed/igTsTbvPPP0?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

## Resources

Find out more about can-utils by running `candump -h`, `cansniffer -h`, `cansend -h`, `cangen -h`, `canplayer -h` and by looking at the [can-utils GitHub repository](https://github.com/linux-can/can-utils).

## Questions?

Join the [Carloop community forum discussion about SocketCAN](https://community.carloop.io/t/use-carloop-with-socketcan-and-can-utils/117) if you have questions or run into difficulties using Carloop as a SocketCAN device.

