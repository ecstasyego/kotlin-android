## Examples
### Example01: Activity Life Cycle
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



### Example02: Fragment
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




<br>



### Example03: Intent
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



<br>



### Example04: Thraed
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






<br>




### Example05: UI Thraed
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





<br>

### Example06: Service without BoradCastManager
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



<br>



### Example07: Service with BoradCastManager
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

