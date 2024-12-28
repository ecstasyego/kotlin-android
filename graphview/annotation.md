
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
import androidx.activity.ComponentActivity
import androidx.core.content.ContextCompat
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import kotlin.random.Random

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val layout = LinearLayout(this).apply { orientation = LinearLayout.VERTICAL }
        val graph = GraphView(this)

        val series = LineGraphSeries<DataPoint>()
        for (i in 0 until 10) {
            val x = i.toDouble()
            val y = Random.nextDouble(-3.0, 3.0)
            series.appendData(DataPoint(x, y), true, 10)
        }

        graph.addSeries(series)
        graph.viewport.isScalable = true
        graph.viewport.isScrollable = true
        graph.gridLabelRenderer.isHorizontalLabelsVisible = true
        graph.gridLabelRenderer.isVerticalLabelsVisible = true

        graph.gridLabelRenderer.horizontalAxisTitle = "Date"
        graph.gridLabelRenderer.verticalAxisTitle = "Value"
        graph.gridLabelRenderer.horizontalAxisTitleTextSize = 40f
        graph.gridLabelRenderer.verticalAxisTitleTextSize = 40f
        graph.gridLabelRenderer.setHorizontalAxisTitleColor(ContextCompat.getColor(this, android.R.color.darker_gray)) // X-axis title color
        graph.gridLabelRenderer.setVerticalAxisTitleColor(ContextCompat.getColor(this, android.R.color.darker_gray)) // Y-axis title color

        graph.gridLabelRenderer.labelHorizontalHeight = 50
        graph.gridLabelRenderer.labelVerticalWidth = 20
        graph.gridLabelRenderer.setHorizontalLabelsAngle(45)

        graph.gridLabelRenderer.gridColor = ContextCompat.getColor(this, android.R.color.darker_gray)  // Light grid color
        graph.gridLabelRenderer.horizontalLabelsColor = ContextCompat.getColor(this, android.R.color.darker_gray)  // Light X-axis label color
        graph.gridLabelRenderer.verticalLabelsColor = ContextCompat.getColor(this, android.R.color.darker_gray)  // Light X-axis label color

        series.color = ContextCompat.getColor(this, android.R.color.darker_gray)  // Lighter graph line color

        layout.addView(graph)
        setContentView(layout)
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



