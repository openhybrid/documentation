# Introduction

Welcome to the OpenHybrid developer community!

This introductory section will provide you with a short overview of the basic concepts you should know before you start
to implement your own hardware interface.


## What is a HybridObject?
A HybridObject is a physical object which is augmented by a virtual object. It consists of so called **IOPoints** which can send and receive data.
IOPoints can be interconnected so that one IOPoint's output can be used as input for a connected IOPoint. Additionally a HybridObject has an
user interface. To learn more about this read: http://documentation.openhybrid.org/Design_Guidelines (NOTE: nothing in this chapter yet)

## What is a hardware interface?

A hardware interface is the bridge or connector between your hardware and the OpenHybrid system. It maps input and output values and events of your hardware
into the OpenHybrid system. Let's take a simple lamp as example. Assuming the lamp is reachable via some protocol and you want to expose its status - if it is
on or off - as IOPoint. Then your hardware interface must communicate with the lamp via the protocol and notify the OpenHybrid server whenever the lamp is
switched on or off. On the other hand you must relay the on/off information you receive on the status IOPoint from the OpenHybrid server to the lamp and turn 
it on or off.

## Follow the design guidelines!

When you implement your hardware interface you should follow the design guidelines which are explained in detail 
[here](http://documentation.openhybrid.org/Design_Guidelines) (NOTE: nothing there yet)
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

## addIO(objName, ioName, plugin, type)

With this function you can add an IOPoint to your HybridObject.

* **objName** is a string value containing the name of the HybridObject
* **ioName** is a string value containing the name of the IOPoint
* **plugin** is a string value containing the  datapoint interface to use. If you don't have your own datapoint interface use "default"
* **type** is a string value containing the name of your hardware interface. It must be the same as the name of the folder containing your hardware
interface code

## clearIO(type)

This function clears all HybridObjects of the specified type. It removes IOPoints which are not longer needed.

* **type** is a string value containing the name of your hardware interface. It must be the same as the name of the folder containing your hardware
interface code

## developerOn()

This function enables the developer functions which allow you to move IOPoints and the WebUI with the RealityEditor.

## getDebug()

Returns **true** if debug mode is active, **false** otherwise.

# Creating a new hardware interface

1. Create a new folder for your hardware interface in the **hardwareInterfaces** folder of your OpenHybrid installation. In the following we assume
you created a folder **MyHWInterface**
2. Create the file **index.js** in the folder **MyHWInterface**
3. Inside **index.js** implement the following functions (take a look at the [empty example](https://github.com/openhybrid/object/blob/beta_hardwareInterfaces/hardwareInterfaces/emptyExample/index.js)):
  * **exports.enabled**  
    Set to **true** to enable your hardware interface. If set to **false** it will be ignored by the OpenHybrid server which means the server never calls
    the exported functions. You should enclose all your code in an if-statement which checks if the hardware interface is enabled or not:
    
```
exports.enabled = false;
if (exports.enabled) {
    var server = require(__dirname + '/../../libraries/HybridObjectsHardwareInterfaces');
    //Your code
}
```

  * **exports.init()**  
    This function is called repeatedly by the server. Place your calls to **addIO()** and **clearIO()** inside this function.

```
exports.init = function(){
   server.addIO("MyHybridObject","MyIOPoint","default", "MyHWInterface");
   server.addIO("MyHybridObject","MyIOPoint","default", "MyHWInterface");
   server.clearIO("MyHWInterface");
}
```

  * **exports.receive**
  * **exports.send**
  * **exports.shutdown**

# Example implementation

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```
