
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
import android.widget.EditText
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomEditText

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomEditText(this)
        setContentView(widget)
    }
}

class CustomEditText @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.editTextStyle,
    defStyleRes: Int = 0
) : EditText(context, attrs, defStyleAttr, defStyleRes) {

    init {
        setTextColor(ContextCompat.getColor(context, android.R.color.black))
        textSize = 18f
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        hint = "Enter text here"
        setHintTextColor(ContextCompat.getColor(context, android.R.color.darker_gray))
        setPadding(16, 16, 16, 16)
        setTypeface(typeface, android.graphics.Typeface.BOLD)
    }
}
```
