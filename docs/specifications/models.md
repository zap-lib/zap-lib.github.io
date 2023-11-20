# Models

> In this section, the term 'bytes' is used to refer to a type representing a sequence of bytes, such as `Buffer` in Node.js and `ByteBuffer` in Java/Kotlin.

## `ZappObject`

An object that represents the encoded bytes on a single datagram.

ZAPP(Zap Protocol) is network protocol defined on the top of UDP datagram for client and server to exchange 'Zapable' data.
For more information about the protocol, read [ZAPP page](../architectures/zap-protocol.md).

| type | signature | description |
|------|-----------|-------------|
| property | `header: ZappHeader` | A header part. |
| property | `payload: ZappPayload` | A payload part. |
| function | `toBytes(): bytes` | Encode `ZappObject` to bytes. The sequence of bytes is MUST encoded as ZAPP Object. |
| static function | `from(b: bytes): ZappObject` | Decode the given bytes in received datagram to `ZapObject`. <hr> `b`: A sequence of bytes to convert that MUST be encoded as ZAPP Object. |

## `ZappHeader`

| type | signature | description |
|------|-----------|-------------|
| property | `timestamp: 64_bit_uint` | An epoch time in milliseconds for creation time of `ZappObject`. (default: Current epoch) |
| property | `resource: ZapResource` | A resource type of the payload. It indicates a format of payload. |
| function | `writeTo(b: bytes): bytes` | Write `ZappHeader` to given `b` and return it. The given `b` MUST be encoded as ZAPP Header. |
| static function | `from(b: bytes): ZappHeader` | Read bytes from the given `b` and decode it to `ZappHeader`. |

## `ZappPayload`

A typealias for bytes.

## `interface Zapable`

An interface for data exchange through Zap. Data objects exchanged via Zap MUST implement this interface. If an object can be transmitted through Zap, it can be referred to as "Zapable".

| type | signature | description |
|------|-----------|-------------|
| property | `resource: ZapResource` | A resource type of the object. |
| function | `toPayload(): ZappPayload` | Encode `Zapable` to `ZappPayload` and return it. |
| static function | `from(payload: ZappPayload): Zapable` | Decode and return `Zapable` object from `ZappPayload`. If possible, it is RECOMMENDED to separate it into `interface DeZapable` to enforce implementation. <hr> `payload`: A payload to decode to `Zapable` object. |
