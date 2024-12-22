
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
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.constraintlayout.widget.ConstraintLayout
import androidx.constraintlayout.widget.ConstraintSet
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    lateinit var widget: CustomConstraintLayout

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomConstraintLayout(this)
        setContentView(widget)
    }
}

class CustomConstraintLayout @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0,
    defStyleRes: Int = 0
) : ConstraintLayout(context, attrs, defStyleAttr, defStyleRes) {

    init {
        setBackgroundColor(ContextCompat.getColor(context, android.R.color.white))
        val textView = TextView(context).apply {
            id = generateViewId()  // 고유 id 생성
            text = "Hello, CustomConstraintLayout!"
            textSize = 20f
            setTextColor(ContextCompat.getColor(context, android.R.color.holo_blue_dark))
        }
        val button = Button(context).apply {
            id = generateViewId()  // 고유 id 생성
            text = "Click Me"
            textSize = 18f
            setBackgroundColor(ContextCompat.getColor(context, android.R.color.holo_green_light))
            setTextColor(ContextCompat.getColor(context, android.R.color.white))
        }
        addView(textView)
        addView(button)

        val constraintSet = ConstraintSet()
        constraintSet.clone(this)
        constraintSet.connect(textView.id, ConstraintSet.TOP, this.id, ConstraintSet.TOP, 100) // 상단 100px
        constraintSet.connect(textView.id, ConstraintSet.LEFT, this.id, ConstraintSet.LEFT, 32) // 왼쪽 32px
        constraintSet.connect(textView.id, ConstraintSet.RIGHT, this.id, ConstraintSet.RIGHT, 32) // 오른쪽 32px
        constraintSet.constrainWidth(textView.id, ConstraintSet.MATCH_CONSTRAINT) // 너비 설정
        constraintSet.applyTo(this)

        constraintSet.clone(this)
        constraintSet.connect(button.id, ConstraintSet.TOP, textView.id, ConstraintSet.BOTTOM, 32) // TextView 아래 32px
        constraintSet.connect(button.id, ConstraintSet.LEFT, this.id, ConstraintSet.LEFT, 32) // 왼쪽 32px
        constraintSet.connect(button.id, ConstraintSet.RIGHT, this.id, ConstraintSet.RIGHT, 32) // 오른쪽 32px
        constraintSet.constrainWidth(button.id, ConstraintSet.MATCH_CONSTRAINT) // 너비 설정
        constraintSet.applyTo(this)
    }
}
```

<br>

### Example02: *.xml(findViewById)
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/value/strings.xml
│   │       ├── res/value/colors.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

</LinearLayout>
```


`strings.xml`
```xml
<resources>
    <string name="app_name">My Application</string>
    <string name="greeting">Hello, Android!</string>
</resources>
```


`colors.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="purple_200">#FFBB86FC</color>
    <color name="purple_500">#FF6200EE</color>
    <color name="purple_700">#FF3700B3</color>
    <color name="teal_200">#FF03DAC5</color>
    <color name="teal_700">#FF018786</color>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
</resources>
```
