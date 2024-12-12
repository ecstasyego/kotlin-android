## Examples
### Example01: Change Layout without LayoutInflater
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

import android.os.Bundle
import android.widget.Button
import androidx.activity.ComponentActivity
import android.widget.LinearLayout
import android.widget.TextView

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            mainLayout.removeViewAt(1)
            mainLayout.addView(
                LinearLayout(this@MainActivity).apply {
                    addView(TextView(this@MainActivity).apply{text = "NEW TEXT"})
                }
            )
        }
    }
}
```


`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/mainLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Click Me" />

    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/viewLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:text="INITIAL TEXT"/>
    </LinearLayout>
</LinearLayout>
```
