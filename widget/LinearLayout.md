## LinearLayout

https://developer.android.com/reference/android/widget/LinearLayout

### XML attributes
- android:baselineAligned : When set to false, prevents the layout from aligning its children's baselines. 
- android:baselineAlignedChildIndex : When a linear layout is part of another layout that is baseline aligned, it can specify which of its children to baseline align to (that is, which child TextView). 
- android:divider : Drawable to use as a vertical divider between buttons. 
- android:gravity : Specifies how an object should position its content, on both the X and Y axes, within its own bounds. 
- android:measureWithLargestChild : When set to true, all children with a weight will be considered having the minimum size of the largest child. 
- android:orientation : Should the layout be a column or a row? Use "horizontal" for a row, "vertical" for a column. 
- android:weightSum : Defines the maximum weight sum. 


<br><br><br>

---

## Example01
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
import android.widget.Button
import android.widget.LinearLayout
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val button01 = Button(this); button01.text = "Button01"
        val button02 = Button(this); button02.text = "Button02"

        val linearLayout = LinearLayout(this)
        linearLayout.orientation = LinearLayout.HORIZONTAL
        linearLayout.addView(button01)
        linearLayout.addView(button02)
        setContentView(linearLayout)
    }
}
```

<br>

### Usage: *.xml(findViewById)
![image](https://github.com/user-attachments/assets/f0a2f228-44d7-4146-bfa5-34ad0c7532e8)

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
    android:orientation="horizontal">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Hello, World!" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Hello, World!" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:text="Hello, World!" />

</LinearLayout>
```

<br>

## Example02
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
import android.widget.LinearLayout
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val parentLayout = LinearLayout(this)
        val childLayout1 = LinearLayout(this)
        val childLayout2 = LinearLayout(this)

        childLayout1.addView(
            TextView(this).apply {
                text = "First Child Layout"
            }
        )
        childLayout2.addView(
            TextView(this).apply {
                text = "Second Child Layout"
            }
        )
        parentLayout.addView(childLayout1)
        parentLayout.addView(childLayout2)

        setContentView(parentLayout)
    }
}
```

<br>

## Example03

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
import android.widget.LinearLayout
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val parentLayout = LinearLayout(this).apply {
            orientation = LinearLayout.VERTICAL
            layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.MATCH_PARENT
            )
        }

        val childLayout1 = LinearLayout(this).apply {
            orientation = LinearLayout.HORIZONTAL
            layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.WRAP_CONTENT
            )
        }

        val childLayout2 = LinearLayout(this).apply {
            orientation = LinearLayout.HORIZONTAL
            layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT,
                LinearLayout.LayoutParams.WRAP_CONTENT
            )
        }

        childLayout1.addView(
            TextView(this).apply {
                text = "First Child Layout"
            }
        )
        childLayout2.addView(
            TextView(this).apply {
                text = "Second Child Layout"
            }
        )
        parentLayout.addView(childLayout1)
        parentLayout.addView(childLayout2)
        setContentView(parentLayout)
    }
}
```


