Talker.js wraps `postMessage` in an easy-to-use API. It handles queueing your messages until it completes a handshake so it knows both sides are ready, it filters out messages that aren't from Talker on the other side, and it is built on [JavaScript Promises](https://kerricklong.com/talks/javascript-promises-thinking-sync-in-an-async-world.html), so you can send a request and receive a response.

## Using Talker.js

The `Talker` constructor takes a `window`, and an origin (or `'*'` to accept messages from any origin).

```
var talker = new Talker(myFrame.contentWindow, 'http://example.com/');
```

### Sending Messages

Use `Talker#send` to send a message to the other side. Messages have a namespace for organization, and can have an object sent for data transfer. The object must be able to pass through `JSON.stringify`.

```
talker.send('myNamespace', { data: 'here' });
```

### Listening for Messages

Talker will call `Talker#onMessage` with a `Talker.IncomingMessage`. That message has properties for the `namespace` and `data` it was originally sent with, as well as an `id` and a reference to its `talker`.

```
talker.onMessage = function(message) {
  console.log(message.namespace, message.data);
  console.log(message.id, message.talker);
};
```

### Responding to Messages

Use `Talker.IncomingMessage#respond` to respond to a message with an object. This returns a promise via [PinkySwear.js](https://github.com/timjansen/PinkySwear.js) that may resolve with a response if one is sent, or may reject with an error.

```
talker.onMessage = function(message) {
  message.respond({ hello: 'there' });
};

talker.send('localStorage', { get: 'username' })
  .then(function(message) {
    console.log(message.namespace, message.data);
    console.log(message.id, message.talker);
  }, function(error) {
    console.error(error);
  })
;
```

### Getting Talker

Talker.js distributions are available via [Bower](http://bower.io/search/?q=talkerjs) and [GitHub](https://github.com/secondstreet/talker.js/releases). The source is also [on GitHub](https://github.com/secondstreet/talker.js). Talker is available as a global, a named or anonymous AMD package, or a Common JS package.

```
$ bower install talkerjs --save
```

If you'd like to contribute, please [Fork us on GitHub](https://github.com/secondstreet/talker.js), or [file an issue](https://github.com/secondstreet/talker.js/issues/new) with any bug reports or feature requests.
