
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
import android.util.AttributeSet
import android.widget.Button
import android.widget.LinearLayout
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomLinearLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomLinearLayout(this)
        setContentView(widget)
    }
}

class CustomLinearLayout @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0,
    defStyleRes: Int = 0
) : LinearLayout(context, attrs, defStyleAttr, defStyleRes) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        orientation = VERTICAL

        val textView1 = TextView(context).apply {
            text = "Welcome to CustomLinearLayout!"
            textSize = 20f
            setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
            setPadding(16, 16, 16, 16)
        }
        val button1 = Button(context).apply {
            text = "Click Me!"
            setTextColor(ContextCompat.getColor(context, android.R.color.white))
            setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_light))
        }
        val button2 = Button(context).apply {
            text = "Another Button"
            setTextColor(ContextCompat.getColor(context, android.R.color.white))
            setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_orange_light))
        }

        addView(textView1)
        addView(button1)
        addView(button2)
    }
}
```

