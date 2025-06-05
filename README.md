# kotlin-android
- APK(s)
    - Build > Build Bundle(s) / APK(s) > Build APK(s)
    - C:\Users\[USER]\AndroidStudioProjects\[APPLICATION]\app\build\outputs\apk\debug
- Physical Device Emulator
    - PC Installation: https://developer.android.com/studio/run/oem-usb
    - Mobile Developer Mode
        - 소프트웨어 정보 -> 빌드번호
        - 개발자 옵션 -> USB Debugging
    - Android Studio
        - Troubleshoot Device Connections(USB Debugging ON/OFF)
        - Tools -> SDK Manager -> Android SDK & SDK Tools
            - Google USB Driver -> Apply & OK
            - Android Emulator -> Apply & OK
        - ADB Server: `C:\Users\[USER]\AppData\Local\Android\Sdk\platform-tools\adb.exe`
            - `adb kill-server`
            - `adb start-server`
            - `adb devices`
            - `adb root`
            - `adb shell`
            - `adb pull [PATH]`
- Device Explorer
    - on Android Studio
        - ADB Shell: `C:\Users\[USER]\AppData\Local\Android\Sdk\platform-tools\adb.exe`
            - `adb root`        
            - `adb shell`
                - `run-as [COM].[EXAMPLE].[APPLICATION]`
        - View > Tools Windows > Device Explorer
- Image Asset Icons
    - Configure Image Asset
        - Name, Source Asset(type: Image)
            - res/drawable/logo.png
            - AndroidManifest.xml: `<application>` ~ `</application>`
                - android:icon="@mipmap/logo_launcher"
                - android:roundIcon="@mipmap/logo_launcher_round"
            - res/mipmap/logo_launcher/*
            - res/mipmap/logo_launcher_round/*
- Vector Asset Icons
    - Remove: C:\Users\user\AppData\Local\Android\Sdk\icons\material\icons_metadata.txt
        - Read-Only
- Caches
    - Remove: C:\Users\user\\.gradle\caches\\*
    - Rebuild: Build/Clean Project, Build/Rebuild Project 
- Permission
    - https://www.privacy.go.kr/front/main/main.do
    
  
## Application Design
- Requirements Analysis
    - User requirements
    - System requirements
    - Performance requirements
    - Security requirements
    - Platform requirements
    - System constraints
- System Architecture Design
    - Client-server model
    - Distributed systems
    - Microservices architecture
    - Database design
    - Communication protocols
- Database Design
    - ERD (Entity-Relationship Diagram)
    - Normalization
    - Query optimization
- User Interface (UI) / User Experience (UX) Design
    - Wireframing
    - Prototyping
    - Usability testing
- Technology Stack Selection
    - Programming languages
    - Frameworks
    - Databases
    - Cloud platforms
    - Version control systems
- Security Design
    - Authentication and authorization
    - Data encryption
    - Vulnerability testing
    - Logging and monitoring
- Testing Design
    - Unit testing
    - Integration testing
    - UI/UX testing
    - Load testing
    - Security testing
- Maintenance and Update Planning
    - Bug fixes
    - Feature updates
    - Performance tuning
- Documentation
    - System design documents
    - Code comments and inline documentation
    - User manuals


  
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
`lifecycle`
```kotlin
onAttach()
  onCreate()
    onCreateView()
    onViewCreated()
    onActivityCreated()
      onStart()
        onResume()
        onPause()
      onStop()
    onDestroyView()
  onDestroy()
onDetach()
```
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
`build.gradle.kts(APP-LEVEL)`
```
plugins {
    alias(libs.plugins.android.application)
    alias(libs.plugins.kotlin.android)
    alias(libs.plugins.kotlin.compose)
    id("kotlin-kapt") // for Room annotation processing
}

android {
    namespace = "com.example.myapplication"
    compileSdk = 35

    defaultConfig {
        applicationId = "com.example.myapplication"
        minSdk = 26
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"

        testInstrumentationRunner = "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            isMinifyEnabled = false
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }
    kotlinOptions {
        jvmTarget = "11"
    }
    buildFeatures {
        compose = true
        viewBinding = true
    }
    kapt {
        correctErrorTypes = true
    }
}

dependencies {
    implementation("io.insert-koin:koin-android:3.4.0")
    implementation("io.insert-koin:koin-android-compat:3.4.0")
    implementation("com.jjoe64:graphview:4.2.2")

    implementation("androidx.room:room-runtime:2.6.0") // Room
    implementation("androidx.room:room-ktx:2.6.0") // Room KTX (for Coroutines)
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4") // Coroutines
    kapt("androidx.room:room-compiler:2.6.0") // Room annotation processor

    implementation(libs.androidx.core.ktx)
    implementation(libs.androidx.lifecycle.runtime.ktx)
    implementation(libs.androidx.activity.compose)
    implementation(platform(libs.androidx.compose.bom))
    implementation(libs.androidx.ui)
    implementation(libs.androidx.ui.graphics)
    implementation(libs.androidx.ui.tooling.preview)
    implementation(libs.androidx.material3)
    implementation(libs.androidx.recyclerview)
    implementation(libs.androidx.constraintlayout)
    implementation(libs.androidx.cardview)
    implementation(libs.material)
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


## Debugging
`Toast`
```kotlin
import android.widget.Toast

Toast.makeText(this, "MESSAGE", Toast.LENGTH_SHORT).show() // activity
Toast.makeText(requireContext(), "MESSAGE", Toast.LENGTH_SHORT).show() // fragment
Toast.makeText(applicationContext, "MESSAGE", Toast.LENGTH_SHORT).show() // service
```

`Log`
```kotlin
import android.util.Log

Log.v("TAG", "This is a verbose log.") // Verbose(v)
Log.d("TAG", "This is a debug log.") // Debug(d)
Log.i("TAG", "This is an info log.") // Info(i)
Log.w("TAG", "This is a warning log.") // Warn(w)
// + Error(e), What a Terrible Failure(wtf)

```




<br><br><br>

## Android Studio
### Keymap: File > Settings > Keymap
- Alt + Enter: Build
- Alt + Arrow: Tab Shift
- Alt + 1: Explorer
- Alt + 7: Code Structure
- Alt + F7: Find Usages
- Ctrl + Left-Click : Definition
- Ctrl + Alt + X
    - File > Settings > Keymap > Search Actions by Name: Show Context Menu: Ctrl + Alt + X
- Shift + ESC: Hide Bottom Window

### Version Control
- File > Settings(Ctrl + Alt + S) > GitHub > `+` > `OK`
- Git > Mange Remotes 


<br><br><br>

## Kotlin-Jupyter
```bash
$ pip install jupyterlab
$ pip install kotlin-jupyter-kernel
```
