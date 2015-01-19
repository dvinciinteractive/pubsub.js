# pubsub.js
A lightweight publish/subscribe server &amp; client written in javascript.

The publish/subscribe model is perfect for making your web application appear real-time. Imagine several people all looking at the same page, when someone makes a change to the page it's instantly reflected to all other viewers of that page. This is `pubsub.js`!  

A `channel` is a group of subscribers all listening for publish, join, & leave events. Channels are created when needed and are destroyed when there are no subscribers.  
  
`pubsub.js` is built with safety in mind. All data sent to the server is validated before it creates channels, subscribes clients, or publishes to a channel.  

### Installing

```
npm install
```

### Running the Server

```
node pubsub-server.js
```

### Client Code

```js
var pubsub = new PubSub('http://localhost:3000');
var channel = pubsub.subscribe('channel ID', 'user token');
channel.onpublish = function(data) {
  console.log('data received:', data);
};
channel.onjoin = function(userToken) {
  console.log('user join:', userToken);
};
channel.onleave = function(userToken) {
  console.log('user leave:', userToken);
};
channel.publish('Hello World!');
channel.unsubscribe();
```

*(see examples/chat.html for a chat demo)*

### Configuration (config.js)

```js
{
  /**
   * The port the application listens on.
   */
  port: 3000,
  
  /**
   * Whether or not to send a publish message back to the client who sent it.
   */
  echoPublish: false,
  
  /**
   * If this server should notify clients when other clients have joined or left
   * a channel. The client's token it used to subscribe is sent along with these
   * notifications so the client can identify them. If this is set to false
   * then tokens are ignored altogether in the system. TODO
   */
  sendJoinLeaveEvents: true,
  
  /**
   * Whether or not to send all of the join tokens of current clients to the 
   * new client when they first join the channel.
   */
  sendExistingClientsOnJoin: true,
  
  /**
   * Requires that a client must be subscribed to a channel before they can publish in it.
   */
  requireSubscription: true,
  
  /**
   * Logs events when true.
   */
  debug: true,
  
  /****************************************************************************
   *                C H A N N E L    I D    V A L I D A T I O N
   ****************************************************************************/
  
  /**
   * The data types that are valid channel IDs.
   * If the id is found not to be valid, a channel will not be created.
   */
  validIds: 
  {
    'number':     true,
    'string':     true,
    'boolean':     true,
    'object':     false,
    'undefined':   false
  },
  
  /**
   * A function which does further validation on a channel ID.
   * If the id is found not to be valid, a channel will not be created.
   */
  validateId: function(id)
  {
    return true;
  },
  
  /****************************************************************************
   *             C L I E N T    T O K E N    V A L I D A T I O N
   ****************************************************************************/
  
  /**
   * The data types that are valid client tokens.
   * If a token is found to be not valid, the user does not join the channel.
   */
  validTokens:
  {
    'number':     true,
    'string':     true,
    'boolean':     true,
    'undefined':   true,
    'object':     false
  },
  
  /**
   * A function which does further validation on a client token.
   * If a token is found to be not valid, the user does not join the channel.  
   */
  validateToken: function(token)
  {
    return true;
  },
  
  /****************************************************************************
   *                 P U B L I S H    V A L I D A T I O N
   ****************************************************************************/
  
  /**
   * The data types that are valid to publish to other clients.
   */
  validPublish:
  {
    'number':     true,
    'string':     true,
    'boolean':     true,
    'undefined':   true,
    'object':     true
  },
  
  /**
   * A function which validates if a message by a client can be published on a channel.
   */
  validatePublish: function(message, client, channel)
  {
    return true;
  }
  
}
```
