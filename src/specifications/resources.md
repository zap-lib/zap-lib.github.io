# Resources

## `enum ZapResource`

A registry of resources supported by Zap and their identification values.

| property | value | description |
|------|-----------|-------------|
| `ACCELEROMETER` | `ACC` | A data measured by an accelerometer sensor. |
| `UI_EVENT` | `UIE` | A data related to event raised by the user interface. |
| `TEXT` | `TXT` | A simple text data. |

## `ZapText`

Represent simple text.

| type | signature | description |
|------|-----------|-------------|
| property | `str: string` | Just string. |
| property | `charset` | A character set of `str`. (default: `UTF-8`) |

## `ZapAccelerometer`

Represent values measured by accelerometer sensor.

| type | signature | description |
|------|-----------|-------------|
| property | `x: float` | Acceleration force along the x axis. (m/s²) |
| property | `y: float` | Acceleration force along the y axis. (m/s²) |
| property | `z: float` | Acceleration force along the z axis. (m/s²) |

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
