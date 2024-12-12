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


### Example02: with HorizontalScrollView and ScrollView
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
import android.widget.HorizontalScrollView
import android.widget.ScrollView
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val horizontalScrollView = HorizontalScrollView(this)
        val scrollView = ScrollView(this)
        val tableLayout = TableLayout(this)
        tableLayout.layoutParams = TableLayout.LayoutParams(
            TableLayout.LayoutParams.MATCH_PARENT,
            TableLayout.LayoutParams.WRAP_CONTENT
        )

        val row1 = TableRow(this).apply {
            addView(TextView(this@MainActivity).apply { text = "Row 1, Column 1" })
            addView(TextView(this@MainActivity).apply { text = "Row 1, Column 2" })
        }

        val row2 = TableRow(this).apply {
            addView(TextView(this@MainActivity).apply { text = "Row 2, Column 1" })
            addView(TextView(this@MainActivity).apply { text = "Row 2, Column 2" })
        }

        tableLayout.addView(row1)
        tableLayout.addView(row2)

        scrollView.addView(tableLayout)
        horizontalScrollView.addView(scrollView)

        setContentView(horizontalScrollView)
    }
}
```

<br>


### Example03: createTable-method with HorizontalScrollView and ScrollView
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
import android.widget.HorizontalScrollView
import android.widget.ScrollView
import android.widget.TableLayout
import android.widget.TableRow
import android.widget.TextView
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val table = createTable(100, 10)
        setContentView(table)
    }

    private fun createTable(numRow:Int, numCol:Int):HorizontalScrollView {

        val horizontalScrollView = HorizontalScrollView(this)
        val scrollView = ScrollView(this)
        val tableLayout = TableLayout(this)
        tableLayout.layoutParams = TableLayout.LayoutParams(
            TableLayout.LayoutParams.MATCH_PARENT,
            TableLayout.LayoutParams.WRAP_CONTENT
        )
        with(tableLayout){
            for (row in 0..<numRow){
                addView(TableRow(this@MainActivity).apply {
                    for (col in 0..<numCol){
                        addView(TextView(this@MainActivity).apply { text = "[$row, $col]   " })
                    }
                })
            }
        }

        scrollView.addView(tableLayout)
        horizontalScrollView.addView(scrollView)
        return horizontalScrollView
    }
}
```

<br>
