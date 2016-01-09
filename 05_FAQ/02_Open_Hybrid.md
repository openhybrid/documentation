# Why can't i see my I/O points after addding them in arduino code , but i can see the slider ?
The I/O points are added to the HybridObject using the **.add([Hybrid Object], [IO Point])**
function which is defined in the HybridObject library  which can be downloaded [here](http://openhybrid.org/download.html)
and has to be placed in the Arduino IDE library folder. For example if you have named your interface "slider" as shown in the videos [here](http://openhybrid.org/adding-web-content.html) , then to add an I/O point to the HybridObject called "slider", 
you use **.add( "slider", "your_I/O_point_name")**. This will add your I/O point to the specified object.

