
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
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomTableLayout
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomTableLayout(this)
        setContentView(widget)
    }
}

class CustomTableLayout @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : TableLayout(context, attrs) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))

        val row1 = TableRow(context).apply {
            val textView1 = TextView(context).apply {
                text = "Row 1, Col 1"
                setPadding(20, 20, 20, 20)
            }
            val textView2 = TextView(context).apply {
                text = "Row 1, Col 2"
                setPadding(20, 20, 20, 20)
            }
            addView(textView1)
            addView(textView2)
        }

        val row2 = TableRow(context).apply {
            val textView1 = TextView(context).apply {
                text = "Row 2, Col 1"
                setPadding(20, 20, 20, 20)
            }
            val textView2 = TextView(context).apply {
                text = "Row 2, Col 2"
                setPadding(20, 20, 20, 20)
            }
            addView(textView1)
            addView(textView2)
        }

        addView(row1)
        addView(row2)
    }
}
```

