## PressTimer
### Example01: *.kt
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
└── build.gradle.kts # PROJECT-LEVEL
```

#### Source Code
`MainActivity.kt`
```kotlin
package com.example.myapplication

import android.annotation.SuppressLint
import android.os.Bundle
import android.os.Handler
import android.os.Looper
import android.view.MotionEvent
import android.widget.ProgressBar
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    private lateinit var progressBar: ProgressBar
    private lateinit var elapsedTimeText: TextView
    private var startTime: Long = 0
    private var isPressed = false
    private val handler = Handler(Looper.getMainLooper())  // Handler tied to main looper
    private var elapsedTime: Long = 0
    private val updateTimeRunnable = object : Runnable {
        override fun run() {
            // Calculate elapsed time
            elapsedTime = System.currentTimeMillis() - startTime

            // Update the progress bar every 0.01 second (1000 * 10ms = 10 seconds max)
            val progress = (elapsedTime / 10).toInt() // Convert milliseconds to 0.01 second intervals
            progressBar.progress = progress

            // Update the elapsed time text (formatted to seconds with 2 decimal places)
            elapsedTimeText.text = "Elapsed Time: %.2f seconds".format(elapsedTime / 1000.0)

            // Call the runnable again in 10 milliseconds if still pressed
            if (isPressed) {
                handler.postDelayed(this, 10)
            }
        }
    }

    @SuppressLint("ClickableViewAccessibility")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.main_layout)

        // Initialize views
        progressBar = findViewById(R.id.progressBar)
        elapsedTimeText = findViewById(R.id.elapsedTimeText)

        // Set up ProgressBar
        progressBar.max = 1000 // Set maximum value to represent 10 seconds (1000 * 10ms = 10 seconds)

        // Set up touch listener for detecting press events
        progressBar.setOnTouchListener { _, event ->
            when (event.action) {
                MotionEvent.ACTION_DOWN -> {
                    // User starts pressing, begin tracking time
                    if (!isPressed) {
                        startTime = System.currentTimeMillis()
                        isPressed = true
                        handler.post(updateTimeRunnable) // Start updating progress bar
                    }
                    true
                }
                MotionEvent.ACTION_UP, MotionEvent.ACTION_CANCEL -> {
                    // User releases press, stop tracking time
                    if (isPressed) {
                        isPressed = false
                        handler.removeCallbacks(updateTimeRunnable) // Stop updating progress bar
                    }
                    true
                }
                else -> false
            }
        }
    }

    override fun onStop() {
        super.onStop()
        handler.removeCallbacks(updateTimeRunnable) // Clean up handlers when the activity stops
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

    <ProgressBar
        android:id="@+id/progressBar"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="100dp"
        android:layout_marginStart="32dp"
        android:layout_marginEnd="32dp"
        android:max="1000"
        android:progress="0"
        android:indeterminate="false"
        style="@android:style/Widget.ProgressBar.Horizontal"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/elapsedTimeText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Elapsed Time: 0 seconds"
        android:textSize="18sp"
        app:layout_constraintTop_toBottomOf="@id/progressBar"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginTop="16dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
