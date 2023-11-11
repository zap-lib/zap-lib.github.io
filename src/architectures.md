# Architectures

Zap is structured as a client-server model. The client unidirectionally sends data, and the server receives and processes it. This architecture keeps Zap's design simple and naturally allows a 1:N structure where multiple clients can send data to a single server.

![](https://user-images.githubusercontent.com/6410412/282240267-7f0fa79c-76b0-444c-9f9f-e99a17fcea51.png)

A data sent from the client to the server is transmitted over UDP socket. Zap is implemented to send this data by defining `ZapDatagram` defined on top of datagram, which contain the client's UUID, resource type, and the actual data value.

Zap uses callback functions to enable the transformation of a single-device application into a multi-device application while maintaining its own structure. The client, after obtaining the data they wish to transmit through the data source access API provided by their development framework, simply needs to call `zap.send(...)`. The server, running a Zap server instance, only needs to define callback functions to specify what code to execute each time it receives data from the client.

The type structure is straightforward. Under the root, there are four main modules: `ZapClient`, `ZapServer`, `Models`, and `Resources`.

![](https://user-images.githubusercontent.com/6410412/282253824-54bf0d8d-ca86-4374-a458-3677cf575aa0.png)

`Models` include interfaces and type definitions generally referenced throughout the Zap system. On the other hand, `Resources` consist of implementations of `Zapable` that can be transmitted via Zap. For example, `ZapAccelerometer` is a `Zapable` implementation representing accelerometer data. These resource implementations are referenced to abstract the data obtained from data sources in `ZapClient` or `ZapServer` and are encapsulated in `ZapDatagram` objects for network transmission. For more detailed information on each component, please read the [Specifications](./specifications/README.md) section.
