# OBDII_Dash
## General Information
------
This project is for demo purpose only, not for real world applications.

The dashboard won't be as smooth as the real dashboard on car, Gear and Boost display won't work, this is due to the protocol limitations.

Modern car only expose diagnostic CAN bus in OBD2 connector, and everything else run on an internal CAN bus.

This project using standard OBD2 protocol, which do not provide gear and boost information, also has 20 queries per second limitation.

Therefore there are two ways to get it work perfectly:

A. Reverse engineer ECU and other module's firmware to figure out the Subaru Scan Tool proprietary protocol.

B. Tap into my car's internal CAN bus, then sniff instead of query from OBD2.

However, I decided to not take my car's ECU apart or poke into wires since my car still under warranty, and the goal of this project is to demonstrate my knowledge and skill rather than build a complete solution.

## How to build

------

### Adapter side

Install VS Code, Platform IO plugin, open project folder [OBDII_Dash_Adapter](https://github.com/424778940z/OBDII_Dash/tree/main/OBDII_Dash_Adapter)

This demo is based on ESP32 Board which come with built in CAN bus support, only needs an external CAN transceiver.

This project using default pin from the library.

ESP32 GPIO 4 --> transceiver CAN RX

ESP32 GPIO 5 --> transceiver CAN TX

If you are using other Arduino board, you may need a MCP2515 module, the Arduino CAN library supports both.

After you swap the driver initialize functions it should be good to go.

### Computer side

This demo was built on Windows with Msys2, however Qt is easily portable.

On Windows, install Msys2, Qt package, and Qt Creator package, and other package as needed, do not have an auto configure for now.

On Linux, same, just no Msys2.

Open project under [OBDII_Dash_UI](https://github.com/424778940z/OBDII_Dash/tree/main/OBDII_Dash_UI)

Should be good to go without change anything.

### Hardware side

I made a customized board with DC-DC step down module, CAN transceiver module and CAN to serial module for monitoring.

You could skip DC-DC by powering the whole circuit through USB.

You may ignore CAN to serial part, it is not needed.

You may need a serial to USB adaptor if your board do not come with one.

## How to run

------

1. Flash adapter firmware, then connect to OBD2 on car, and serial port on computer.
2. Open dashboard application, chose the correct serial port.
3. Dashboard application is now waiting for data from adapter.
4. Turn on the car, the dashboard will start displaying information.

## How to test

------

In file [OBDII_Dash_Adapter/src/main.cpp:298](https://github.com/424778940z/OBDII_Dash/blob/main/OBDII_Dash_Adapter/src/main.cpp#L298)

### Testing communication between adapter and dashboard application

You could change `dpkg.mode = CMD_MODE_OPTIMIZED;` to `dpkg.mode = CMD_MODE_RANDOM;` 

This will make the adaptor report back random data with in reasonable range.

### Testing CAN OBDII without dashboard application

You could change `dpkg.mode = CMD_MODE_OPTIMIZED;` to `dpkg.mode = CMD_MODE_DEBUG;` 

This will make the adaptor print all information in text through serial port.

# Photos and Videos

------
As stated above, you could ignore the CAN sniffer / monitor module (biggest green module).

Two jumpers are for switching power and CAN sniffer modes.

There is no other chip / boards in the OBD2 connector, just a break out connector.

## Whole Setup
![Whole Setup](../main/images/whole.png?raw=true)

## Board Front
![Board Front](../main/images/board_front.jpg?raw=true)

## Board Back
![Board Back](../main/images/board_back.jpg?raw=true)

## UI Speed Test
[![](http://img.youtube.com/vi/PYe7n_EPdbE/0.jpg)](http://www.youtube.com/watch?v=PYe7n_EPdbE "UI Speed Test")

## In Car Test
[![](http://img.youtube.com/vi/MfvGJcwQHoQ/0.jpg)](http://www.youtube.com/watch?v=MfvGJcwQHoQ "In Car Test")