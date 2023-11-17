# Resources

## `enum ZapResource`

A registry of resources supported by Zap and their identification keys.

| property | key<br>(8-bit uint) | type |
|------|-----------|-------------|
| `ACCELEROMETER` | 10 | `ZapAccelerometer` |
| `GRAVITY` | 11 | `ZapGravity` |
| `GYROSCOPE` | 12 | `ZapGyroscope` |
| `ILLUMINANCE` | 13 | `ZapIlluminance` |
| `MAGNETIC_FIELD` | 14 | `ZapMagneticField` |
| `UI_EVENT` | 20 | `ZapUiEvent` |
| `TEXT` | 30 | `ZapText` |
| `GEO_POINT` | 31 | `ZapGeoPoint` |

## `ZapAccelerometer`

Represent values measured by accelerometer sensor

```text
+-------------+-------------+-------------+
| x (32 bits) | y (32 bits) | z (32 bits) |
+-------------+-------------+-------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `x: float` | Acceleration force along the x axis. (m/s²) |
| property | `y: float` | Acceleration force along the y axis. (m/s²) |
| property | `z: float` | Acceleration force along the z axis. (m/s²) |

## `ZapGravity`

Represent the force of gravity that is applied to a device.

```text
+-------------+-------------+-------------+
| x (32 bits) | y (32 bits) | z (32 bits) |
+-------------+-------------+-------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `x: float` | Force of gravity along the x axis. (m/s²) |
| property | `y: float` | Force of gravity along the y axis. (m/s²) |
| property | `z: float` | Force of gravity along the z axis. (m/s²) |

## `ZapGyroscope`

Represent a device's rate of rotation.

```text
+-------------+-------------+-------------+
| x (32 bits) | y (32 bits) | z (32 bits) |
+-------------+-------------+-------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `x: float` | Rate of rotation around the x axis. (rad/s) |
| property | `y: float` | Rate of rotation around the y axis. (rad/s) |
| property | `z: float` | Rate of rotation around the z axis. (rad/s) |

## `ZapIlluminance`

Represent the ambient light level.

```text
+--------------+
| lx (32 bits) |
+--------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `lx: float` | Ambient light level. (lx) |

## `ZapMagneticField`

Represent a ambient geomagnetic field.

```text
+-------------+-------------+-------------+
| x (32 bits) | y (32 bits) | z (32 bits) |
+-------------+-------------+-------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `x: float` | Geomagnetic field strength along the x axis. (μT) |
| property | `y: float` | Geomagnetic field strength along the y axis. (μT) |
| property | `z: float` | Geomagnetic field strength along the z axis. (μT) |

## `ZapUiEvent`

Represent data related to event raised by the user interface.

| type | signature | description |
|------|-----------|-------------|
| property | `ui_id: string` | An identifier for the UI. |
| property | `event: ZapUiEvent.Event` | A type of event occurring for the UI. |
| property | `value: string?` | A value changed due to the event. (optional) |

### `Event`

| property | value | description |
|------|-----------|-------------|
| `CLICK_DOWN` | `CLICK_DOWN` | An event triggered when a click (or touch) is initiated on the UI. |
| `CLICK_UP` | `CLICK_UP` | An event triggered when a click (or touch) on the UI has ended. |

## `ZapText`

Represent simple text.

```text
+------------------+--------------+
| charset (8 bits) | str (n bits) |
+------------------+--------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `str: string` | Just string. |
| property | `charset: ZapCharset` | A character set of `str`. (default: UTF-8) |

### `ZapCharset`

The character sets.

> Some platforms may support only the limited character sets. In such cases, it is RECOMMENDED to either enforce the use of the different character sets or consider it as an error.

| property | key<br>(8-bit uint) |
|----------|---------------------|
| UTF_8 | 0 |
| UTF_16 | 1 |
| UTF_16BE | 2 |
| UTF_16LE | 3 |
| UTF_32 | 4 |
| UTF_32BE | 5 |
| UTF_32LE | 6 |
| ISO_8859_1 | 7 |
| US_ASCII | 8 |

## `ZapGeoPoint`

Represent a point on earth in geological coordinates.

```text
+--------------------+---------------------+
| latitude (64 bits) | longitude (64 bits) |
+--------------------+---------------------+
```

| type | signature | description |
|------|-----------|-------------|
| property | `latitude: double` | A latitude of the point. |
| property | `longitude: double` | A longitude of the point. |
