## Intent
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import android.widget.*
import android.content.Intent

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout) // res/layout/main_layout.xml

        val widget = findViewById<[InputWidgetClass]>(R.id.[WidgetID]) // *.xml
        widget.setClickable(true)
        widget.setOnClickListener{
            val intent = Intent(this, ResultActivity::class.java)
            intent.putExtra("height", 50.0)
            intent.putExtra("width", 200.0)
            startActivity(intent)
        }
    }
}

class ResultActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_result) // res/layout/activity_result.xml

        val height = intent.getIntExtra("height", 0)
        val width = intent.getIntExtra("width", 0)
        Log.d("ResultActivity", "height : $height , width : $width")

        val widget = findViewById<[OutputWidgetClass]>(R.id.[WidgetID])
        widget.text = height * width  
    }
}
```

```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity
import android.widget.*
import android.content.Intent

class MainActivity : ComponentActivity() {
    private val widget: [InputWidgetClass] by lazy { findViewById<[InputWidgetClass]>(R.id.[WidgetID]) }  // *.xml

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout) // res/layout/main_layout.xml
        [inputWidget]()
        }

    private fun [inputWidget]() {
        widget.setClickable(true)
        widget.setOnClickListener{
            val intent = Intent(this, ResultActivity::class.java)
            intent.putExtra("height", 50.0)
            intent.putExtra("width", 200.0)
            startActivity(intent)
        }
    }
}

class ResultActivity: AppCompatActivity() {
    private val widget:[OutputWidgetClass] by lazy { findViewById<[OutputWidgetClass]>(R.id.[WidgetID]) }  // *.xml

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_result) // res/layout/activity_result.xml

        val height = intent.getIntExtra("height", 0)
        val width = intent.getIntExtra("width", 0)
        Log.d("ResultActivity", "height : $height , width : $width")

        widget.text = height * width  
    }
}
```


<br><br><br>

## Activity Type

`MainActivity.kt`: ComponentActivity
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}
```

`MainActivity.kt`: AppCompatActivity
```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}
```

### setContentView
```kotlin
package com.example.myapplication

import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity(R.layout.main_layout)
```

```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity(R.layout.main_layout) {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

```kotlin
package com.example.myapplication

import android.os.Bundle
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)
    }
}
```
