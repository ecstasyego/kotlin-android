## Examples
### Example01: Activity Design
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

import android.os.Bundle
import androidx.activity.ComponentActivity
import android.content.Intent
import android.view.View
import android.widget.Button
import android.widget.LinearLayout

class MainActivity : ComponentActivity() {
    private lateinit var layout:LinearLayout
    private lateinit var widget:Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        layout = LinearLayout(this).apply { addView( Button(this@MainActivity).apply {id = View.generateViewId(); text="MainActivity"} ) }
        setContentView(layout)

        widget = findViewById(layout.getChildAt(0).id)
        widget.setOnClickListener{
            startActivity( Intent(this, ResultActivity01::class.java) )
        }

    }
}

class ResultActivity01: ComponentActivity() {
    private lateinit var layout:LinearLayout
    private lateinit var widget:Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        layout = LinearLayout(this).apply { addView( Button(this@ResultActivity01).apply {id = View.generateViewId(); text="ResultActivity01"} ) }
        setContentView(layout)

        widget = findViewById(layout.getChildAt(0).id)
        widget.setOnClickListener{
            startActivity( Intent(this, ResultActivity02::class.java) )
        }

    }
}

class ResultActivity02: ComponentActivity() {
    private lateinit var layout:LinearLayout
    private lateinit var widget:Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        layout = LinearLayout(this).apply { addView( Button(this@ResultActivity02).apply {id = View.generateViewId(); text="ResultActivity02"} ) }
        setContentView(layout)

        widget = findViewById(layout.getChildAt(0).id)
        widget.setOnClickListener{
            startActivity( Intent(this, ResultActivity03::class.java) )
        }

    }
}

class ResultActivity03: ComponentActivity() {
    private lateinit var layout:LinearLayout
    private lateinit var widget:Button
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        layout = LinearLayout(this).apply { addView( Button(this@ResultActivity03).apply {id = View.generateViewId(); text="ResultActivity03"} ) }
        setContentView(layout)

        widget = findViewById(layout.getChildAt(0).id)
        widget.setOnClickListener{
            startActivity( Intent(this, MainActivity::class.java) )
        }
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
        <activity android:name=".ResultActivity01"></activity>
        <activity android:name=".ResultActivity02"></activity>
        <activity android:name=".ResultActivity03"></activity>

    </application>

</manifest>
```

