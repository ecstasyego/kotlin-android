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
import android.view.ViewGroup
import androidx.appcompat.app.AppCompatActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = GraphAdapter(generateGraphData())

        setContentView(recyclerView)
    }

    private fun generateGraphData(): List<List<DataPoint>> {
        val data = mutableListOf<List<DataPoint>>()
        for (i in 0 until 10) {
            val series = mutableListOf<DataPoint>()
            for (j in 0..10) {
                series.add(DataPoint(j.toDouble(), Math.sin(j.toDouble() + i)))
            }
            data.add(series)
        }
        return data
    }
}


class GraphAdapter(private val graphData: List<List<DataPoint>>) : RecyclerView.Adapter<GraphAdapter.GraphViewHolder>() {

    override fun getItemCount(): Int = graphData.size

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): GraphViewHolder {
        val graphView = GraphView(parent.context)
        graphView.layoutParams = ViewGroup.LayoutParams(
            ViewGroup.LayoutParams.MATCH_PARENT,
            500
        )
        return GraphViewHolder(graphView)
    }

    override fun onBindViewHolder(holder: GraphViewHolder, position: Int) {
        val series = LineGraphSeries<DataPoint>(graphData[position].toTypedArray())
        holder.graphView.addSeries(series)
    }

    class GraphViewHolder(val graphView: GraphView) : RecyclerView.ViewHolder(graphView)
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


