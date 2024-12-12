## Examples
### Usage: *.kt
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

import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.FrameLayout
import android.widget.LinearLayout
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    private var counter = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val mainLayout = LinearLayout(this).apply{
            addView(Button(this@MainActivity).apply{text="Click Me!";      id=View.generateViewId()})
            addView(
                FrameLayout(this@MainActivity).apply{
                    id=View.generateViewId()
                    addView(TextView(this@MainActivity).apply{text = "TEXT01"; visibility=View.INVISIBLE; id=View.generateViewId()})
                    addView(TextView(this@MainActivity).apply{text = "TEXT02"; visibility=View.INVISIBLE; id=View.generateViewId()})
                    addView(TextView(this@MainActivity).apply{text = "TEXT03"; visibility=View.VISIBLE  ; id=View.generateViewId()})
                }
            )
        }
        setContentView(mainLayout)

        val button = mainLayout.getChildAt(0) as Button
        val frameLayout = mainLayout.getChildAt(1) as FrameLayout
        val textViews = (0..<frameLayout.childCount).map{frameLayout.getChildAt(it) as TextView}.toMutableList()

        button.setOnClickListener {
            counter++
            val visibilities = List(textViews.size){it == counter%textViews.size}.map{ if (it) View.VISIBLE else View.INVISIBLE }.toList()
            for ( idx in 0..<visibilities.size ){
                textViews[idx].apply{ visibility = visibilities[idx] }
            }
        }
    }
}
```

<br>

### Usage: *.xml(findViewById)
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
import android.view.View
import android.widget.Button
import android.widget.FrameLayout
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    private var counter:Int = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val frameLayout = findViewById<FrameLayout>(R.id.viewLayout)
        val first_view = frameLayout.getChildAt(0)
        val last_view = frameLayout.getChildAt(1)

        val visibilities = listOf(View.INVISIBLE, View.VISIBLE)
        val button = findViewById<Button>(R.id.button)
        button.setOnClickListener {
            counter++
            first_view.visibility = visibilities[counter%2]
            last_view.visibility = visibilities[(counter%2+1)%2]
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

    <FrameLayout
        android:id="@+id/viewLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="FIRST TEXT"
            android:visibility="invisible"
            android:textSize="18sp"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:visibility="visible"
            android:text="LAST TEXT"
            android:textSize="18sp"/>
    </FrameLayout>

</LinearLayout>
```

