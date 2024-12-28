## Examples
### Example01: Time Axis
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
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import java.text.SimpleDateFormat
import java.util.Date
import java.util.Locale
import java.util.Calendar
import kotlin.random.Random

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val layout = LinearLayout(this).apply { orientation = LinearLayout.VERTICAL }
        val graph = GraphView(this)

        val series = LineGraphSeries<DataPoint>()
        val dateRange = DateRange(start="2024-01-01", periods=1000).values
        for (i in 0 until dateRange.size) {
            val x = i.toDouble()
            val y = Random.nextDouble(-3.0, 3.0)
            series.appendData(DataPoint(x, y), true, 1000)
        }

        graph.gridLabelRenderer.labelFormatter = object : com.jjoe64.graphview.DefaultLabelFormatter() {
            override fun formatLabel(value: Double, isValueX: Boolean): String {
                if (isValueX) {
                    return dateRange.filter{ it["index"]?.toInt() == value.toInt()}[0]["date"]!!
                } else {
                    return super.formatLabel(value, isValueX)
                }
            }
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

        graph.gridLabelRenderer.labelVerticalWidth = 20
        graph.gridLabelRenderer.labelHorizontalHeight = 50
        graph.gridLabelRenderer.setHorizontalLabelsAngle(45)


        layout.addView(graph)
        setContentView(layout)
    }
}



class DateRange(start:String? = null, end:String? = null, periods:Int? = null){
    val dateFormat = SimpleDateFormat("yyyy-MM-dd", Locale.getDefault())
    var startDate:Date = if (periods != null && start == null && end != null) dateFormat.parse("0000-01-01") else dateFormat.parse(start?: dateFormat.format(Date()))
    var endDate:Date = if (periods != null && start != null && end == null) dateFormat.parse("9999-12-31") else dateFormat.parse(end?: dateFormat.format(Date()))

    val values = mutableListOf<Map<String, String>>()
    init{
        var cnt:Int = 0
        var calendar = Calendar.getInstance().apply{ time = startDate }
        while (calendar.time <= endDate) {
            values.add(
                mapOf(
                    "index" to cnt.toString(),
                    "date" to dateFormat.format(calendar.time),
                    "year" to calendar.get(Calendar.YEAR).toString(),
                    "month" to calendar.get(Calendar.MONTH).toString(),
                    "day" to calendar.get(Calendar.DAY_OF_MONTH).toString(),
                )
            )

            // iter-condition
            if (periods != null){ if (periods == cnt + 1) break }

            // dayshift
            val dayshift = if (startDate < endDate) 1 else -1
            calendar.add(Calendar.DATE, dayshift)
            cnt++
        }
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





