
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
import android.widget.LinearLayout
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat
import androidx.core.widget.NestedScrollView

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomScrollView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomScrollView(this)
        setContentView(widget)
    }
}

class CustomScrollView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0,
    defStyleRes: Int = 0
) : NestedScrollView(context, attrs, defStyleAttr) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        val linearLayout = LinearLayout(context).apply {
            orientation = LinearLayout.VERTICAL
        }

        for (i in 1..20) {
            val textView = TextView(context).apply {
                text = "Item $i"
                textSize = 18f
                setPadding(20, 20, 20, 20)
            }
            linearLayout.addView(textView)
        }
        addView(linearLayout)
    }
}
```

