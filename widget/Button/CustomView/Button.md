
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
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    lateinit var widget: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomButton(this)
        setContentView(widget)
    }
}

class CustomButton @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.buttonStyle,
    defStyleRes: Int = 0
) : Button(context, attrs, defStyleAttr, defStyleRes) {

    init {
        text = "Click Me!"
        textSize = 18f
        setTextColor(context.getColor(android.R.color.white))
        setBackgroundColor(context.getColor(android.R.color.holo_green_light))
        background = context.getDrawable(android.R.drawable.btn_default)
        setPadding(50, 20, 50, 20)
    }
}
```

