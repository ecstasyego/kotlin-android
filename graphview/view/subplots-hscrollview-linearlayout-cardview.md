
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
import android.os.Bundle
import android.widget.HorizontalScrollView
import android.widget.LinearLayout
import androidx.appcompat.app.AppCompatActivity
import androidx.cardview.widget.CardView
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val horizontalScrollView = HorizontalScrollView(this)
        horizontalScrollView.isHorizontalScrollBarEnabled = true

        val linearLayout = LinearLayout(this)
        linearLayout.orientation = LinearLayout.HORIZONTAL

        for (i in 0 until 6) {
            val cardView = CardView(this)
            cardView.layoutParams = LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.MATCH_PARENT)
            cardView.radius = 16f
            cardView.cardElevation = 8f

            val graphView = GraphView(this)
            val series = LineGraphSeries<DataPoint>()
            for (j in 0..10) {
                series.appendData(DataPoint(j.toDouble(), Math.sin(j.toDouble())), true, 10)
            }
            graphView.addSeries(series)

            val params = LinearLayout.LayoutParams(1000, 500)  // width, height
            graphView.layoutParams = params

            cardView.addView(graphView)

            linearLayout.addView(cardView)
        }

        horizontalScrollView.addView(linearLayout)

        setContentView(horizontalScrollView)
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


