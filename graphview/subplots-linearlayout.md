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
import android.widget.LinearLayout
import androidx.appcompat.app.AppCompatActivity
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Create a LinearLayout to hold the graphs
        val linearLayout = LinearLayout(this)
        linearLayout.orientation = LinearLayout.VERTICAL // Stack graphs vertically
        linearLayout.layoutParams = LinearLayout.LayoutParams(
            LinearLayout.LayoutParams.MATCH_PARENT,
            LinearLayout.LayoutParams.MATCH_PARENT
        )

        // Create the first GraphView (first subplot)
        val graph1 = GraphView(this)
        val series1 = LineGraphSeries<DataPoint>()
        for (i in 0 until 10) {
            val x = i.toDouble()
            val y = Math.sin(x)
            series1.appendData(DataPoint(x, y), true, 10)
        }
        graph1.addSeries(series1)
        graph1.viewport.isScalable = true
        graph1.viewport.isScrollable = true

        // Set the size for the first GraphView (for example, 500px height)
        val params1 = LinearLayout.LayoutParams(
            LinearLayout.LayoutParams.MATCH_PARENT, 500 // Set height to 500px
        )
        graph1.layoutParams = params1

        // Create the second GraphView (second subplot)
        val graph2 = GraphView(this)
        val series2 = LineGraphSeries<DataPoint>()
        for (i in 0 until 10) {
            val x = i.toDouble()
            val y = Math.cos(x)
            series2.appendData(DataPoint(x, y), true, 10)
        }
        graph2.addSeries(series2)
        graph2.viewport.isScalable = true
        graph2.viewport.isScrollable = true

        // Set the size for the second GraphView (for example, 500px height)
        val params2 = LinearLayout.LayoutParams(
            LinearLayout.LayoutParams.MATCH_PARENT, 500 // Set height to 500px
        )
        graph2.layoutParams = params2

        // Add both graphs (subplots) to the LinearLayout
        linearLayout.addView(graph1)
        linearLayout.addView(graph2)

        // Set the content view to be the LinearLayout
        setContentView(linearLayout)
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



