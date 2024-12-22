
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
import android.widget.Switch
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomSwitch
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        widget = CustomSwitch(this).apply {
            text = "Enable Feature"
            textSize = 18f
        }
        setContentView(widget)
    }
}

class CustomSwitch @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.switchStyle
) : Switch(context, attrs, defStyleAttr) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.transparent))
        setTextColor(ContextCompat.getColor(context, android.R.color.black))

        isChecked = false
        setOnCheckedChangeListener { _, isChecked ->
            if (isChecked) {
                setTextColor(ContextCompat.getColor(context, android.R.color.white))
                setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_light))
                text = "Feature Enabled"
            } else {
                setTextColor(ContextCompat.getColor(context, android.R.color.white))
                setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_red_light))
                text = "Feature Disabled"
            }
        }
    }
}
```

