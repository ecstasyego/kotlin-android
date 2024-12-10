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

import android.os.Bundle
import android.widget.Button
import androidx.activity.ComponentActivity
import androidx.gridlayout.widget.GridLayout
import androidx.gridlayout.widget.GridLayout.LayoutParams

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val gridLayout = GridLayout(this).apply {
            rowCount = 2
            columnCount = 5
            layoutParams = LayoutParams().apply {
                width = LayoutParams.MATCH_PARENT // MATCH_PARENT, WRAP_CONTENT
                height = LayoutParams.MATCH_PARENT // MATCH_PARENT, WRAP_CONTENT
            }
        }

        for (i in 1..2) {
            for (j in 1..5) {
                val button = Button(this).apply {
                    text = "Button $i, $j"
                    layoutParams = LayoutParams().apply {
                        width = 0
                        height = LayoutParams.WRAP_CONTENT
                        rowSpec = GridLayout.spec(i-1)
                        columnSpec = GridLayout.spec(j-1, 1f)
                    }
                }
                gridLayout.addView(button)
            }
        }

        setContentView(gridLayout)
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
<androidx.gridlayout.widget.GridLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:columnCount="5"
    app:rowCount="2">

    <Button
        android:text="Button 1, 1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 1, 2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 1, 3"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 1, 4"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 1, 5"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 2, 1"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 2, 2"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 2, 3"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 2, 4"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

    <Button
        android:text="Button 2, 5"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_columnWeight="1" />

</androidx.gridlayout.widget.GridLayout>
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


