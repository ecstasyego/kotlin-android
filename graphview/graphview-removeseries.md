
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

import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import kotlin.random.Random

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val graph = GraphView(this)

        val seriesNames = listOf("s0", "s1", "s2")
        for (seriesName in seriesNames){
            val series = LineGraphSeries<DataPoint>().apply{ title = seriesName }
            for (i in 0 until 10) {
                val x = i.toDouble()
                val y = Random.nextDouble(-3.0, 3.0)
                series.appendData(DataPoint(x, y), true, 10)
            }
            graph.addSeries(series)
        }

        graph.removeSeries( graph.series.find{it.title == "s0"} )
        graph.removeSeries( graph.series.find{it.title == "s1"} )
        graph.removeSeries( graph.series.find{it.title == "s2"} )

        graph.removeAllSeries()
        setContentView(graph)
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




