---
hide:
  - navigation
---

# Getting Started

Let's explore step-by-step how to create a multi-device application using Zap. In this example, we'll use the [Zap Kotlin implementation](https://github.com/zap-lib/kotlin) on Android to real-time transmit accelerometer sensor values from a mobile device to a [Zap Node.js implementation](https://github.com/zap-lib/node) running on a laptop.

## Server

Let's first create a server that will receive accelerometer sensor values from the client. The server will run on the laptop. Simply create an npm project, and after that (you can use yarn or pnpm if you prefer), install `zap-lib-js`.

```sh
$ mkdir my-zap-server
$ cd my-zap-server
$ npm init
$ npm install zap-lib-js
```

Now, simply create an index.js file and write the Zap server. This server will receive accelerometer sensor values measured based on the x, y, z axes from the client and print them to the console each time it receives the values.

```js
import { ZapServer } from 'zap-lib-js';

(new class extends ZapServer {
  onAccelerometerReceived(info, data) {
    console.log(`Data received from ${info.dgram.address}: (${data.x}, ${data.y}, ${data.z})`);
  }
}).listen();
```

It's surprisingly simple. Now, if you run `index.js` using `node`, the server will be up and listening.

```sh
$ node index.js
```

## Client

Now that we've created the server, let's create the client. Let's consider the client to be running on an Android mobile device.

Create an Android project based on an empty activity, and then add dependency. Since Zap is distributed through JitPack, you need to first add the JitPack repository to the `settings.gradle` file.

```diff
dependencyResolutionManagement {
  repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
  repositories {
    google()
    mavenCentral()
+   maven { url 'https://jitpack.io' }
  }
}
```

Next, add the dependency to the `build.gradle` file. Replace `$VERSION` with your desired version. For example, if you want to use version v0.1.0, input `0.1.0`.

```diff
dependencies {
  // ...
+ implementation 'com.github.zap-lib:kotlin:$VERSION'
}
```

Now, in the `MainActivity.kt` file, define the callback function that is called every time the accelerometer sensor embedded in the device measures a new value. For now, let's just output the measured x, y, z values using Logcat.

```kotlin
class MainActivity: AppCompatActivity(), SensorEventListener {
  private lateinit var sensorManager: SensorManager

  override fun onCreate(state: Bundle?) {
    super.onCreate(state)
    setContentView(R.layout.activity_main)

    sensorManager = getSystemService(Context.SENSOR_SERVICE) as SensorManager
  }

  override fun onSensorChanged(event: SensorEvent) {
    if (event.sensor.type == Sensor.TYPE_ACCELEROMETER) {
      val (x, y, z) = event.values
      Log.i(this::class.java.simpleName, "$x, $y, $z")
    }
  }

  override fun onAccuracyChanged(p0: Sensor?, p1: Int) {}

  override fun onStart() {
    super.onStart()
    sensorManager.registerListener(this,
      sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER),
      SensorManager.SENSOR_DELAY_GAME)
    }

  override fun onStop() {
    super.onStop()
    sensorManager.unregisterListener(this)
  }
}
```

In this state, when you build and run the application, the values measured based on the x, y, z axes will be logged every time the device is tilted. Now, let's send the values to the server using Zap. Since Zap communicates through UDP socket, you need to add `INTERNET` permission to the `AndroidManifest.xml` file first.

```diff
<?xml ...>
<manifest ...>
+   <uses-permission android:name="android.permission.INTERNET" />
    <application ...>
      <activity ... />
    </application>
</manifest>
```

Sending data using Zap is really easy. You just need to add a few lines to the code written above.

```diff
class MainActivity: AppCompatActivity(), SensorEventListener {
  private lateinit var sensorManager: SensorManager
+ private lateinit var zap: ZapClient

  override fun onCreate(state: Bundle?) {
    super.onCreate(state)
    setContentView(R.layout.activity_main)

    sensorManager = getSystemService(Context.SENSOR_SERVICE) as SensorManager
+   zap = ZapClient(InetAddress.getByName("192.168.0.1"))
  }

  override fun onSensorChanged(event: SensorEvent) {
    if (event.sensor.type == Sensor.TYPE_ACCELEROMETER) {
      val (x, y, z) = event.values
-     Log.i(this::class.java.simpleName, "$x, $y, $z")
+     zap.send(ZapAccelerometer(x, y, z))
    }
  }

  override fun onAccuracyChanged(p0: Sensor?, p1: Int) {}

  override fun onStart() {
    super.onStart()
    sensorManager.registerListener(this,
      sensorManager.getDefaultSensor(Sensor.TYPE_ACCELEROMETER),
      SensorManager.SENSOR_DELAY_GAME)
    }

  override fun onStop() {
    super.onStop()
    sensorManager.unregisterListener(this)
+   zap.stop()
  }
}
```

Now, if you tilt the client device, you should see numerous values being continuously printed in the console of the server device.

> Note that when creating a `ZapClient` instance, the server's IP address is passed as a constant value. Here, `192.168.0.1` is the local IP address of the laptop where the server, written earlier, is running. In this example, the client and server are on the same network, so a local IP can be used. If the server device has its public IP, it does not necessarily need to be connected to the same network.

You might find it unreasonable to write the server's IP address as a constant. There are various ways to dynamically determine the server's IP address, but typically, you can consider the following methods:

- NFC: Place the client device close to the server device to receive the server's IP address.
- Bluetooth: Pair the server device and the client device to obtain the server's IP address.
- QR: The server device provides a QR code containing its IP address on a monitor, and the client device can scan it to obtain the address.

 You can check the example application in the [examples repository](https://github.com/zap-lib/examples) for the QR method.
