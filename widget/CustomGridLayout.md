
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
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.content.Context
import android.os.Bundle
import android.view.View
import android.widget.GridLayout
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomLayout(this)
        widget.addView(TextView(this).apply { text = "Hello, World!" })

        setContentView(widget)
    }
}

class CustomLayout @JvmOverloads constructor(context: Context) : GridLayout(context) {
    init {
        layoutParams = LayoutParams().apply {
            width = LayoutParams.MATCH_PARENT // MATCH_PARENT, WRAP_CONTENT
            height = LayoutParams.MATCH_PARENT // MATCH_PARENT, WRAP_CONTENT
        }
    }

    override fun addView(child: View) {
        super.addView(child)
    }
}
```

