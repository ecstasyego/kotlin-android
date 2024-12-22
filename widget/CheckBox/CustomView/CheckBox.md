
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
import android.widget.CheckBox
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomCheckBox

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomCheckBox(this)
        setContentView(widget)
    }
}

class CustomCheckBox @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = android.R.attr.checkboxStyle,
    defStyleRes: Int = 0
) : CheckBox(context, attrs, defStyleAttr, defStyleRes) {

    init {
        text = "I agree to the terms and conditions"
        textSize = 18f
        setTextColor(ContextCompat.getColor(context, android.R.color.black))
        buttonTintList = ContextCompat.getColorStateList(context, android.R.color.holo_blue_light)
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        setPadding(32, 32, 32, 32)
        isChecked = false
    }
}
```

