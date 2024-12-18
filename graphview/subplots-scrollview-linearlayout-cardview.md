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
import android.widget.ScrollView
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val scrollView = ScrollView(this)

        val linearLayout = LinearLayout(this)
        linearLayout.orientation = LinearLayout.VERTICAL

        for (i in 0 until 5) {
            val cardView = CardView(this)
            cardView.layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT, 500
            )
            cardView.radius = 16f
            cardView.cardElevation = 8f

            val graphView = GraphView(this)
            val series = LineGraphSeries<DataPoint>()
            for (j in 0..10) {
                series.appendData(DataPoint(j.toDouble(), Math.sin(j.toDouble())), true, 10)
            }
            graphView.addSeries(series)

            graphView.layoutParams = LinearLayout.LayoutParams(
                LinearLayout.LayoutParams.MATCH_PARENT, 500
            )

            cardView.addView(graphView)

            linearLayout.addView(cardView)
        }

        scrollView.addView(linearLayout)

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


