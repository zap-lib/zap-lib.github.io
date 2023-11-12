# Models

## `ZapDatagram`

A protocol defined on top of datagrams for client and server to exchange `Zapable` data.

| type | signature | description |
|------|-----------|-------------|
| property | `header: ZapHeader` | A header part. |
| property | `payload: ZapPayload` | A payload part. |
| function | `toString(): string` | Compose a string to send with the header and payload. The composed string MUST be formatted to `header;payload`. |
| function | `from(str: string): ZapDatagram` | Convert the given string in received datagram to `ZapDatagram`. <hr> `str`: A string to convert that MUST be formatted as `header;payload`. |

### `ZapHeader`

| type | signature | description |
|------|-----------|-------------|
| property | `id: string` | An ID of the client. |
| property | `resource: ZapResource` | A resource type of the payload. It indicates a format of payload. |
| function | `toString(): string` | Convert `ZapHeader` to string and return it. It MUST be formatted as `id,resource`. |

### `ZapPayload`

A typealias for string type.

## `interface Zapable`

An interface for data exchange through Zap. Data objects exchanged via Zap MUST implement this interface. If an object can be transmitted through Zap, it can be referred to as "Zapable".

| type | signature | description |
|------|-----------|-------------|
| property | `resource: ZapResource` | A resource type of the object. |
| function | `toPayload(): ZapPayload` | Convert `Zapable` to `ZapPayload` and return it. |
| static function | `from(obj: ZapPayload): Zapable` | Converts and returns `Zapable` object from `ZapPayload`. If possible, it is RECOMMENDED to separate it into `interface DeZapable` to enforce implementation. <hr> `obj`: A payload object to convert to `Zapable` object.  |
