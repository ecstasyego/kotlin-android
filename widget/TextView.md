### Examples
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
import android.widget.TextView
import android.widget.Toast
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val textView: TextView = findViewById(R.id.textView)
        textView.text = "New Text"
        textView.setTextColor(Color.RED)
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

![image](https://github.com/user-attachments/assets/001fe0e0-c48b-47c5-82ff-f76e30343040)
