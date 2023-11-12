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
| open function | `onAccelerometerChanged(id: string, x: float, y: float, z: float): void` | A callback function called whenever accelerometer sensor data is received. |
| open function | `onUIEventReceived(id: string, ui_id: string, event: ZapUiEvent.Event, value: string?): void` | A callback function called whenever UI event data is received. |
| open function | `onTextReceived(id: string, str: string, charset): void` | A callback function called whenever text data is received.|

The server implementation, upon receiving a datagram, MUST reference the `ZapHeader` to identify the resource and then invoke the corresponding callback function for that resource. For detailed specifications on supported resources, please refer to [Resources](./resources.md) section.

It is RECOMMENDED to implement throwing a "Not yet implemented" exception if the callback function is not defined and the corresponding resource data is received.
