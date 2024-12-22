
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
import android.view.View
import android.widget.RadioButton
import android.widget.RadioGroup
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomRadioGroup

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        widget = CustomRadioGroup(this)
        widget.addView( RadioButton(this).apply { text = "Option 1"; id = View.generateViewId() })
        widget.addView( RadioButton(this).apply { text = "Option 2"; id = View.generateViewId() })
        setContentView(widget)
    }
}

class CustomRadioGroup @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
) : RadioGroup(context, attrs) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.transparent))
        setOnCheckedChangeListener { group, checkedId ->
            val checkedRadioButton = findViewById<RadioButton>(checkedId)
            checkedRadioButton?.let {
                it.text = "Selected: ${it.text}"
            }
        }
    }
}
```
