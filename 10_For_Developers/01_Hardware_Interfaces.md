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

# Creating a new hardware interface

Description of how to implement a new hardware interface

# Example implementation

```

//Enable this hardware interface
exports.enabled = false;

if (exports.enabled) {
    var fs = require('fs');
    var mpd = require('mpd');
    var _ = require('lodash');
    var server = require(__dirname + '/../../libraries/HybridObjectsHardwareInterfaces');

    var mpdServers = JSON.parse(fs.readFileSync(__dirname + "/config.json", "utf8"));
    var cmd = mpd.cmd;


    /**
     * @desc setup() runs once, adds and clears the IO points
     **/
    function setup() {
        server.developerOn();

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
                        server.writeIOToServer(key, "volume", status.volume / 100, "f");
                    }

                });

            });


            //playing status has changed
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
        if (server.getDebug()) console.log("Incoming: " + objName + "   " + ioName + "   " + value);
        if (mpdServers.hasOwnProperty(objName)) {
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

```