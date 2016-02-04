# Introduction

Welcome to the OpenHybrid developer community!

This introductory section will provide you with a short overview of the basic concepts you should know before you start
to implement your own hardware interface.


## What is a HybridObject?
A HybridObject is a physical object which is augmented by a virtual object. It consists of so called **IOPoints** which can send and receive data.
IOPoints can be interconnected so that one IOPoint's output can be used as input for a connected IOPoint. Additionally a HybridObject has an
user interface. To learn more about this read: TODO: Add links

## What is a hardware interface?

A hardware interface is the bridge or connector between your hardware and the OpenHybrid system. It maps input and output values and events of your hardware
into the OpenHybrid system. Let's take a simple lamp as example. Assuming the lamp is reachable via some protocol and you want to expose its status - if it is
on or off - as IOPoint. Then your hardware interface must communicate with the lamp via the protocol and notify the OpenHybrid server whenever the lamp is
switched on or off. On the other hand you must relay the on/off information you receive on the status IOPoint from the OpenHybrid server to the lamp and turn 
it on or off.

## Follow the design guidelines!

When you implement your hardware interface you should follow the design guidelines which are explained in detail here (TODO: Add link).
The most important thing is that your IOPoints only handle values in the range '[0,1]'. This restriction ensures that all IOPoints can
communicate with each other.


# Hardware Interfaces API

The Hardware Interfaces API provides all the functions you need to create your own hardware interface.

## writeIOToServer(objName, ioName, value, mode)

With this function you can pass data from your hardware interface to the HybridObjects server.

* **objName** is a string value containing the name of the HybridObject
* **ioName** is a string value containing the name of the IOPoint
* **value** is a floating point value
* **mode** is a string value and can be one of the following:
  * **"f"**: indicate that **value** is a float value
  * **"d"**: indicate that **value** is either 0 or 1 (digital)
  * **"p"**: indicate that **value** is a floating point value meant to be added to the receiving IOPoint
  * **"n"**: indicate that **value** is a floating point value meant to be substracted from the receiving IOPoint

# Creating a new hardware interface

Description of how to implement a new hardware interface

# Example implementation

An example, probably simply the mpd interface