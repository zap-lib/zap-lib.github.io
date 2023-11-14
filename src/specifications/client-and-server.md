# Client and Server

## `ZapClient`

A client sends data to server.

| type | signature | description |
|------|-----------|-------------|
| property | `server_address` | An IP address of the device running `ZapServer`. |
| function | `send(obj: Zapable): void` | Send given `Zapable` object to the server. <hr> `obj`: An object to send. |
| function | `stop(): void` | Close the socket. |

## `ZapServer`

A server receives data from client. The 'open function' refers to a callback function that users SHOULD override when declaring a ZapServer object to define its behavior. Refer to the [Resources](./resources.md) section for information on the parameters of each open function.

| type | signature | description |
|------|-----------|-------------|
| function | `listen(port: int): void` | Start listening the transmitted data from clients on the given port. <hr> `port`: A port number for receiving data (default: `65500`).  |
| function | `stop(): void` | Stop listening to clients. |
| open function | `onAccelerometerChanged(info: MetaInfo, data: ZapAccelerometer): void` | A callback function called whenever accelerometer sensor data is received. |
| open function | `onUIEventReceived(info: MetaInfo, data: ZapUiEvent): void` | A callback function called whenever UI event data is received. |
| open function | `onTextReceived(info: MetaInfo, data: ZapText): void` | A callback function called whenever text data is received.|

The server implementation, upon receiving a datagram, MUST reference the `ZappHeader` to identify the resource and then invoke the corresponding callback function for that resource. For detailed specifications on supported resources, please refer to [Resources](./resources.md) section.

It is RECOMMENDED to implement throwing a "Not yet implemented" exception if the callback function is not defined and the corresponding resource data is received.

### `MetaInfo`

| type | signature | description |
|------|-----------|-------------|
| property | `header: ZappHeader` | ZAPP header object. |
| property | `dgram` | A framework-specific type that contains datagram information such as address and port. |
