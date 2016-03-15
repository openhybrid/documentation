## Version 1.6.0
* new version place holder


## Version 1.5.7
* Bugfix: Fixed scaling bug with extra resolution retina displays such as the iPhone 6+

## Version 1.5.6
* Bugfix: Fixed an issue where non Retina devices had a wrong GUI scale.
* (This version has an error on all iOS6+ Devices.)

## Version 1.5.5 
* Bugfix: The interface was wrong positioned on some iOS devices. This has been fixed.
* Compatibility: The minimal required iOS version is now 7.0. Use 7.0 only if you have no other option. 


## Version 1.5.3 
* The only change to Version 1.5.2 is the compatibility with some older iOS devices that support 32bit applications only. You need a minimum iOS Version of iOS 8 to run the Reality Editor.


## Version 1.5.2 
* The Image Tracker has been updated to Vuforia 5.0.5
* New Feature: Enable Extended Tracking in the Preferences
* New Feature: Clear Sky mode for presentations without GUI
Unlike in the normal mode, web interfaces are not destroyed after 3 seconds of invisibility.
This keeps your demo instantly responsive, but might slow down when interacting with many objects.
* New Feature: Load a modified version of the Web Based GUI from your own Web-Server. Use [https://github.com/openhybrid/editor](https://github.com/openhybrid/editor) as a reference for your own modifications. The Reality Editor will automatically return to the old GUI if the code at the destination URL is not responding. You can also reset to the old GUI by putting your device in airplane mode and then start the Reality Editor.
* Preferences redesign and new Buttons
* The App keeps all states after reload.


## Version 0.1.1
* Renamed from Hybrid Editor to Reality Editor
* Small design fixes
* Small bug fixes in the web based GUI

## Version 0.1
* initial release