### What are all these buttons?
There are two different modes to interact with your Hybrid Object.

For one there is the Augmented User Interface Mode. It has the following Icon in the Editor: 

<img src="http://forum.openhybrid.org/uploads/default/original/1X/57780fd6b11c29fdc7eae73ed064ad40ac1daf9f.jpg" width="60" height="70">

This is where you can generate HTML user interfaces for, that you can interact with. 
Like a slider. This interface is what you create with the html webpage shown in your screenshot above.
**MAKE SURE THAT YOU REFERENCE OBJECT.JS IN YOUR HTML FILE, OTHERWISE NOTHING SHOWS UP**

The other function is the programming. It has the following icon:

<img src="http://forum.openhybrid.org/uploads/default/original/1X/eb4c3b3bc844916e03762ea428f15dacd37573f3.jpg" width="60" height="70">

The programming is where your IO-Points show up.
These IO-Points are automatically generated. The generation is based on what IO-Points you define in your Arduino Code. You can use the Reality Editor to position them rightfully.

The IO-Points will allow you to connect objects or IO-Points with each other.
If you want to build simple interfaces, the entire visual appearance of the programming interface is something you have no influence. It will just work for you.


### I can only find the iOS version of the Reality Editor. What about Android and Windows?

At this point we only compiled the Reality Editor for iOS. The Reality Editor code is based on C++ and Javascript. We use OpenFrameworks for staying platform independent and WebKit to provide you with the Javascript based Interface. There are  problems compiling vuforia together with OpenFrameworks for android. If you want to help solving this problem, please support julapy with his [ofxQCAR project](https://github.com/julapy/ofxQCAR).


### Does the reality editor work with/require objects manufactured with the capability of receiving input from the Reality Editor?

The Reality Editor works with this open hybrid platform. Right now openhybrid works with the arduino Yun.
@Carsten has build Hue Light hardwareInterface and @V_Mohammed_Ibrahim created a tutorial on how to run the platform on the raspberry pi.


### What language do you predominantly used in the development of Reality Editor.
Open Hybrid and big parts of the Reality Editor are written with javascript and its server runtime version node.js.


### What is the HRQR code on the objects found in many of the examples? Is this some sort of requirement to pick of wireless transmissions sent by the application?

The [HRQR](http://www.hrqr.org) like stickers that you can see in the videos are not required.
The Reality Editor uses so called natural features to see what object is in front of it.
It can be any kind of image and it is nothing special. It only needs to have some sort of detail.
You can read more about the details for the tracker here: [https://developer.vuforia.com/library/articles/Solution/Natural-Features-and-Ratings](https://developer.vuforia.com/library/articles/Solution/Natural-Features-and-Ratings). The data connection is happening via wifi.
