socket-io-demo
==============

This is a (very) slightly modified version of the demo at http://socket.io/get-started/chat/

## Prerequisites:
Install node.js and npm. DBC machines should have these already

## To use:
-  In a terminal, clone this repo
-  cd into the socket-io-demo directory thus created
-  In the terminal type "npm install" and hit enter. This will download and install the two dependencies listed in package.json - express and socket.io. You may also see other packages installed installed - these are the dependencies listed by express and socket (and theirs in turn etc.) Listing packages in package.json and running npm install is analagous to listing gems in a Gemfile and running bundle.
-  In that same window type node server.js. This should start the server on port 3000 - you'll hopefully see "listening on *:3000"
-  Open safari and point it to http://localhost:3000
-  Open another browser and point it too to http://localhost:3000. Start typing in the text field at the bottom of either browser window. You should see the pages in both browser windows update with the contents of the text field after each "key up" event.

## What's going on?
server.js  contains this block:

```
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    io.emit('remote chat message', msg);
    console.log('message: ' + msg);
  });
});
```
That sets up an event handler that socket.io will execute every time a client connects. The handler in turn sets up a second handler which will fire each time a "chat message" event happens on the socket. That inner handler handler calls "io.emit" to send that event back to all connected clients.

On the client side a jQuery document.ready sets things up.

```
      var socket = io();

      $('#m').keyup(function(){
        socket.emit('chat message', $('#m').val());
      });

      socket.on('chat message', function(msg){
       $('#messages').append($('<li>').text(msg));
      });
    });


```

var socket=io() sets up the connection to the socket on the server. They keyup event handler tells the socket to send the value of the text field as a "chat message" event. The "socket.on" handler sets up a handler to execute each time a chat message event comes in from the server. These events can have any name of course - it's just a label.

So when I hit a key in the text field these events happen:
- keyup handler sends an event to the socket.
- server-side socket.on('chat message'...) event handler executes.
- That server-side handler calls io.emit to send the message to all clients.
- The client-side "socket.on" gets that event and calls the attached handler which appends an li element to the ul id="messages" element in the DOM.






