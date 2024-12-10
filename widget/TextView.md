## Examples
### Usage: *.kt
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

import android.graphics.Color
import android.graphics.Typeface
import android.os.Bundle
import android.view.Gravity
import android.widget.LinearLayout
import android.widget.TextView
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val textView = TextView(this)
        textView.text = "Hello, Kotlin!"
        textView.text = getString(R.string.greeting)
        textView.textSize = 24f
        textView.setTextColor(Color.RED)
        textView.setTextColor(ContextCompat.getColor(this, R.color.purple_700))
        textView.setTypeface(null, Typeface.BOLD) // FONT
        textView.setTypeface(Typeface.SERIF)  // FONT
        textView.setShadowLayer(1.5f, 5f, 5f, Color.GRAY)  // SHADE
        textView.gravity = Gravity.CENTER  // ALIGN
        textView.layoutParams = LinearLayout.LayoutParams(
            LinearLayout.LayoutParams.MATCH_PARENT, // WIDTH
            LinearLayout.LayoutParams.WRAP_CONTENT  // HEIGHT
        )
        textView.setOnClickListener {
            Toast.makeText(this, "TextView clicked", Toast.LENGTH_SHORT).show()
        }

        val layout = LinearLayout(this)
        layout.orientation = LinearLayout.VERTICAL
        layout.addView(textView)
        setContentView(layout)
    }
}
```

<br>

### Usage: *.xml(findViewById)
![image](https://github.com/user-attachments/assets/001fe0e0-c48b-47c5-82ff-f76e30343040)

#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/values/strings.xml
│   │       ├── res/values/colors.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.graphics.Color
import android.graphics.Typeface
import android.os.Bundle
import android.view.Gravity
import android.widget.TextView
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        textView.text = "New Text"
        textView.text = getString(R.string.greeting)
        textView.setTextColor(Color.RED)
        textView.setTextColor(ContextCompat.getColor(this, R.color.purple_700))
        textView.setTypeface(null, Typeface.BOLD) // FONT
        textView.setTypeface(Typeface.SERIF)  // FONT
        textView.setShadowLayer(1.5f, 5f, 5f, Color.GRAY)  // SHADE
        textView.gravity = Gravity.CENTER  // ALIGN
        textView.setOnClickListener {
            Toast.makeText(this, "TextView clicked", Toast.LENGTH_SHORT).show()
        }
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, World!"
        android:textSize="20sp"
        android:textColor="#000000"
        android:layout_marginTop="20dp"/>

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

