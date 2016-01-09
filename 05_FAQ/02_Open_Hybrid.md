Q : Why cant i see my I/O points after addding them in arduino code , but i can see the slider ?

A: The I/O points are added to the HybridObject using the .add([Hybrid Object], [IO Point]) 
   function which is defined in the HybridObject library  which can be downloaded from 
   ( http://openhybrid.org/download.html ) and placed in the Arduino IDE library folder

   Now if you have named your interface as “ slider” as shown in the videos here 
   http://openhybrid.org/adding-web-content.html , then to add an I/O point to the
   HybridObject called “slider” , 
   you use .add( “slider” ,”your_I/O_point_name”) , This will add your I/O point to the specified object

