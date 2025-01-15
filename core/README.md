# Core Concepts

- Application > (Intent) > Activity > (Bundle) > Fragement
    - Activity, Fragement > (Intent) > Service > (BroadcastReceiver) > Activity, Fragement

```
- Context
    - Application 
        - Main Thread(UI): Activity, Fragement
            - Intent, State, Bundle, SharedPreferences 
            - View
                - DataBinding, LayoutInflater
                    - findViewById
            - ViewModel
                - LiveData, StateFlow
        - Background Thread: Service
            - Intent, BroadcastReceiver
``` 

```kotlin
package com.example.myapplication

import android.app.Service
import android.content.Intent
import android.os.Bundle
import android.os.IBinder
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.fragment.app.FragmentTransaction

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val frameLayout = FrameLayout(this@MainActivity).apply {id = View.generateViewId()}
        setContentView(frameLayout)

        // Service
        val intent = Intent(this, ContentService::class.java)
        intent.putExtra("SERVICE_PARAM00", "Hello")
        intent.putExtra("SERVICE_PARAM01", "Service")
        startService(intent)

        // Fragment
        val fragment = MainFragment.newInstance("Hello", "Fragment")
        val transaction: FragmentTransaction = supportFragmentManager.beginTransaction()
        transaction.replace(frameLayout.id, fragment)
        transaction.commit()
    }
}

class MainFragment : Fragment() {
    private lateinit var param00: String
    private lateinit var param01: String

    companion object {
        fun newInstance(param00:String, param01:String): MainFragment {
            val fragment = MainFragment()
            val args = Bundle()
            args.putString("ARGS_PARAM00", param00)
            args.putString("ARGS_PARAM01", param01)
            fragment.arguments = args
            return fragment
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        param00 = arguments?.getString("ARGS_PARAM00").toString()
        param01 = arguments?.getString("ARGS_PARAM01").toString()
    }

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
        return LinearLayout(requireContext()).apply {
            addView( TextView(requireContext()).apply {text = "This is main fragment."} )
        }
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
    }

}

class ContentService : Service() {
    private lateinit var param00: String
    private lateinit var param01: String

    override fun onCreate() {
        super.onCreate()
        Toast.makeText(this, "Service Created", Toast.LENGTH_SHORT).show()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        param00 = intent?.getStringExtra("SERVICE_PARAM00").toString()
        param01 = intent?.getStringExtra("SERVICE_PARAM01").toString()

        Toast.makeText(this, "$param00, $param01", Toast.LENGTH_SHORT).show()
        return START_STICKY
    }

    override fun onBind(intent: Intent?): IBinder? {
        return null
    }
}
```



<br><br><br>

## Change View
### Example01: change layout through functions of removal and creation
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
import android.widget.Button
import androidx.activity.ComponentActivity
import android.widget.LinearLayout
import android.widget.TextView

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            mainLayout.removeViewAt(1)
            mainLayout.addView(
                LinearLayout(this@MainActivity).apply {
                    addView(TextView(this@MainActivity).apply{text = "NEW TEXT"})
                }
            )
        }
    }
}
```


`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/viewLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="INITIAL TEXT"/>
    </LinearLayout>
</LinearLayout>
```

<br>

### Example02: change layout with FrameLayout(1: visibility control) 
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

`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.FrameLayout
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    private var counter:Int = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val frameLayout = findViewById<FrameLayout>(R.id.viewLayout)
        val first_view = frameLayout.getChildAt(0)
        val last_view = frameLayout.getChildAt(1)

        val visibilities = listOf(View.INVISIBLE, View.VISIBLE)
        val button = findViewById<Button>(R.id.button)
        button.setOnClickListener {
            counter++
            first_view.visibility = visibilities[counter%2]
            last_view.visibility = visibilities[(counter%2+1)%2]
        }
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

    <FrameLayout
        android:id="@+id/viewLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="FIRST TEXT"
            android:visibility="invisible"
            android:textSize="18sp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:visibility="visible"
            android:text="LAST TEXT"
            android:textSize="18sp"/>
    </FrameLayout>

</LinearLayout>
```


<br>

### Example03: change layout with FrameLayout(2: removal and creation)
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
import android.widget.Button
import android.widget.FrameLayout
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val frameLayout = findViewById<FrameLayout>(R.id.viewLayout)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            frameLayout.removeAllViews()
            frameLayout.addView(TextView(this@MainActivity).apply{text = "NEW TEXT"})
        }
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

    <FrameLayout
        android:id="@+id/viewLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="INITIAL TEXT"
            android:textSize="18sp"/>
    </FrameLayout>

</LinearLayout>
```


<br>



### Example04: change layout with LayoutInflater
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/layout/custom_layout.xml
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
import android.widget.Button
import androidx.activity.ComponentActivity
import android.widget.LinearLayout

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            mainLayout.removeViewAt(1)
            mainLayout.addView(
                LayoutInflater.from(this).inflate(R.layout.custom_layout, null)
            )
        }
    }
}
```


`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/viewLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="INITIAL TEXT"/>
    </LinearLayout>
</LinearLayout>
```


`custom_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/customLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:text="NEW TEXT"/>
</LinearLayout>
```


<br>

### Example05: change layout with ViewSwitcher
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
```

`main_layout.xml`
```xml
```


<br>

### Example06: change layout with ViewFlipper
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
```

`main_layout.xml`
```xml
```




<br>

### Example07: change layout with FragmentTransaction
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
```

`main_layout.xml`
```xml
```


<br>

### Example08: change layout with Intent
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
```

`AndroidManifest.xml`
```xml
```

`main_layout.xml`
```xml
```


