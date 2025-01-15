
## Examples
- https://github.com/jjoe64/GraphView/wiki/Documentation

### Example01: Basic
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

import android.content.Context
import android.os.Bundle
import android.util.AttributeSet
import androidx.activity.ComponentActivity
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import kotlin.random.Random

class MainActivity : ComponentActivity() {
    lateinit var widget:CustomGraphView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomGraphView(this)
        setContentView(widget)
    }
}

class CustomGraphView @JvmOverloads constructor(
    context: Context, attrs: AttributeSet? = null, defStyle: Int = 0
) : GraphView(context, attrs, defStyle) {
    init {
        val series = LineGraphSeries<DataPoint>()
        for (i in 0 until 10) {
            val x = i.toDouble()
            val y = Random.nextDouble(-3.0, 3.0)
            series.appendData(DataPoint(x, y), true, 10)
        }

        this.addSeries(series)
        this.viewport.isScalable = true
        this.viewport.isScrollable = true
        this.gridLabelRenderer.isHorizontalLabelsVisible = false
        this.gridLabelRenderer.isVerticalLabelsVisible = true
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


<br>


### Example02: Dynamic Layout
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

import android.content.Context
import android.os.Bundle
import android.util.AttributeSet
import androidx.activity.ComponentActivity
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import kotlin.random.Random

class MainActivity : ComponentActivity() {
    lateinit var widget:CustomGraphView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = CustomGraphView.newInstance(this)
        setContentView(widget)
    }
}

class CustomGraphView @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyle: Int = 0) : GraphView(context, attrs, defStyle) {
    companion object {
        fun newInstance(context: Context): CustomGraphView {
            return CustomGraphView(context)
        }
    }

    init {
        val series = LineGraphSeries<DataPoint>()
        for (i in 0 until 10) {
            val x = i.toDouble()
            val y = Random.nextDouble(-3.0, 3.0)
            series.appendData(DataPoint(x, y), true, 10)
        }

        this.addSeries(series)
        this.viewport.isScalable = true
        this.viewport.isScrollable = true
        this.gridLabelRenderer.isHorizontalLabelsVisible = false
        this.gridLabelRenderer.isVerticalLabelsVisible = true
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


<br>


### Example03: Static Layout
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

import android.content.Context
import android.os.Bundle
import android.util.AttributeSet
import androidx.activity.ComponentActivity
import com.jjoe64.graphview.GraphView
import com.jjoe64.graphview.series.DataPoint
import com.jjoe64.graphview.series.LineGraphSeries
import kotlin.random.Random

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}

class CustomGraphView @JvmOverloads constructor(context: Context, attrs: AttributeSet? = null, defStyle: Int = 0) : GraphView(context, attrs, defStyle) {
    init {
        val series = LineGraphSeries<DataPoint>()
        for (i in 0 until 10) {
            val x = i.toDouble()
            val y = Random.nextDouble(-3.0, 3.0)
            series.appendData(DataPoint(x, y), true, 10)
        }

        this.addSeries(series)
        this.viewport.isScalable = true
        this.viewport.isScrollable = true
        this.gridLabelRenderer.isHorizontalLabelsVisible = false
        this.gridLabelRenderer.isVerticalLabelsVisible = true
    }
}
```

`main_layout.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<com.example.myapplication.CustomGraphView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/customGraphView"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

</com.example.myapplication.CustomGraphView>
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









