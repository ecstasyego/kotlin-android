## PressGraph
### Example01: Coroutine
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

import android.annotation.SuppressLint
import android.os.Bundle
import android.view.MotionEvent
import android.widget.ProgressBar
import android.widget.TextView
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import androidx.appcompat.app.AppCompatActivity
import kotlinx.coroutines.*

class MainActivity : AppCompatActivity() {
    private lateinit var graphView: GraphView
    private lateinit var progressBar: ProgressBar
    private lateinit var elapsedTimeText: TextView

    private var startTime: Long = 0
    private var isPressed = false
    private var elapsedTime: Long = 0
    private val series = LineGraphSeries<DataPoint>()
    private var timeIndex: Double = 0.0
    private var job: Job? = null

    @SuppressLint("ClickableViewAccessibility")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        graphView = findViewById(R.id.graph)
        progressBar = findViewById(R.id.progressBar)
        elapsedTimeText = findViewById(R.id.elapsedTimeText)

        // Set up GraphView
        graphView.addSeries(series)
        graphView.viewport.isScalable = true
        graphView.viewport.isScrollable = true
        graphView.viewport.setXAxisBoundsManual(true)
        graphView.viewport.setMinX(0.0)
        graphView.viewport.setMaxX(10.0) // Set to 10 seconds for example
        graphView.viewport.setMinY(0.0)
        graphView.viewport.setMaxY(1.0)

        progressBar.max = 1000 // Set max value for 10 seconds (1000 * 10ms = 10 seconds)

        graphView.setOnTouchListener { _, event ->
            when (event.action) {
                MotionEvent.ACTION_DOWN -> {
                    if (!isPressed) {
                        startTime = System.currentTimeMillis()
                        isPressed = true
                        startUpdatingGraph() // Start updating graph
                    }
                    true
                }
                MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                    if (isPressed) {
                        isPressed = false
                        stopUpdatingGraph() // Stop updating graph
                    }
                    true
                }
                else -> false
            }
        }
    }

    private fun startUpdatingGraph() {
        job = CoroutineScope(Dispatchers.Main).launch {
            while (isPressed) {
                // Calculate elapsed time
                elapsedTime = System.currentTimeMillis() - startTime
                val progress = (elapsedTime / 10) // Update every 0.01 second

                // Update the progress bar
                progressBar.progress = progress.toInt()

                // Update the elapsed time text
                elapsedTimeText.text = "Elapsed Time: ${elapsedTime / 1000.0} seconds"

                // Update the graph
                series.appendData(DataPoint(timeIndex, elapsedTime / 1000.0), true, 1000) // 1000 points max
                timeIndex += 0.01

                // Delay for 10ms before the next update
                delay(10)
            }
        }
    }

    private fun stopUpdatingGraph() {
        job?.cancel() // Stop the coroutine when the user releases the press
    }

    override fun onStop() {
        super.onStop()
        job?.cancel() // Cancel the coroutine when the activity is stopped
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <!-- GraphView for the graph display -->
    <com.jjoe64.graphview.GraphView
        android:id="@+id/graph"
        android:layout_width="0dp"
        android:layout_height="300dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"/>

    <!-- TextView to display the elapsed time -->
    <TextView
        android:id="@+id/elapsedTimeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Elapsed Time: 0"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@id/graph"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        android:layout_marginTop="16dp"/>

    <!-- ProgressBar to show progress -->
    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toBottomOf="@id/elapsedTimeText"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        android:layout_centerHorizontal="true"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```


`build.gradle.kts(APP-LEVEL)`
```kotlin
dependencies {
    implementation("com.jjoe64:graphview:4.2.2")
}
```



`gradle.properties`
```
android.useAndroidX=true
android.enableJetifier=true
```
