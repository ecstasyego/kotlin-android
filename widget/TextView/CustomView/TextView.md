
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
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomTextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomTextView(this)
        setContentView(widget)
    }
}

class CustomTextView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.textViewStyle,
    defStyleRes: Int = 0
) : TextView(context, attrs, defStyleAttr, defStyleRes) {

    init {
        text = "Hello, CustomTextView!"
        textSize = 20f
        setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
        gravity = android.view.Gravity.CENTER
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        setPadding(32, 32, 32, 32)
        setTypeface(typeface, android.graphics.Typeface.BOLD)
    }
}
```

