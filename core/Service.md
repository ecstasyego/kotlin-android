## Examples

### Example01: Service with BroadcastReceiver
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.app.Service
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.os.Bundle
import android.os.IBinder
import android.widget.LinearLayout
import android.widget.Toast
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {

    private val resultReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            val result = intent?.getStringExtra("result")
            Toast.makeText(this@MainActivity, "Service Result: $result", Toast.LENGTH_LONG).show()
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        val serviceIntent = Intent(this, MyService::class.java)
        startService(serviceIntent)  // Service Start
    }

    override fun onStart() {
        super.onStart()
        val filter = IntentFilter("com.example.myapplication.RESULT_ACTION")
        registerReceiver(resultReceiver, filter, Context.RECEIVER_EXPORTED) // Service Register
    }

    override fun onStop() {
        super.onStop()
        unregisterReceiver(resultReceiver) // Service Unregister
    }
}


class MyService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Thread {
            Thread.sleep(2000)
            val result = "WORK DONE!"
            sendResultToActivity(result)
        }.start()

        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }

    private fun sendResultToActivity(result: String) {
        val intent = Intent("com.example.myapplication.RESULT_ACTION")
        intent.putExtra("result", result)
        sendBroadcast(intent)  // Broadcast TO Activity
    }
}
```

`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <service android:name=".MyService" android:exported="false" />

    </application>

</manifest>
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
android {
    defaultConfig {
        minSdk = 26
    }
}
```


<br>


### Example02: Service with LocalBroadcastManager
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.app.Service
import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.os.Bundle
import android.os.IBinder
import android.widget.LinearLayout
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.localbroadcastmanager.content.LocalBroadcastManager

class MainActivity : ComponentActivity() {

    private val resultReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            val result = intent?.getStringExtra("result")
            Toast.makeText(this@MainActivity, "Service Result: $result", Toast.LENGTH_LONG).show()
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        val serviceIntent = Intent(this, MyService::class.java)
        startService(serviceIntent)  // Service Start

    }

    override fun onStart() {
        super.onStart()
        val filter = IntentFilter("com.example.myapplication.RESULT_ACTION")
        LocalBroadcastManager.getInstance(this).registerReceiver(resultReceiver, filter)
    }

    override fun onStop() {
        super.onStop()
        LocalBroadcastManager.getInstance(this).unregisterReceiver(resultReceiver)

    }
}


class MyService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Thread {
            Thread.sleep(2000)
            val result = "WORK DONE!"
            sendResultToActivity(result)
        }.start()

        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }

    private fun sendResultToActivity(result: String) {
        val intent = Intent("com.example.myapplication.RESULT_ACTION")
        intent.putExtra("result", result)
        LocalBroadcastManager.getInstance(this).sendBroadcast(intent)
    }
}
```



`AndroidManifest.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <service android:name=".MyService" android:exported="false" />

    </application>

</manifest>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
android {
    defaultConfig {
        minSdk = 26
    }
}

dependencies {
    implementation("androidx.localbroadcastmanager:localbroadcastmanager:1.1.0")
}
```

<br>

### Example03: Service with LiveData
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
```

`AndroidManifest.xml`
```xml
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
```



<br>



### Example04: Service with EventBus
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
```

`AndroidManifest.xml`
```xml
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
```
