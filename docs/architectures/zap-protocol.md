# ZAPP (Zap Protocol)

Zap defines a new network protocol over UDP to facilitate data exchange in an agreed-upon format across various platforms. This protocol is referred to as ZAPP(Zap Protocol), and the ZAPP data contained in a single datagram is called a 'ZAPP Object'.

The ZAPP Object is divided into two main parts: the header and the payload. The header contains metadata about the ZAPP Object, while the payload represents the actual data of interest to the communicating parties.

![](https://user-images.githubusercontent.com/6410412/284769831-6a80cd39-fb13-41b8-8db7-fd135a4e1aab.svg)

The header consists of a timestamp and a resource field. The first 64 bits of the header constitute the timestamp field, representing the epoch time in milliseconds when the ZAPP Object was created. As UDP does not guarantee the order of datagrams, this field can be utilized if the order of data is crucial. This field can be interpreted as an unsigned integer.

Following the timestamp, the next 8 bits constitute the resource field. The resource field informs which resource the payload represents and simultaneously implies how the payload is encoded. This field can be interpreted as an unsigned integer.

After the 72-bit header, there is the payload part. The format of the payload varies depending on the resource. It can be a simple primitive type, text with delimiters, or JSON. For example, the payload of acceleration data is encoded with values measured along the x, y, and z axes, each taking up 32 bits, totaling 96 bits.

![](https://user-images.githubusercontent.com/6410412/284768903-f53e497f-9f88-404a-b5a7-fa8a14d2f877.svg)

Refer to the [Resources](../specifications/resources.md) page for information on the payload format each resource takes. The maximum length of the payload is theoretically up to 65,518 bytes, although it may vary depending on the implementation of Zap server.
