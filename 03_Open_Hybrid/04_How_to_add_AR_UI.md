## I only want to receive value from the server. How can i do this?

You need to send a  obj.readRequest("`<your IOpoint>`");
If you want it to be updating in realtime, then you need to but the read Request in a loop.
Thats what happening with 

     setInterval(function () {

      }, 50);
Watch out that you have a delay (50) between the requests so that the object server is not overwhelmed.

Once the object sees the read request, it will send back the requested IO-point value.

    obj.object.on("object", function (msg) {
            var data = JSON.parse(msg)
            if (obj.read("<your IOpoint>", data)) {
                slider.value = obj.read("<your IOpoint>", data) * 255;
            }
        });

The code above is listening (obj.object.on) for incoming data. When the object sends back the data, this function is called.

Replace `<your IOpoint>` with the name of your IO-Point.