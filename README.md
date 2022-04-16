# @buccaneerai/rxjs-ws
> 🔌 RxJS operators for working with WebSockets

## Installation
This is a private package. It requires setting up access in your npm config.

```bash
yarn add @buccaneerai/rxjs-ws
```

## API

### `conduit`
Opening a two-way WebSocket is a very common software pattern.  The `conduit` operator makes it trivially easy to do this.  It pipes an Observable into a two-way websocket. Each item in the Observable will be published to the server. The output stream will be the messages sent from the server to the client.  

Optionally, it can be performed using a custom serializer or deserializer -- otherwise, it will assume the messages are encoded/decoded as JSON and transmitted as JSON strings.

#### Basic usage
```javascript
import { from } from 'rxjs';
import { conduit } from '@buccaneerai/rxjs-ws';

const messagesToSend$ = from([
  {body: 'data'},
  {body: 'more data'},
]);
const socketResponse$ = messageToSend$.pipe(
  conduit({url: 'wss://mysite.com'})
);

socketResponse$.subscribe(); // this will attempt to send the messages to the server
```

#### With error handling
```javascript
import { from } from 'rxjs';
import { conduit } from '@buccaneerai/rxjs-ws';

const messagesToSend$ = from([
  {body: 'data'},
  {body: 'more data'},
]);
const socketResponse$ = messageToSend$.pipe(
  conduit({url: 'wss://mysite.com'})
);

socketResponse$.subscribe(); // this will attempt to send the messages to the server
socketResponse$.error$.subscribe(); // returns WebSocket errors
```

#### Custom serialization
```javascript
import { from } from 'rxjs';
import { conduit } from '@buccaneerai/rxjs-ws';

const decodeMessage = base64Message => atob(base64Message);
const encodeMessage = binaryString => btoa(binartString);

const messagesToSend$ = from([
  'somebinarystring',
  'anotherbinarystring',
]);
const socketResponse$ = messageToSend$.pipe(
  conduit({
    url: 'wss://mysite.com',
    serializer: encodeMessage,
    deserializer: decodeMessage,
  })
);
socketResponse$.subscribe();
```

## Contributing, Deployments, etc.
See [CONTRIBUTING.md](https://github.com/buccaneerai/rxjs-ws/blob/master/docs/CONTRIBUTING.md) file for information about deployments, etc.
