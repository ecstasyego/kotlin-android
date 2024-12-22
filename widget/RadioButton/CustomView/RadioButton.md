
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
import android.widget.RadioButton
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomRadioButton

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomRadioButton(this)
        setContentView(widget)
    }
}

class CustomRadioButton @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.radioButtonStyle,
    defStyleRes: Int = 0
) : RadioButton(context, attrs, defStyleAttr, defStyleRes) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.transparent))
        setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
        textSize = 18f
        isChecked = false

        setOnCheckedChangeListener { _, isChecked ->
            if (isChecked) {
                setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_light))
            } else {
                setBackgroundColor(ContextCompat.getColor(context, android.R.color.transparent))
            }
        }
    }
}
```
