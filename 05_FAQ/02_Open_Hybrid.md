## Why can't i see my I/O points after addding them in arduino code, but i can see the slider?

The I/O points are added to the HybridObject using the **.add([Hybrid Object], [IO Point])**
function which is defined in the HybridObject library  which can be downloaded [here](http://openhybrid.org/download.html)
and has to be placed in the Arduino IDE library folder. For example if you have named your interface "slider" as shown in the videos [here](http://openhybrid.org/adding-web-content.html) , then to add an I/O point to the HybridObject called "slider", 
you use **.add( "slider", "your_I/O_point_name")**. This will add your I/O point to the specified object.


## I cannot access the HybridObject interface on arduino_name.local:8080 after resetting the Yun with the HybridObject image

1. This might be due to the name not being resolved to ip automatically, in that case find the ip of the arduino yun from yours routers configuration page ( say it is 192.168.0.5) you can access the hybridobject interface by from 192.168.0.5:8080
2. You need to be connected in a private WIFI network. If you have a professional network, UDP broadcasting on Port 52316 needs to be possible.
3. It needs around 2 minutes for the Hybrid Object to fully load on an Arduino Yun. There is a white LED on the Yun that starts glowing after two minutes. Only if it starts blinking right after, the Yun is a Hybrid Object.
4. Check your router/modem configuration page to see if wireless isolation is enabled. If it is enabled, then disable it. It prevents devices from communicating through your ports (8080, 80) which are used for various functions.
5. Your Arduino Yun might has an old firmware. Before Release 1.3 (July 14th, 2014) the Yun would trigger the fail-save boot mode if there is any activity on the serial interface during the boot process. With Release 1.3 it has been changed to trigger the fail-save boot mode with typing "ard" on the serial interface. A timeout at start was not necessary anymore and therefore we removed it. It can be possible that you have a board that is older then release 1.3. If you want to update your yun firmware, you need to do the following instructions to update your arduino yun: [https://www.arduino.cc/en/Tutorial/YunUBootReflash](https://www.arduino.cc/en/Tutorial/YunUBootReflash)

## My Hybrid Object does not respond to my Arduino Code.

1. Have you programmed the Arduino with the Arduino code?
2. If the Arduino is programmed and you still do not see any sign for the IO-Points it could be the following: Sometimes the Open Hybrid platform and the Arduino Code are out of synchronization.
In that case you need to push the Arduino Reset button (32u4 Reset) on the Arduino Yun board.
Most likely after you press this button, the visual IO-Points will show up. 

![](http://forum.openhybrid.org/uploads/default/original/1X/82d776ee7c7175976ee2126b91462facff582f25.png)


## I want to upload a target file but nothing happens.
Make sure that your browser is not automatically unzipping the zip file.
For example: Safari > Preferences > General > uncheck: Open “safe” files after downloading

## I compiled a code with Arduino but no data is showing up in the Object.
If you just send constant data to the Hybrid Object, it will not be submitted.
The system only sends updates of data. This is to prevent any overload in the system.
Once your values are changing, these changes will be sent.

## My Object does not show up in the Editor or anywhere else.
Make sure that your Wifi Network supports UDP Broadcasting. This is usually the case for all private networks. In cooperate or university networks, the UDP broadcasting might be deactivated. 


## My object shows up, but I can not move and scale
Make sure that the object.js file present and you have referenced object.js in your html code:

      <script src="object.js"></script>

This file handles the communication between your object interface and the editor.

## Is it possible to use Arduino mega, uno or any Arduino other then the Yun?
Unfortunately the Hybrid Object code does not run on this Microprocessor. You need to have enough capacity to run the node.js runtime compiler on the system. Right now, the Arduino Yun is the only board from the Arduino family that has enough capacity.

## What's the functionality of the Arduino-YUN-Image? Is it uniform for all Hybrid Objects?

The Arduino-YUN-Image is the operation system for the Arduino Yun. You will need to prepare a SD card with it so that you can start your Arduino Yun with it. [http://openhybrid.org/install.html](http://openhybrid.org/install.html)

This image is uniform for all Arduino Yun. You can learn how to build an object with following this tutorial:
[http://openhybrid.org/arduino-yun-example.html](http://openhybrid.org/arduino-yun-example.html)

## How is the data communicates between each Hybrid Object working? 
Your iPhone loads the user interface in form of an HTML page from the Object. It also loads a data set from which it renders the IO-Points. When you connect two IO Points, the iPhone sends information about this connection to the origin Object. From that moment on, if data on the IO Point of the Origin Object changes it will be send to the other object.

## I am not familiar with AR technology, I saw there are some virtual images shown when the iphone camera aim at the target. So is there some AR modeling code embed in the Reality Editor App?
All of the AR is happening in the background. You do not need any knowledge about this technology. You will learn everything needed with the example: [http://openhybrid.org/arduino-yun-example.html](http://openhybrid.org/arduino-yun-example.html)
Here you can learn how to upload your user interfaces and how to position and scale:
[http://openhybrid.org/adding-web-content.html](http://openhybrid.org/adding-web-content.html)


## The laptop is used to create Web based AR content for the Reality Editor. Then when I manipulate the hybrid objects with Reality Editor, does the laptop still need to work as a server?
All the Server activity is happening on the Arduino Yun. You do not need an extra external computer. You use your laptop to generate the UI and then you upload it in to the object.



