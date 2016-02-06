# Introduction

Welcome to the OpenHybrid developer community!

This introductory section will provide you with a short overview of the basic concepts you should know before you start
to implement your own hardware interface.


## What is a HybridObject?
A HybridObject is a physical object which is augmented by a virtual object. It consists of so called **IOPoints** which can send and receive data.
IOPoints can be interconnected so that one IOPoint's output can be used as input for a connected IOPoint. Additionally a HybridObject has a web based
user interface. To learn more about this read:[Design_Guidelines]( http://documentation.openhybrid.org/Design_Guidelines) (NOTE: Added basic introduction)

## What is a hardware interface?

A hardware interface is the bridge or connector between your hardware and the OpenHybrid system. It maps input and output values and events of your hardware
into the OpenHybrid system. Let's take a simple lamp as example. Assuming the lamp is reachable via some protocol and you want to expose its status - if it is
on or off - as IOPoint. Then your hardware interface must communicate with the lamp via the protocol and notify the OpenHybrid server whenever the lamp is
switched on or off. On the other hand you must relay the on/off information you receive on the status IOPoint from the OpenHybrid server to the lamp and turn 
it on or off accordingly.

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
3. Inside **index.js** implement the following functions (take a look at the [empty example](https://github.com/openhybrid/object/blob/beta_hardwareInterfaces/hardwareInterfaces/emptyExample/index.js)).
   To use the Hardware Interfaces API reference it with `var server = require(__dirname + '/../../libraries/HybridObjectsHardwareInterfaces');`
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
       server.addIO("MyHybridObject","MyIOPoint2","default", "MyHWInterface");
       server.clearIO("MyHWInterface");
    }
    ```

  * **exports.receive()**  
    This is called once by the server. Start the event loop of your hardware interface here or register to events from your hardware.
    Call **writeIOToServer()** whenever you receive new values from your hardware and you want to send the data to the server.

    ```
    exports.receive = function(){
        //Poll for values
       setInterval(function() {
            //Assuming that readHWValue is some function that get's a value
            //from the hardware. You will have to create your own function that
            //does this.
            var value = readHWValue(); 
       }, 1000);
    }
    ```

  * **exports.send(objName, ioName, value, mode, type)**  
    This function is called by the HybridObject server whenever data for one of your HybridObject's IO points arrives. Parse the data inside
    the function and pass it to your hardware.
    * **objName** is a string value containing the name of the HybridObject
    * **ioName** is a string value containing the name of the IOPoint
    * **value** is the incoming value
    * **mode** is a string value describing the datatype of value
    * **type** is a string value containing the name of your hardware interface. It must be the same as the name of the folder containing your hardware
    interface code

    ```
    exports.send = function(objName, ioName, value, mode, type){
        if (objName == "MyHybridObject" && ioName == "MyIOPoint") {
            //Assuming that writeValueToHW is a function that writes a value to the
            //actual hardware. You will have to create your own function that
            //does this.
            writeValueToHW(value);
        }
    }
    ```
  * **exports.shutdown()**  
    This function is called once by the server when the process is being torn down. Clean up open file handles or resources and return quickly.

    ```
    exports.shutdown = function() {
        //cleanup here
    }
    ```

# Example implementation

The following is a working example. It implements a [Music Player Daemon](http://www.musicpd.org/) hardware interface
and it uses [the mpd package from npmjs.com](https://www.npmjs.com/package/mpd) to communicate with the mpd server.
It implements two IOPoints - status and volume - for each configured mpd server. It requires a configuration file **config.json** which
configures the data needed to connect to a mpd server. For host you can either pass the hostname or ip address of the mpd server:

    {
      "Radio1": {
        "host": "hostname1",
        "id": "Radio1",
        "port": "6600"
      },
    "Radio2": {
        "host": "hostname2",
        "id": "Radio2",
        "port": "6600"
      }
    }

The **index.js**:

    //Enable this hardware interface
    exports.enabled = false;

    //only execute the following code if the hardware interface is enabled
    if (exports.enabled) {
        //require some external libraries
        var fs = require('fs');
        var mpd = require('mpd');
        var _ = require('lodash');
        //require the Hardware Interfaces API
        var server = require(__dirname + '/../../libraries/HybridObjectsHardwareInterfaces');

        //load the config file
        var mpdServers = JSON.parse(fs.readFileSync(__dirname + "/config.json", "utf8"));
        var cmd = mpd.cmd;


        /**
         * @desc setup() runs once, establishes the connection with the mpd server
         *               and adds event handlers
         **/
        function setup() {
            //enable developer mode for the RealityEditor
            server.developerOn();

            //Parse the config data
            for (var key in mpdServers) {
                var mpdServer = mpdServers[key];
                mpdServer.ready = false;

                mpdServer.client = mpd.connect({ port: mpdServer.port, host: mpdServer.host });

                mpdServer.client.on('error', function (err) {
                    console.log("MPD " + mpdServer.id + " " + err);
                });


                //Create listeners for mpd events
                mpdServer.client.on('ready', function () {
                    mpdServer.ready = true;
                });

                //volume has changed
                mpdServer.client.on('system-mixer', function () {
                    mpdServer.client.sendCommand(cmd("status", []), function (err, msg) {
                        if (err) console.log("Error: " + err);
                        else {
                            var status = mpd.parseKeyValueMessage(msg);
                            //Notify the HybridObject server of the volume change
                            //Map the volume [0,100] to [0,1]
                            server.writeIOToServer(key, "volume", status.volume / 100, "f");
                        }

                    });

                });


                //playing status has changed
                //Set the "status" IOPoint to 0 if playback is stopped, to 1 when playing and to 0.5 when paused
                mpdServer.client.on('system-player', function () {
                    mpdServer.client.sendCommand(cmd("status", []), function (err, msg) {
                        if (err) console.log("Error executing mpd command: " + err);
                        else {
                            var status = mpd.parseKeyValueMessage(msg);
                            if (status.state == "stop") {
                                server.writeIOToServer(key, "status", 0, "f");
                            } else if (status.state == "play") {
                                server.writeIOToServer(key, "status", 1, "f");
                            } else if (status.state == "pause") {
                                server.writeIOToServer(key, "status", 0.5, "f");
                            }
                        }

                    });

                });

            }
        }


        exports.receive = function () {
            setup();
        };

        exports.send = function (objName, ioName, value, mode, type) {
            //Debug output if debug mode is enabled
            if (server.getDebug()) console.log("Incoming: " + objName + "   " + ioName + "   " + value);
            if (mpdServers.hasOwnProperty(objName)) {
                //Write incoming data to the mpd server
                if (ioName == "volume") {
                    mpdServers[objName].client.sendCommand("setvol " + _.floor(value * 100), function (err, msg) {
                        if (err) console.log("Error executing mpd command: " + err);
                    });
                } else if (ioName == "status") {
                    if (value < 0.33) {
                        if (mpdServers[objName].ready) {
                            mpdServers[objName].client.sendCommand(cmd("stop", []), function (err, msg) {
                                if (err) console.log("Error executing mpd command: " + err);
                            });
                        }
                    } else if (value < 0.66) {
                        if (mpdServers[objName].ready) {
                            mpdServers[objName].client.sendCommand(cmd("pause", []), function (err, msg) {
                                if (err) console.log("Error executing mpd command: " + err);
                            });
                        }
                    } else {
                        if (mpdServers[objName].ready) {
                            mpdServers[objName].client.sendCommand(cmd("play", []), function (err, msg) {
                                if (err) console.log("Error executing mpd command: " + err);
                            });
                        }
                    }
                }

            }

        };

        exports.init = function () {
            if (server.getDebug()) console.log("mpd init()");
            for (var key in mpdServers) {
                server.addIO(key, "volume", "default", "mpdClient");
                server.addIO(key, "status", "default", "mpdClient");
            }
            server.clearIO("mpdClient");
        };
    }

