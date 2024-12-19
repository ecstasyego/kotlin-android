# kotlin-android
- Physical Device Emulator
    - PC Installation: https://developer.android.com/studio/run/oem-usb
    - Mobile Developer Mode
        - 소프트웨어 정보 -> 빌드번호
        - 개발자 옵션 -> USB 디버깅
- Vector Asset Icons
    - Remove: C:\Users\user\AppData\Local\Android\Sdk\icons\material\icons_metadata.txt
        - Read-Only
    
## File System
- C:\Users\user\AndroidStudioProjects\Project

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

`AndroidManifest.xml`
- Activity
- Service
- Receiver
- Provider

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
    </application>

</manifest>
```

`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}
```
`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!" />
</LinearLayout>
```


## API Reference
- https://developer.android.com/reference
- https://developer.android.com/reference/classes
- https://developer.android.com/reference/packages


## Application Architecture
- data: Entity, Database, Repository
- network: Service, Request, Response
- screen: Activity, Fragement, ViewModel, State
- widgets: ViewHolder, Listener, Adapter
  
![image](https://github.com/user-attachments/assets/31e0c027-96ab-43b7-b7d0-081fcfa35726)

- https://developer.android.com/topic/architecture
- https://developer.android.com/topic/architecture/recommendations
- https://github.com/android/architecture-templates/tree/base

### Activity
`lifecycle`
```kotlin
onCreate()
  onStart()
    onResume()
    onPause()
  onStop()
onDestroy()
```

```kotlin
val intent = Intent(this, Activity::class.java)

intent.putExtra("key", "value")
intent.putExtra("key1", 0)
intent.putExtra("key2", 0.0)
intent.getIntExtra("id_number", 0)
intent.getStringExtra("key")
intent.getStringExtra("name") ?:"Unknown"
intent.getDoubleExtra("height", 0.0)
intent.getDoubleExtra("width", 0.0)

startActivity( Intent(this, Activity::class.java) )
finish()
```


### Fragment
```kotlin
val fragment = MainFragment()
val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
transaction.replace(R.id.fragment_container, fragment)
transaction.commit()
```

### Intent
```kotlin
// Explicit Intent
val intent = Intent(this, Activity::class.java)
val intent = Intent(this, Service::class.java)
val intent = Intent(this, BroadcastReceiver::class.java)

// Implicit Intent
val intent = Intent(Intent.ACTION_VIEW, Uri.parse("https://www.example.com"))
val intent = Intent(Intent.ACTION_DIAL, Uri.parse("tel:123456789"))
val intent = Intent(Intent.ACTION_BATTERY_LOW)
val intent = Intent(Intent.ACTION_SEND)
```

#### PendingIntent
```kotlin
val intent = Intent(this, Activity::class.java)
val pendingIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT)

val intent = Intent(this, Service::class.java)
val pendingIntent = PendingIntent.getService(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT)
```

#### Intent Flags
```kotlin
val intent = Intent(this, Activity::class.java)
intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK
```


### Service
```kotlin
val intent = Intent(this, Service::class.java)

startService(intent)
stopService(intent)
bindService(intent, serviceConnection, Context.BIND_AUTO_CREATE)
unbindService(serviceConnection)
```

### Broadcast Receiver
```kotlin
val intent = Intent(this, BroadcastReceiver::class.java)
val intent = Intent(Intent.ACTION_BATTERY_LOW)

sendBroadcast(intent)
```

### Content Provider
```kotlin
```

### View
```kotlin
```

### Context
```kotlin
```

### RecyclerView
```kotlin
```

### SharedPreferences
```kotlin
```

### Room Database
```kotlin
```



<br><br><br>
## Build System: Gradle / kts
`build.gradle.kts`
```
dependencies {
    implementation("androidx.constraintlayout:constraintlayout:2.2.0-alpha07")
    implementation("androidx.constraintlayout:constraintlayout-compose:1.1.0-alpha07")
    implementation("com.github.PhilJay:MPAndroidChart:v3.1.0")

    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.lifecycle.runtime.ktx)
    implementation(libs.androidx.activity.compose)
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.androidx.ui)
    implementation(libs.androidx.ui.graphics)
    implementation(libs.androidx.ui.tooling.preview)
    implementation(libs.androidx.material3)
    testImplementation(libs.junit)
    androidTestImplementation(libs.androidx.junit)
    androidTestImplementation(libs.androidx.espresso.core)
    androidTestImplementation(platform(libs.androidx.compose.bom))
    androidTestImplementation(libs.androidx.ui.test.junit4)
    debugImplementation(libs.androidx.ui.tooling)
    debugImplementation(libs.androidx.ui.test.manifest)
}
```

`settings.gradle.kts`
```
maven (url  = "https://jitpack.io")
```

### Packages
- Graph Visualization
    - AChartEngine
        - dependencies { implementation("org.achartengine:achartengine:1.2.0") }
    - AnyChart
        - dependencies { implementation("com.anychart:android:1.1.0") }
    - EazeGraph
        - dependencies { implementation("com.github.QuadFlask:colorpicker:0.0.13") } 
    - GraphView
        - dependencies { implementation("com.jjoe64:graphview:4.2.2") }
    - HelloCharts
        - dependencies { implementation("com.gitlab.hellocharts:library:1.5.8") }
    - MPAndroidChart
        - dependencies { implementation("com.github.PhilJay:MPAndroidChart:v3.1.0") }
    - Plotly
        - dependencies { implementation("com.plotly:plotly-android:2.0.0") }
    - SciChart
        - dependencies { implementation("com.github.QuadFlask:colorpicker:0.0.13") }
- Data
    - Room
        - dependencies { }
    - DataFrame
        - dependencies { }



<br><br><br>

## Android Studio
### Keymap
- Alt + Arrow: Tab Shift
- Alt + 1: Explorer
- Ctrl + Alt + X
    - File > Settings > Keymap > Search Actions by Name: Show Context Menu: Ctrl + Alt + X
- Shift + ESC: Hide Bottom Window






