# Demo: Socket.io

This is effectively the **SAME** server that we built in class 17, refactored to use `socket.io` instead of raw TCP. This is a chance to not only talk through the advantages of web sockets, but also revisit the multi-service architecture that we're building.

## Live Coding Demo(s)
These are designed to work in concert. The server acts as controller, hearing all inbound events and pushing out subsequent events to clients that care.  The app.js pumps a bunch of events **into** the server, while 'some-other-app.js' simply sits there and hears messages as they arrive.  You could theoretically create a system with 10 'some-other-app.js' type of apps that care about different events, and have app.js pump out random events (instead of always 'speak') to simulate a busy system.

Build this up slowly, with just server and app, and have app hear events (even though they're just responses to it's own events) from the server.  Have the server send directed events.  Then bring in 'some-other-app' to show how you can have many other clients connecting in.

You can even open up https://amritb.github.io/socketio-client-tool and connect it to your server as well. If you use it to emit events your app should show them logging out as well.  Note -- that only works against localhost OR an SSL server

### First Pass - "simple-server"
Build out a simple server that represents a single pool of connections and events with socket.io.

Things to point out
  * Clients will automatically reconnect and re-bind events after a disconnect (server restart, etc)
  * Socket.io is managing all of the events, headers, and client pools

#### Server -- `server.js`
* Sets up a listener on port 3000
* On each client's connect, it sets a listener on "speak"
* On speak, it emits a "message" event

#### Client -- `app.js`
* Connects to the server
* Every 750ms, emits a 'speak' event.
* This simulates a database api server that is notifying the world when a record gets saved

#### Logger -- `logger.js`
* Connects to the server
* Has a listener on 'message' events, which logs out the contents.

Once all of the above demos are built up, running and well understood, talk with the students about Socket.io support for **namespaces** and **rooms**, which allow you to segment your connections (even within the same client)

### Second Pass: Add Namespaces and Rooms

Demonstrate the power of socket.io and it's ability to allow you to connect not only to the global pool, but to 2 layers of sub-pools (namespaces) and sub-sub pools (rooms within namespaces)

#### `server.js`
* Add support for 2 namespaces, and within one of them an event ('join') with a room.
* Handle a new event just for that room

#### `app.js`
* Join the 2 new namespaces from the server
* Push events to them both

#### `school-logger`, `home-logger`, `codefellows-logger`
* New apps that look like the main logger, but with some exceptions
* They each connect to either the school or home namespace
* The codefellows one only connects to the codefellows room in the school namespace

When you run the app, it'll fire additional events. Each of the loggers should see different info:

* logger.js - sees events on the main connection (CORE) only
* home-logger.js - sees events in the "home" namespace only
* school-logger.js - sees events in the "school" namespace only (will not see events in any rooms in the school namespace, however)
* codefellows-logger.js - Sees school namespace events AND codefellows room events.
