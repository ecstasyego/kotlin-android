## Examples
### Example01: Basic
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
import android.os.Build
import android.os.Bundle
import android.os.IBinder
import android.widget.LinearLayout
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.annotation.RequiresApi

class MainActivity : ComponentActivity() {

    private val resultReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            val result = intent?.getStringExtra("result")
            Toast.makeText(this@MainActivity, "Service Result: $result", Toast.LENGTH_LONG).show()
            // abortBroadcast()
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))

        val serviceIntent = Intent(this, ContentService::class.java)
        startService(serviceIntent)  // Service Start
    }

    @RequiresApi(Build.VERSION_CODES.O)
    override fun onStart() {
        super.onStart()
        val filter = IntentFilter("com.example.myapplication.RESULT_ACTION")
        registerReceiver(resultReceiver, filter, Context.RECEIVER_EXPORTED) // Service Register
        //registerReceiver(resultReceiver, filter, "com.example.myapplication.PERMISSION", null) // with permission
    }

    override fun onStop() {
        super.onStop()
        unregisterReceiver(resultReceiver) // Service Unregister
    }
}


class ContentService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Thread {
            Thread.sleep(2000)
            val result = "WORK DONE!"

            val intent = Intent("com.example.myapplication.RESULT_ACTION")
            intent.putExtra("result", result)
            sendBroadcast(intent)  // Broadcast TO Activity
            //sendBroadcast(intent, "com.example.myapplication.PERMISSION") // with Permission
        }.start()

        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
        <service android:name=".ContentService" />

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


### Example02: Normal Broadcast
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
import android.os.Build
import android.os.Bundle
import android.os.IBinder
import android.widget.LinearLayout
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.annotation.RequiresApi

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

        val serviceIntent = Intent(this, ContentService::class.java)
        startService(serviceIntent)  // Service Start
    }

    @RequiresApi(Build.VERSION_CODES.O)
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


class ContentService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Thread {
            Thread.sleep(2000)
            val result = "WORK DONE!"

            val intent = Intent("com.example.myapplication.RESULT_ACTION")
            intent.putExtra("result", result)
            sendBroadcast(intent)  // Broadcast TO Activity
        }.start()

        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
        <service android:name=".ContentService" />

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

### Example03: Ordered Broadcast
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
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
import android.os.Build
import android.os.Bundle
import android.os.IBinder
import android.widget.LinearLayout
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.annotation.RequiresApi

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

        val serviceIntent = Intent(this, ContentService::class.java)
        startService(serviceIntent)  // Service Start
    }

    @RequiresApi(Build.VERSION_CODES.O)
    override fun onStart() {
        super.onStart()
        val filter = IntentFilter("com.example.myapplication.RESULT_ACTION")
        filter.priority = 100 // Higher priority means it will receive the broadcast first.
        registerReceiver(resultReceiver, filter, Context.RECEIVER_EXPORTED) // Service Register
    }

    override fun onStop() {
        super.onStop()
        unregisterReceiver(resultReceiver) // Service Unregister
    }
}


class ContentService : Service() {
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Thread {
            Thread.sleep(2000)
            val result = "WORK DONE!"

            val intent = Intent("com.example.myapplication.RESULT_ACTION")
            intent.putExtra("result", result)
            sendOrderedBroadcast(intent, null)  // Second argument is permission, set to null for no permission.
        }.start()

        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
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
        <service android:name=".ContentService" />

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

### Example04: Protected Broadcast
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.widget.LinearLayout
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val linearLayout = LinearLayout(this)
        linearLayout.orientation = LinearLayout.HORIZONTAL
        setContentView(linearLayout)
    }
}
```

