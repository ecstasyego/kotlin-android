
## Examples
### Example01: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       └── AndroidManifest.xml
│   ├── google-services.json
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
import com.google.firebase.database.DatabaseReference
import com.google.firebase.database.FirebaseDatabase

class MainActivity : ComponentActivity() {
    private lateinit var database: FirebaseDatabase
    private lateinit var myRefAll: DatabaseReference
    private lateinit var myRef00: DatabaseReference
    private lateinit var myRef01: DatabaseReference

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))
        database = FirebaseDatabase.getInstance()

        myRef00 = database.getReference("message00")
        myRef01 = database.getReference("message01")

        myRefAll = database.reference
        myRef00 = myRefAll.child("message00")
        myRef01 = myRefAll.child("message01")

    }
}

data class User(val name: String="", val age: Int=0)
```

`AndroidManifest.xml`
```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.example.myapplication">

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

`google-services.json`
```json
{
  "project_info": {
    "project_number": "------------",
    "firebase_url": "https://[project-name]-default-rtdb.firebaseio.com",
    "project_id": "[project-name]",
    "storage_bucket": "ailever-beta.firebasestorage.app"
  },
  "client": [
    {
      "client_info": {
        "mobilesdk_app_id": "-:------------:android:----------------------",
        "android_client_info": {
          "package_name": "com.example.myapplication"
        }
      },
      "oauth_client": [],
      "api_key": [
        {
          "current_key": "---------------------------------------"
        }
      ],
      "services": {
        "appinvite_service": {
          "other_platform_oauth_client": []
        }
      }
    }
  ],
  "configuration_version": "1"
}
```

`build.gradle.kts(PROJECT-LEVEL)`
```kotlin
plugins {
  id("com.google.gms.google-services") version "4.4.2" apply false
}
```

`build.gradle.kts(APP-LEVEL)`
```kotlin
plugins {
  id("com.android.application")
  id("com.google.gms.google-services")
}

dependencies {
  implementation(platform("com.google.firebase:firebase-bom:33.7.0"))
  implementation("com.google.firebase:firebase-database")
}
```
