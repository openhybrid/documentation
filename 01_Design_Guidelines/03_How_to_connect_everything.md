# How to connect the functionality of electronic devices

Traditionally, you would create some kind of standard that knows every possible representation of the relevant objects so that every interface can be defined. For example, say you have two objects, a toaster and a food processor, and now you would need to create a standard that knows how to connect these two objects.


![](http://openhybrid.org/images/picture16.jpg)

With Open Hybrid you have a visual representation of your object’s functionalities augmented onto the physical object. Where before an abstract standard needed to be created, you can now just visually break down an object to all its components.
The toaster now consists of a heating element, a setup button, a push slider, and a timing rotation dial. All of these elements are represented with just a simple number between 0.0 and 1.0. This same simple representation applies to the food processor.
Visually broken down, the food processor consist of a motor and a 4 step dial.


![](http://openhybrid.org/images/picture17.jpg)

If you want to connect two objects with Open Hybrid, you are really only connecting the numbers associated with a given object, never objects themselves. For example, if you need a timer for your food processor, just borrow the functionality from your toaster. Connect the toaster’s push slider to the timer dial, the timer dial to the 4 step dial of the food processor and finally the dial to the motor. If you now push down the push slider on the toaster, the timer gets started. The signal from the timer gets scaled by the 3 step dial and eventually drives the motor. Once the timer runs out, the food processor stops.


![](http://openhybrid.org/images/picture181.jpg)

This is the power of Open Hybrid. Now that the interface allows you break down every object to its components, you only need to deal with the smallest entity of a message: a number. As such, Open Hybrid is compatible with every Hybrid Object that has been created, and any object that will be built.