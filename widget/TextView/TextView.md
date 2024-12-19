## Examples
### Example01: with design
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
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val textView = TextView(this)
        textView.text = "Hello, Kotlin!"
        setContentView(textView)
    }
}
```
