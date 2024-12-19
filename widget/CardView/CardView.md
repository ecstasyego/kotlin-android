
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

import android.os.Bundle
import android.widget.LinearLayout
import android.widget.TextView
import androidx.activity.ComponentActivity
import androidx.cardview.widget.CardView

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val main_layout = LinearLayout(this).apply{
            addView(CardView(this@MainActivity).apply{
                addView(LinearLayout(this@MainActivity).apply{
                    addView(TextView(this@MainActivity).apply{text="TEXTVIEW00"})
                    addView(TextView(this@MainActivity).apply{text="TEXTVIEW01"})
                    addView(TextView(this@MainActivity).apply{text="TEXTVIEW03"})
                })
            })
        }
        setContentView(main_layout)
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
    android:orientation="vertical">

    <androidx.cardview.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:elevation="4dp"
        android:layout_margin="8dp">

        <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:padding="16dp">

            <TextView
                android:id="@+id/textView00"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TEXTVIEW00"
                android:textSize="16sp"/>

            <TextView
                android:id="@+id/textView01"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TEXTVIEW01"
                android:textSize="14sp"
                android:visibility="visible" />

            <TextView
                android:id="@+id/textView02"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TEXTVIEW02"
                android:textSize="14sp"
                android:visibility="invisible" />

            <TextView
                android:id="@+id/textView03"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="TEXTVIEW03"
                android:textSize="14sp"
                android:visibility="gone" />
        </LinearLayout>
    </androidx.cardview.widget.CardView>
</LinearLayout>
```


