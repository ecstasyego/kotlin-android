
### Example02: with design
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

import android.graphics.Color
import android.graphics.Typeface
import android.os.Bundle
import android.view.Gravity
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val textView = TextView(this)
        textView.text = "Hello, Kotlin!"
        textView.textSize = 24f
        textView.setTextColor(Color.RED)
        textView.setTypeface(null, Typeface.BOLD) // FONT
        textView.setTypeface(Typeface.SERIF)  // FONT
        textView.setShadowLayer(1.5f, 5f, 5f, Color.GRAY)  // SHADE
        textView.gravity = Gravity.CENTER  // ALIGN
        textView.layoutParams = LinearLayout.LayoutParams(
            LinearLayout.LayoutParams.MATCH_PARENT, // WIDTH
            LinearLayout.LayoutParams.WRAP_CONTENT  // HEIGHT
        )

        setContentView(textView)
    }
}
```
