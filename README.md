<h1><img src="https://user-images.githubusercontent.com/6410412/280930996-ab5931f7-1f1f-42b9-a243-63d1d9c3ada4.png" width="22px" height="22px" /> Zap</h1>

Zap is an application programming library for building multi-device application that enable communication with other devices. While mobile devices offer a wide range of data sources, such as motion sensors, biometrics devices, microphones, touchscreens and more, traditional PCs like laptops and desktops are typically lack these resources.

The data sources available on mobile devices are valuable, but are often device-dependent, limiting their widespread use. Imagine if PCs could use the series of data from the accelerometer sensor on a mobile device. A simple example is using smartphone as motion controller for PC.

<video src="https://user-images.githubusercontent.com/6410412/284037217-6dbbdcce-1cf4-4c92-b903-15f670bfa9bc.mp4" muted controls></viedo>

In the video above, a smartphone served as a motion controller for the driving game '[slow roads](https://slowroads.io/)' running on laptop. The smartphone's accelerometer sensor transmitted the measured values to the laptop via Zap client, where the laptop received these values through Zap server. Subsequently, the server triggers a specific keys(in this case, 'A' and 'D' keys) based on the values and their corresponding thresholds.

Of course, it can also become a simple remote controller, not a motion controller. Below, a video demonstrates using a smartphone as a controller to play '[Super Mario Bros.](https://supermario-game.com/)'.

<video src="https://user-images.githubusercontent.com/6410412/281803217-38e92248-7f19-4b56-be38-f6339e9c4086.mp4" muted controls></viedo>

To overcome the limitation that data sources are confined to a single mobile device, Zap provides programming interface to access data sources on other devices. In the following example shows that the client instance on an Android device sends acceleration force data to the server device.

```kotlin
class MainActivity: AppCompatActivity(), SensorEventListener {
  private lateinit var zap: ZapClient

  override fun onCreate(state: Bundle?) {
    // Create a new zap client with the server's IP address.
    zap = ZapClient(InetAddress.getByName(...))
  }

  // Define the method that is invoked whenever
  // any sensors on the local device have changed.
  override fun onSensorChanged(event: SensorEvent) {
    if (event.sensor.type == Sensor.TYPE_ACCELEROMETER) {
      val (x, y, z) = event.values
      // Send the data acquired from the accelerometer to the server.
      zap.send(ZapAccelerometer(x, y, z))
    }
  }
}
```

> Currently, Zap does not provide a built-in methods for obtaining the server's IP address. To identify the server device's address, you may consider alternative way such as using QR code to share the server's address or communicating via protocols like NFC or BLE.

On the server device, the server instance can retrieve the data from accelerometer sensor on the client device. In this example, TypeScript code is executed on the Node.js runtime on a desktop.

```typescript
// Create and start a new zap server to listen for data from clients.
(new class extends ZapServer {
  // Define the method that is called whenever accelerometer sensor data is
  // received from client devices.
  onAccelerometerReceived(info: MetaInfo, data: ZapAccelerometer) {
    console.log(`Data received from ${info.dgram.address}: (${data.x}, ${data.y}, ${data.z})`);
  }
}).listen();
```

The main goal of Zap is to support mobile-PC communication, but it also extends its capabilities to enable mobile-mobile and PC-PC communication. Furthermore, it's not limited to PCs; any devices capable of running Zap implementations(e.g., Kiosk device, Smart TV, etc.) can also participate in this communication.

## Example Applications

What kind of awesome applications can we build by extending the boundaries of data sources with Zap? Here are a some example applications:

- [Motion controller](https://github.com/zap-lib/examples/tree/main/motion-controller): An example of turning a mobile device into a remote game controller. This controller features motion sensing capabilities, which determine the device's orientation through an accelerometer sensor. The server, running on a PC, receives data from the controller and maps it to specific keys on the keyboard.
- [Digital ink](https://github.com/zap-lib/examples/tree/main/digital-ink): An example that assists in obtaining text from handwritten notes made on a mobile device's touchscreen on a PC.

See more examples on [zap-lib/examples](https://github.com/zap-lib/examples) repository.

## Implementations

- Node.js: [github.com/zap-lib/node](https://github.com/zap-lib/node)
- Kotlin: [github.com/zap-lib/kotlin](https://github.com/zap-lib/node)

## License

Zap is released under the [Apache License, Version 2.0](LICENSE).
