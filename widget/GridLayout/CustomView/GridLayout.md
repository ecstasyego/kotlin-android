
## Examples
### Example01: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
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
import android.widget.GridLayout
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomGridLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomGridLayout(this)
        setContentView(widget)
    }
}

class CustomGridLayout @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0,
    defStyleRes: Int = 0
) : GridLayout(context, attrs, defStyleAttr, defStyleRes) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        columnCount = 2  // 2열로 설정
        rowCount = 2  // 2행으로 설정

        val textView1 = TextView(context).apply {
            text = "Cell 1"
            textSize = 18f
            setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
            setPadding(16, 16, 16, 16)
        }

        val textView2 = TextView(context).apply {
            text = "Cell 2"
            textSize = 18f
            setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
            setPadding(16, 16, 16, 16)
        }

        val button1 = Button(context).apply {
            text = "Button 1"
            setTextColor(ContextCompat.getColor(context, android.R.color.white))
            setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_dark))
        }

        val button2 = Button(context).apply {
            text = "Button 2"
            setTextColor(ContextCompat.getColor(context, android.R.color.white))
            setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_dark))
        }

        addView(textView1)
        addView(textView2)
        addView(button1)
        addView(button2)

        val params1 = LayoutParams().apply {
            rowSpec = GridLayout.spec(0)  
            columnSpec = GridLayout.spec(0)
        }
        textView1.layoutParams = params1

        val params2 = LayoutParams().apply {
            rowSpec = GridLayout.spec(0)  
            columnSpec = GridLayout.spec(1)
        }
        textView2.layoutParams = params2

        val params3 = LayoutParams().apply {
            rowSpec = GridLayout.spec(1)  
            columnSpec = GridLayout.spec(0) 
        }
        button1.layoutParams = params3

        val params4 = LayoutParams().apply {
            rowSpec = GridLayout.spec(1)  
            columnSpec = GridLayout.spec(1)
        }
        button2.layoutParams = params4
    }
}
```

