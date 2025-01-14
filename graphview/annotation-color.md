
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

import android.graphics.Color
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import android.widget.FrameLayout
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val frameLayout = FrameLayout(this)
        frameLayout.setBackgroundColor(Color.LTGRAY)

        val graph = GraphView(this)
        val series1 = LineGraphSeries<DataPoint>(arrayOf(
            DataPoint(0.0, 1.0),
            DataPoint(1.0, 3.0),
            DataPoint(2.0, 2.0),
            DataPoint(3.0, 4.0),
            DataPoint(4.0, 1.5)
        ))
        val series2 = LineGraphSeries<DataPoint>(arrayOf(
            DataPoint(0.0, 1.0),
            DataPoint(1.0, 1.0),
            DataPoint(2.0, 1.0),
            DataPoint(3.0, 1.0),
            DataPoint(4.0, 1.0)
        ))

        graph.addSeries(series1)
        graph.addSeries(series2)

        series1.color = Color.RED
        series1.isDrawDataPoints = true
        series1.isDrawBackground = true
        series1.backgroundColor = Color.argb(50, 255, 0, 0)

        series2.color = Color.BLUE
        series2.isDrawDataPoints = true

        frameLayout.addView(graph)
        setContentView(frameLayout)
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



