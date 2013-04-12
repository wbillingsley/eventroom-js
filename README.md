# eventroom-js

EventRoom is javascript code for handling a connection to an EventRoom on the server, using Server Sent Events or Websockets.

    var er = EventRoom.create()
    er.connectSSE()
    
Will create a Server Sent Events connection to `/eventRoom/ssevents`.  The URL can be configured.

EventRoom expects to receive an initial event over this new connection to the server containing the following to give the eventroom its "listenerName" (the key it needs to send with subscription requests).

    {
      "type": "connected",
      "listenerName": *listenerName*
    }

### Listening to the EventRoom

To register a listener to the client-side EventRoom, call:

    er.addListener(myListener)
    
A listener is any object that has a `receive(msg)` function.

    er.subscribe({ type: 'foo', id: '123' })
    
Will cause the EventRoom to send a POST request to `/eventRoom/subsctibe` containing

    {
       "listenerName": *listenerName*,
       "subscription": { "type": "foo", "id": "123" }
    }

The server-side should use this to subscribe the listener to the additional channel.


### iframes sharing their parent's connection

If `EventRoom.create()` is called from within an iframe, it will attempt to look for an EventRoom in the parent page and connect using that. It will share the connection if:

1. It finds EventRoom in the parent page, and
2. The EventRoom in the parent page had the same `param` value. (As connection URLs might depend on this param value.)

