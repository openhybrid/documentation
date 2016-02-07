# How to develop directly on the Arduino Yun 

The OpenHybrid arduino Yun image has SSHFS on board.
You can mount the filesystem via 

    sshfs root@<objectName>.local:/ ~/mountpoint

Additional you can get in to the shell via:

    ssh root@<objectName>.local

The image has a swap image activated. 
This means that npm installations for node.js will work just fine, but take for ever to complete.



# How to connect Lego Mindstorms with the Arduino Yun

In order to connect Lego with the Arduino Yun, you can use the following diagrams.
You might want to play around with the voltage divider.
I just added it to the circuit and have not tested if the values of the resistors are right.

![](http://forum.openhybrid.org/uploads/default/optimized/1X/07b7aba0b2256d36152371b251e2b1ac1dd8c53b_1_543x500.jpg)

 The Motor needs to connect to an H-Bridge. We found the SN754410 very useful. A good example for how to use an H-Bridge with the Arduino can be found  [http://itp.nyu.edu/physcomp/labs/motors-and-transistors/dc-motor-control-using-an-h-bridge/](http://itp.nyu.edu/physcomp/labs/motors-and-transistors/dc-motor-control-using-an-h-bridge/)

The other simple sensors need a certain wiring but then they can be read just as an analog sensor (EOPD, Force), or a push button (Touch). 

As you can see the Diagramms are from 2013. We used EV3 Motors, but all the Sensors are from a previous generation of Mindstorms or from [https://www.hitechnic.com](https://www.hitechnic.com)

The EV3 Sensors have a data bus implemented. It is more difficult to connect with them.


# How to connect the Yun to the Arduino’s servo Library

You will need to use an h-bridge to power the motors. Something like this here: 
[http://www.digikey.com/product-detail/en/A4953ELJTR-T/620-1428-1-ND/2765622](http://www.digikey.com/product-detail/en/A4953ELJTR-T/620-1428-1-ND/2765622)

The MIT CBA has a circuit for it:

![](http://forum.openhybrid.org/uploads/default/original/1X/96eaf860f00d479871322be3b2e7af6b34d89a8b.png =500x500)

The H-bridge allows you to move a motor in two directions.
The motor can have a different voltage then your arduino yun. 

You can find more interesting output possibilities from the MIT Media Lab and CBA class called "How to Make almost Anything" [here: http://academy.cba.mit.edu/classes/output_devices/index.html](here: http://academy.cba.mit.edu/classes/output_devices/index.html)

If the CBA example does not match, a tutorial for another component that runs with 5 Volt for the motor can be found here:
http://itp.nyu.edu/physcomp/labs/motors-and-transistors/dc-motor-control-using-an-h-bridge/

WATCH OUT: The Yun only runs internally with 3.3 volt! Never give it more. 
However, you can power it with via the USB connector with regulated 5 volts. 

Double check and test your circuits.

You can learn more about the Arduino Yun here:
[https://www.arduino.cc/en/Main/ArduinoBoardYun](https://www.arduino.cc/en/Main/ArduinoBoardYun)

