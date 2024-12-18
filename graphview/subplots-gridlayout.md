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
import android.view.Gravity
import android.widget.GridLayout
import androidx.appcompat.app.AppCompatActivity
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val gridLayout = GridLayout(this)
        gridLayout.rowCount = 2  
        gridLayout.columnCount = 1

        val graphView1 = GraphView(this)
        val series1 = LineGraphSeries<DataPoint>()
        for (i in 0..10) {
            series1.appendData(DataPoint(i.toDouble(), Math.sin(i.toDouble())), true, 10)
        }
        graphView1.addSeries(series1)
        graphView1.layoutParams = GridLayout.LayoutParams().apply {
            width = GridLayout.LayoutParams.MATCH_PARENT
            height = 500
            setGravity(Gravity.CENTER)
        }

        val graphView2 = GraphView(this)
        val series2 = LineGraphSeries<DataPoint>()
        for (i in 0..10) {
            series2.appendData(DataPoint(i.toDouble(), Math.cos(i.toDouble())), true, 10)
        }
        graphView2.addSeries(series2)
        graphView2.layoutParams = GridLayout.LayoutParams().apply {
            width = GridLayout.LayoutParams.MATCH_PARENT
            height = 500
            setGravity(Gravity.CENTER)
        }

        gridLayout.addView(graphView1)
        gridLayout.addView(graphView2)

        setContentView(gridLayout)
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



