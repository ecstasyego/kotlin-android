## Change View
### Example01: change layout
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


<br>

### Example02: change layout with FrameLayout
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
import android.widget.FrameLayout
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val frameLayout = findViewById<FrameLayout>(R.id.viewLayout)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            frameLayout.removeAllViews()
            frameLayout.addView(TextView(this@MainActivity).apply{text = "NEW TEXT"})
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
            android:text="INITIAL TEXT"
            android:textSize="18sp"/>
    </FrameLayout>

</LinearLayout>
```


<br>



### Example03: change layout with LayoutInflater
#### File System
```
.Project
├── app
│   ├── src
│   │   └── main
│   │       ├── java/com/example/myapplication/MainActivity.kt
│   │       ├── res/layout/main_layout.xml
│   │       ├── res/layout/custom_layout.xml
│   │       └── AndroidManifest.xml
│   └── build.gradle.kts # APP-LEVEL
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.view.LayoutInflater
import android.widget.Button
import androidx.activity.ComponentActivity
import android.widget.LinearLayout

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        val mainLayout = findViewById<LinearLayout>(R.id.mainLayout)
        val button = findViewById<Button>(R.id.button)

        button.setOnClickListener {
            mainLayout.removeViewAt(1)
            mainLayout.addView(
                LayoutInflater.from(this).inflate(R.layout.custom_layout, null)
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


`custom_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/customLayout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_height="wrap_content"
        android:layout_width="wrap_content"
        android:text="NEW TEXT"/>
</LinearLayout>
```


<br>

### Example04: change layout with FragmentManager
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
```

`main_layout.xml`
```xml
```


<br>

### Example05: change layout with Intent on activity
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
```

`AndroidManifest.xml`
```xml
```

`main_layout.xml`
```xml
```


