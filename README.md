# @rxtk/ws
> 🔌 RxJS operators for working with WebSockets

```bash
yarn add @rxtk/ws
```

## API

### `conduit`
Opening a two-way WebSocket is a very common software pattern.  The `conduit` operator makes it trivially easy to do this.  It pipes an Observable into a two-way websocket. Each item in the input Observable will be published to the server. The output Observable will be the messages sent from the server to the client.

Optionally, it can be performed using a custom serializer or deserializer -- otherwise, it will assume the messages are encoded/decoded as JSON and transmitted as JSON strings.

The conduit also supports error handling and many disconnection scenarios (see examples below).

#### Basic usage
```javascript
import { from } from 'rxjs';
import { conduit } from '@rxtk/ws';

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
import { conduit } from '@rxtk/ws';

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
import { conduit } from '@rxtk/ws';

const decodeMessage = base64Message => atob(base64Message);
const encodeMessage = binaryString => btoa(binaryString);

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
