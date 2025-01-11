
## Examples
### Example01: *.kt
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/drawable/ripple_background.xml
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
    lateinit var widget:CardView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CardView(this@MainActivity).apply{
            addView(LinearLayout(this@MainActivity).apply{
                orientation = LinearLayout.VERTICAL
                addView(TextView(this@MainActivity).apply{text="TEXTVIEW00"})
                addView(TextView(this@MainActivity).apply{text="TEXTVIEW01"})
                addView(TextView(this@MainActivity).apply{text="TEXTVIEW03"})
            })
        }
        widget.isClickable = true
        widget.isFocusable = true
        widget.setBackgroundResource(R.drawable.ripple_background)

        setContentView(widget)
    }
}
```

`ripple_background.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@android:color/darker_gray" >

    <!-- Background shape that will be shown when the ripple is triggered -->
    <item android:id="@android:id/mask">
        <shape xmlns:android="http://schemas.android.com/apk/res/android">
            <solid android:color="@android:color/white"/>
            <corners android:radius="8dp"/>
        </shape>
    </item>

    <!-- Content of the ripple background -->
    <item>
        <shape xmlns:android="http://schemas.android.com/apk/res/android">
            <solid android:color="@android:color/white"/>
            <corners android:radius="8dp"/>
        </shape>
    </item>

</ripple>
```


<br>


### Example02: *.xml
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/drawable/ripple_background.xml
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
<androidx.cardview.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/cardView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp"
    android:layout_marginBottom="8dp"
    android:radius="8dp"
    android:clickable="true"
    android:focusable="true"
    android:background="@drawable/ripple_background">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:id="@+id/textView00"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="TEXTVIEW00"
            android:textSize="16sp" />

        <TextView
            android:id="@+id/textView01"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="TEXTVIEW01"
            android:textSize="16sp" />

        <TextView
            android:id="@+id/textView03"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="TEXTVIEW03"
            android:textSize="16sp" />

    </LinearLayout>
</androidx.cardview.widget.CardView>
```

`ripple_background.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@android:color/darker_gray" >

    <!-- Background shape that will be shown when the ripple is triggered -->
    <item android:id="@android:id/mask">
        <shape xmlns:android="http://schemas.android.com/apk/res/android">
            <solid android:color="@android:color/white"/>
            <corners android:radius="8dp"/>
        </shape>
    </item>

    <!-- Content of the ripple background -->
    <item>
        <shape xmlns:android="http://schemas.android.com/apk/res/android">
            <solid android:color="@android:color/white"/>
            <corners android:radius="8dp"/>
        </shape>
    </item>

</ripple>
```

