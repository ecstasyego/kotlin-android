## Examples



### Example01: Basic
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

import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity(R.layout.main_layout)
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">
</LinearLayout>
```


<br>

### Example02: XML
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
    android:orientation="vertical"
    android:padding="16dp">
</LinearLayout>
```


<br>

### Example03: Layout Class
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
import android.widget.LinearLayout
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(LinearLayout(this))
    }
}
```


<br>


### Example04: Activity Life Cycle
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
import android.view.Gravity
import android.widget.Button
import android.widget.LinearLayout
import android.widget.Toast

class MainActivity : ComponentActivity() {
    private lateinit var button: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val linearLayout = LinearLayout(this)
        linearLayout.orientation = LinearLayout.VERTICAL
        linearLayout.gravity = Gravity.CENTER

        button = Button(this)
        button.text = "ONCREATE"
        button.gravity = Gravity.CENTER

        button.setOnClickListener {
            Toast.makeText(this, "ONCREATE", Toast.LENGTH_SHORT).show()
        }

        linearLayout.addView(button)
        setContentView(linearLayout)
    }

    override fun onStart() {
        super.onStart()
        button.text = "ONSTART"
        Toast.makeText(this, "ONSTART", Toast.LENGTH_SHORT).show()
    }

    override fun onResume() {
        super.onResume()
        button.text = "ONRESUME"
        Toast.makeText(this, "ONRESUME", Toast.LENGTH_SHORT).show()
    }

    override fun onPause() {
        super.onPause()
        button.text = "ONPAUSE"
        Toast.makeText(this, "ONPAUSE", Toast.LENGTH_SHORT).show()
    }

    override fun onStop() {
        super.onStop()
        button.text = "ONSTOP"
        Toast.makeText(this, "ONSTOP", Toast.LENGTH_SHORT).show()
    }

    override fun onRestart() {
        super.onRestart()
        button.text = "ONRESTART"
        Toast.makeText(this, "ONRESTART", Toast.LENGTH_SHORT).show()
    }

    override fun onDestroy() {
        super.onDestroy()
        Toast.makeText(this, "ONDESTROY", Toast.LENGTH_SHORT).show()
    }
}
```


<br>



### Example05: Fragment
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/value/themes.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val main_layout = LinearLayout(this).apply {
            addView( FrameLayout(this@MainActivity).apply {id = View.generateViewId()} )
        }
        setContentView(main_layout)

        val fragment = MainFragment()
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(main_layout.getChildAt(0).id, fragment)
        transaction.commit()
    }
}

class MainFragment : Fragment() {
    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment."} )
        }
    }
}
```

`themes.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="Theme.MyApplication" parent="Theme.AppCompat.Light" />
</resources>
```



<br>



### Example06: Intent
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



<br>



### Example07: UI and Background Thraed
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
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import android.widget.Toast

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val tableLayout = TableLayout(this)
        setContentView(tableLayout)

        // BACKGROUND THREAD
        Thread(Runnable {
            val rows = mutableListOf<TableRow>()
            List(100) { row ->
                val rowView = TableRow(this)
                List(10) { col ->
                    rowView.addView(TextView(this@MainActivity).apply { text = "[$row, $col]" })
                }
                rows.add(rowView)
            }

            // UI THREAD
            runOnUiThread {
                rows.forEach { tableLayout.addView(it) }
                Toast.makeText(this@MainActivity, "UI THREAD", Toast.LENGTH_SHORT).show()
            }
        }).start()
    }
}
```




<br>




### Example08: Thraed with lifecycle (lifecycleScope.launch)
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
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import android.widget.Toast
import androidx.lifecycle.lifecycleScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val tableLayout = TableLayout(this)
        setContentView(tableLayout)

        // BACKGROUND THREAD
        lifecycleScope.launch(Dispatchers.IO) {
            val rows = mutableListOf<TableRow>()
            List(100) { row ->
                val rowView = TableRow(this@MainActivity)
                List(10) { col ->
                    rowView.addView(TextView(this@MainActivity).apply { text = "[$row, $col]" })
                }
                rows.add(rowView)
            }

            // UI THREAD
            launch(Dispatchers.Main) {
                rows.forEach { tableLayout.addView(it) }
                Toast.makeText(this@MainActivity, "UI THREAD", Toast.LENGTH_SHORT).show()
            }
        }
    }
}
```


<br>

### Example09: Service with BroadcastReceiver
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
import android.view.View
import android.widget.Button
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
        val layout = LinearLayout(this).apply{
            addView(Button(this@MainActivity).apply { id= View.generateViewId(); text = "Start Service" })
        }
        setContentView(layout)

        val button = findViewById<Button>(layout.getChildAt(0).id)
        button.setOnClickListener {
            val serviceIntent = Intent(this, MyService::class.java)
            startService(serviceIntent)  // Service Start
        }

        val filter = IntentFilter("com.example.myapplication.RESULT_ACTION")
        registerReceiver(resultReceiver, filter, Context.RECEIVER_EXPORTED) // BroadcastReceiver
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(resultReceiver)
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
<manifest xmlns:android="http://schemas.android.com/apk/res/android" xmlns:tools="http://schemas.android.com/tools">
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

