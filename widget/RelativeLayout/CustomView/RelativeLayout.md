
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
import android.widget.Button
import android.widget.RelativeLayout
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomRelativeLayout
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomRelativeLayout(this)
        setContentView(widget)
    }
}

class CustomRelativeLayout @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0,
    defStyleRes: Int = 0
) : RelativeLayout(context, attrs, defStyleAttr) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))

        val textView = TextView(context).apply {
            id = View.generateViewId()  // 고유한 ID를 생성
            text = "Hello, CustomRelativeLayout!"
            textSize = 20f
            setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
        }
        val button = Button(context).apply {
            id = View.generateViewId()  // 고유한 ID를 생성
            text = "Click Me"
            textSize = 18f
            setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_light))
            setTextColor(ContextCompat.getColor(context, android.R.color.white))
        }
        addView(textView)
        addView(button)

        val paramsTextView = textView.layoutParams as LayoutParams
        paramsTextView.addRule(CENTER_HORIZONTAL)  // 화면 중앙에 배치
        paramsTextView.topMargin = 100  // 상단 여백 100px
        textView.layoutParams = paramsTextView

        val paramsButton = button.layoutParams as LayoutParams
        paramsButton.addRule(BELOW, textView.id)  // TextView 아래에 배치
        paramsButton.topMargin = 32  // TextView와의 간격
        button.layoutParams = paramsButton
    }
}
```

