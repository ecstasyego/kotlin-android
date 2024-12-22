
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
import android.widget.ToggleButton
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomToggleButton
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        widget = CustomToggleButton(this).apply {
            text = "Toggle"
            textSize = 18f
        }
        setContentView(widget)
    }
}

class CustomToggleButton @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.buttonStyle
) : ToggleButton(context, attrs, defStyleAttr) {
    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_red_light))
        textSize = 18f
        setTextColor(ContextCompat.getColor(context, android.R.color.white))
        setOnCheckedChangeListener { _, isChecked ->
            if (isChecked) {
                setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_light))
                setTextColor(ContextCompat.getColor(context, android.R.color.white))
                text = "ON"
            } else {
                setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_red_light))
                setTextColor(ContextCompat.getColor(context, android.R.color.white))
                text = "OFF"
            }
        }
    }
}
```

