## Version 1.6.0
* **Improvement**: The GUI renders 29% faster in iOS 8 and higher due a switch from UIWebView to WKWebView.
* **Improvement**: UIs are only transformed if the transformation matrix has changed. Saves Javascript processing time.
* **Improvement**: Lines show the perspective correct size according to the depth.
* **Improvement:** Improved accuracy for 3d transformation. In this version improvement is calibrated for iPhone 5, 5s, 6, 6s+, iPad 2, 3, 4, mini 2G. More devices will follow.
* **New Feature:** Unconstrained Editing allows the free positioning of UI's in 3D Space.
* **New Feature:** Editing mode shows now, if the UI is visually behind the marker.
* **New Feature:** Reset Button allows to put a UI back to the default position.
* **New Feature:** send and receive global broadcasting messages among all UI's. .addGlobalMessageListener(callback(e)) and .sendGlobalMessage(message);
* **New Feature:** UI's can now render full screen and subscribe to the 3D position relative to the marker. This allows the support for WebGL.
* **New Feature:** A touch Overlay indicates where the finger touches the display and provides guidance when connecting IO-Points
* **New Feature:** IO-Points are now responsive. The Reality Editor provides states.
* **New Feature:** Lines between IO-Points are now animated dot lines, that indicate the direction of a signal flow.
* **New Feature:** The GUI buttons are redesigned for more visible space.
* **New Feature:** New super fast startup. Up to 50 most used targets are permanently stored in the device.
* **New Feature:** The Reality Editor automatically checks all target files and updates target files if they have changed on the server.
* **Bugfix:** More responsive checkboxes for the settings menu.
* **Bugfix:** UI's are now sandboxed. It prevents the UI from loading web content on the GUI Interface level.
* **Bugfix:** Downloader registered multiple instances of an object.



## Version 1.5.7
* **Bugfix:** Fixed scaling bug with extra resolution retina displays such as the iPhone 6+

## Version 1.5.6
* **Bugfix:** Fixed an issue where non Retina devices had a wrong GUI scale.
* **Info:(This version has an error on all iOS6+ Devices.)

## Version 1.5.5 
* **Bugfix:** The interface was wrong positioned on some iOS devices. This has been fixed.
* **Compatibility: The minimal required iOS version is now 7.0. Use 7.0 only if you have no other option. 


## Version 1.5.3 
* The only change to Version 1.5.2 is the compatibility with some older iOS devices that support 32bit applications only. You need a minimum iOS Version of iOS 8 to run the Reality Editor.


## Version 1.5.2 
* **New Feature:** The Image Tracker has been updated to Vuforia 5.0.5
* **New Feature:** Enable Extended Tracking in the Preferences
* **New Feature:** Clear Sky mode for presentations without GUI
Unlike in the normal mode, web interfaces are not destroyed after 3 seconds of invisibility.
This keeps your demo instantly responsive, but might slow down when interacting with many objects.
* **New Feature:** Load a modified version of the Web Based GUI from your own Web-Server. Use [https://github.com/openhybrid/editor](https://github.com/openhybrid/editor) as a reference for your own modifications. The Reality Editor will automatically return to the old GUI if the code at the destination URL is not responding. You can also reset to the old GUI by putting your device in airplane mode and then start the Reality Editor.
* **Info:** Preferences redesign and new Buttons
* **New Feature:** The App keeps all states after reload.


## Version 0.1.1
* **Info:** Renamed from Hybrid Editor to Reality Editor
* **Bugfix:** Small design fixes
* **Bugfix:** Small bug fixes in the web based GUI

## Version 0.1
* **Info:** initial release