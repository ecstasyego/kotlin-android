
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
import androidx.activity.viewModels
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.Observer
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import android.view.ViewGroup
import android.widget.LinearLayout
import androidx.cardview.widget.CardView

class MainActivity : AppCompatActivity() {

    private val viewModel: GraphViewModel by viewModels()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        val recyclerView = RecyclerView(this)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = GraphAdapter()

        viewModel.graphData.observe(this, Observer { data ->
            (recyclerView.adapter as GraphAdapter).submitList(data)
        })

        setContentView(recyclerView)
    }
}



class GraphAdapter : RecyclerView.Adapter<GraphAdapter.GraphViewHolder>() {

    private var data: List<List<DataPoint>> = emptyList()

    fun submitList(newData: List<List<DataPoint>>) {
        data = newData
        notifyDataSetChanged()
    }

    override fun getItemCount(): Int = data.size

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): GraphViewHolder {
        val cardView = CardView(parent.context)
        cardView.layoutParams = ViewGroup.LayoutParams(
            ViewGroup.LayoutParams.MATCH_PARENT,
            ViewGroup.LayoutParams.WRAP_CONTENT
        )
        cardView.radius = 16f
        cardView.cardElevation = 8f

        val linearLayout = LinearLayout(parent.context)
        linearLayout.orientation = LinearLayout.VERTICAL
        cardView.addView(linearLayout)

        return GraphViewHolder(cardView, linearLayout)
    }

    override fun onBindViewHolder(holder: GraphViewHolder, position: Int) {
        val graphView = GraphView(holder.linearLayout.context)
        graphView.layoutParams = ViewGroup.LayoutParams(
            ViewGroup.LayoutParams.MATCH_PARENT,
            500
        )

        val series = LineGraphSeries<DataPoint>(data[position].toTypedArray())

        graphView.addSeries(series)

        holder.linearLayout.addView(graphView)
    }

    class GraphViewHolder(val cardView: CardView, val linearLayout: LinearLayout) : RecyclerView.ViewHolder(cardView)
}

class GraphViewModel : ViewModel() {

    private val _graphData = MutableLiveData<List<List<DataPoint>>>()
    val graphData: LiveData<List<List<DataPoint>>> get() = _graphData

    init {
        loadGraphData()
    }

    private fun loadGraphData() {
        val data = mutableListOf<List<DataPoint>>()
        for (i in 0 until 10) {
            val series = mutableListOf<DataPoint>()
            for (j in 0..10) {
                series.add(DataPoint(j.toDouble(), Math.sin(j.toDouble() + i)))
            }
            data.add(series)
        }
        _graphData.value = data
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


