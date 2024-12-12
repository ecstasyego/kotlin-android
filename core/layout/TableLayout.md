## Examples
### Example01: Basic
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
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val tableLayout = TableLayout(this)
        tableLayout.layoutParams = TableLayout.LayoutParams(
            TableLayout.LayoutParams.MATCH_PARENT,
            TableLayout.LayoutParams.WRAP_CONTENT
        )

        val row1 = TableRow(this)
        row1.addView( TextView(this).apply{ text = "Row 1, Column 1" } )
        row1.addView( TextView(this).apply{ text = "Row 1, Column 2" } )

        val row2 = TableRow(this)
        row2.addView( TextView(this).apply{ text = "Row 2, Column 1" } )
        row2.addView( TextView(this).apply{ text = "Row 2, Column 2" } )

        tableLayout.addView(row1)
        tableLayout.addView(row2)

        setContentView(tableLayout)
    }
}
```

<br>


### Example02
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
```

<br>
