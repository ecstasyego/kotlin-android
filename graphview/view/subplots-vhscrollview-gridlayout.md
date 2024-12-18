## Examples
- https://github.com/jjoe64/GraphView/wiki/Documentation

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
├── build.gradle.kts # PROJECT-LEVEL
└── gradle.properties # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.widget.GridLayout
import android.widget.ScrollView
import android.widget.HorizontalScrollView
import androidx.appcompat.app.AppCompatActivity
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val scrollView = ScrollView(this)

        val horizontalScrollView = HorizontalScrollView(this)
        horizontalScrollView.isHorizontalScrollBarEnabled = true

        val gridLayout = GridLayout(this)
        gridLayout.rowCount = 10
        gridLayout.columnCount = 3
        gridLayout.orientation = GridLayout.HORIZONTAL

        for (i in 0 until gridLayout.rowCount * gridLayout.columnCount) {
            val graphView = GraphView(this)
            val series = LineGraphSeries<DataPoint>()
            for (j in 0..10) {
                series.appendData(DataPoint(j.toDouble(), Math.sin(j.toDouble())), true, 10)
            }
            graphView.addSeries(series)

            val params = GridLayout.LayoutParams().apply {
                width = 1000
                height = 500
            }
            graphView.layoutParams = params

            gridLayout.addView(graphView)
        }

        horizontalScrollView.addView(gridLayout)

        scrollView.addView(horizontalScrollView)

        setContentView(scrollView)
    }
}
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("com.jjoe64:graphview:4.2.2")
    implementation("androidx.compose.material3:material3:1.0.0")
    implementation("androidx.compose.ui:ui:1.0.0")
    implementation("androidx.compose.foundation:foundation:1.0.0")
    implementation("androidx.compose.material:material:1.5.0")
}
```

`gradle.properties`
```
android.useAndroidX=true
android.enableJetifier=true
```



