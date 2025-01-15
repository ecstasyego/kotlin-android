## Examples
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
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.os.Bundle
import android.widget.SeekBar
import android.widget.Toast
import androidx.activity.ComponentActivity

class MainActivity : ComponentActivity() {
    lateinit var widget:SeekBar

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        widget = SeekBar(this)
        setContentView(widget)

        widget.setOnSeekBarChangeListener(object : SeekBar.OnSeekBarChangeListener {
            override fun onProgressChanged(seekBar: SeekBar?, progress: Int, fromUser: Boolean) {
                Toast.makeText(this@MainActivity, "Progress: $progress", Toast.LENGTH_SHORT).show()
            }

            override fun onStartTrackingTouch(seekBar: SeekBar?) {
                Toast.makeText(this@MainActivity, "Start", Toast.LENGTH_SHORT).show()
            }

            override fun onStopTrackingTouch(seekBar: SeekBar?) {
                Toast.makeText(this@MainActivity, "End", Toast.LENGTH_SHORT).show()
            }
        })

    }
}
```
